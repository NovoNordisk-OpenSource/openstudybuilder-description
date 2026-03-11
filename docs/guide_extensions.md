# Guide for Extensions in OpenStudyBuilder {: class="guideH1"}

(created 2025-03-10 using OSB 2.5)
{: class="guideCreated"}

## Introduction

The **Extensions framework** allows OpenStudyBuilder to be extended with new functionality without modifying the core codebase. New pages, API endpoints, navigation items, and business logic can be packaged as self-contained modules that are automatically discovered and loaded at runtime.

This makes it possible to develop and deploy additional capabilities - for specific integration needs, custom workflows, or specialized features - alongside the main platform without risking the stability of the core. Organizations and contributors can build on top of OpenStudyBuilder in a structured, contained way.

![New Extensions Framework for OpenStudyBuilder - Key Points Visualized](./img/guide_extensions_01.png)
{: class="imageParagraph"}

Figure 1: New Extensions Framework for OpenStudyBuilder
{: class="imageDescription"}

### What Extensions Can Do

**Frontend extensions** add entirely new views and navigation items into the OpenStudyBuilder UI. They are written in Vue.js and have access to the full set of existing UI components and design patterns. A frontend extension can appear as a new page under any section of the application - Studies, Library, or Administration.

**API extensions** define their own endpoints, data operations, and business logic. They are written in Python using FastAPI and are served under a dedicated `/extensions-api` path, with their own Swagger documentation. They integrate into the same infrastructure as the rest of the platform.

The two types are independent but complementary: a frontend extension typically calls an API extension for its data operations.

### Design Philosophy and Stability

The Extensions framework is intentionally kept flexible. The interfaces it exposes are not planned to be frozen, because rigidity would undermine the core goal of adaptability. This means:

- Frontend extensions can reuse all existing OSB UI components, but those components may change as the core evolves. Extensions that rely on internal UI elements may require updates.
- There is no component versioning, and none is planned.
- Anyone building extensions should treat this as a living foundation and plan for ongoing maintenance.

---

## Extensions Architecture

Extensions slot into two specific locations in the OpenStudyBuilder codebase:

| Extension type | Location in OSB source | Served at |
|---|---|---|
| Frontend | `studybuilder/src/extensions/<name>/` | Integrated into the UI |
| API | `clinical-mdr-api/extensions/<name>/` | `/extensions-api/` |

Both are **auto-discovered at startup** - no registration or wiring is required beyond placing the files in the correct directory.

**Feature flags** are a mechanism in OpenStudyBuilder to control the visibility of modules at runtime. They are used in the core application to show or hide entire functional areas - for example, compound management in the UI can be enabled or disabled via a feature flag. The same mechanism applies to extensions: a frontend extension can declare a feature flag in its router configuration, and it will only appear in the navigation when that flag is enabled. Whether a feature flag is used is optional and controlled by the extension itself - an extension that should always be visible simply omits the `featureFlag` property from its route definition.

**Frontend extensions do not require an API extension.** A frontend extension can connect to the existing OSB API directly, integrate with external systems, or display information that requires no backend at all. API extensions are only needed when the extension requires custom backend logic not available in the existing OSB API.

**API extensions do not require a frontend extension.** A standalone API extension is useful for creating new endpoints needed by downstream automation processes - for example, exporting OSB data in a specific format not currently supported, or providing an endpoint for another tool to trigger actions such as initial study setup in the database.

**Extension Scope.** API extensions run inside the same process as the OpenStudyBuilder extensions API, which means they can import and call OpenStudyBuilders internal Python service layer directly - the same services that power the native API. This gives extensions access to studies, controlled terminology, concepts, and all other domain objects without making HTTP calls back to the main API. The full scope of OSB business logic and data access is available as a Python import.

---

## For Developers: Creating Extensions

### Frontend Extension

**Structure**

A frontend extension is a Vue.js module placed in:

```
studybuilder/src/extensions/<extension-name>/
```

It is loaded automatically when the StudyBuilder development server starts or the application is built. The extension can define new routes, pages, and sidebar navigation entries, and has access to all existing OSB Vue components.

A minimal frontend extension consists of a router configuration, at least one Vue view component, a store, and localization files. The [Hello extension](https://github.com/NovoNordisk-OpenSource/openstudybuilder-solution/tree/main/studybuilder/src/extensions/hello){target=_blank} is the recommended starting point - its file structure reflects the minimum required. Detailed documentation of the expected structure is available in the Frontend Extensions [README](https://github.com/NovoNordisk-OpenSource/openstudybuilder-solution/blob/main/studybuilder/src/extensions/README.md){target=_blank}.

**Local development setup**

- Have a local OSB instance running (for example pulled or built from Docker images).
- In the `studybuilder/` directory, update the `.env` file - for example:

```
VUE_APP_API_BASE_URL=http://localhost:5005/api
VUE_APP_DOC_BASE_URL=http://localhost:5005/doc
```

- Update `public/config.json` with the correct URLs - for example:

```json
"API_BASE_URL": "http://localhost:5005/api",
"EXTENSIONS_API_BASE_URL": "http://localhost:5005/extensions-api",
"DOC_BASE_URL": "http://localhost:5005/doc"
```

- Install dependencies and start the development server:

```bash
yarn install
yarn dev
```

**Enabling a frontend extension via feature flag**

Whether a frontend extension is controlled by a feature flag depends on its router configuration. If the route's `meta` object includes a `featureFlag` property (as in the Hello extension), the page only appears when that flag is enabled in OSB. If the `featureFlag` property is omitted, the extension is always visible once loaded.

To enable a feature-flag-controlled extension, the flag must be created and set to true in OSB. There are two ways to do this:

*Option 1 - via the Swagger API* (e.g. at `http://localhost:5005/api/docs`):

Navigate to **Feature Flags → POST**, click **Try it out**, and submit:

```json
{
  "name": "<extension-name>",
  "enabled": true,
  "description": "Description of the extension"
}
```

*Option 2 - via the Administration panel*: Add the feature flag to the initial data file `studybuilder-import/configuration/feature_flags.csv` and enable it in the Administration pane.

---

### API Extension

**Structure**

An API extension is a Python module placed in:

```
clinical-mdr-api/extensions/<extension-name>/
```

It should define a FastAPI router. The extensions API framework auto-discovers and mounts it under `/extensions-api`. The extension has access to OpenStudyBuilders internal service layer in-process and shares the same authentication middleware.

**Local development setup**

- In the `clinical-mdr-api/extensions/<extension-name>/` directory, copy `.env.example` to `.env` and adjust:

```
OAUTH_ENABLED=false
NEO4J_DSN=bolt://neo4j:changeme1234@localhost:5002/mdrdb
```

- If your extension depends on a separate Python package, install it into the extensions environment:

```bash
pipenv shell
pip install <package>
exit
```

- Start the extensions API development server:

```bash
cd clinical-mdr-api
pipenv run extensions-api-dev
```

The extension's endpoints will be available at `http://localhost:8009/<extension-name>/` and documented at `http://localhost:8009/docs`.

---

### Docker Setup

To run extensions within a locally running OSB Docker Compose stack:

1. Copy the API extension folder into `clinical-mdr-api/extensions/<extension-name>/`
2. Copy the frontend extension folder into `studybuilder/src/extensions/<extension-name>/`

If the API extension requires additional Python packages, add a `compose.override.yaml` to the root folder of the full OSB Solution to install them into the extensions API container:

```yaml
services:
  extensionsapi:
    command: >-
      sh -c "pipenv run pip install --quiet <package-name> && exec pipenv run uvicorn"
```

For a local package not yet published to PyPI, mount the source folder and install from the path instead:

```yaml
services:
  extensionsapi:
    volumes:
      - <install-path>/your-package:/tmp/ext-package:ro
    command: >-
      sh -c "pipenv run pip install --quiet /tmp/ext-package && exec pipenv run uvicorn"
```

Rebuild and restart the affected containers (remove old dockercontainers for extension and frontend and the corresponding images):

```bash
docker compose up -d --force-recreate --no-deps extensionsapi
docker compose up -d --force-recreate --no-deps frontend
```

---

### Testing

**API extensions** can be tested using the OSB test runner from the `clinical-mdr-api/` directory:

```bash
pipenv run extensions-test
```

This runs all tests found in `extensions/*/tests/`. Tests should be placed in a `tests/` subfolder within the extension directory. The Hello extension includes a working example of this structure.

**Frontend extensions** do not have a dedicated test runner as part of the extensions framework. Code quality checks (linting and formatting) can be run with:

```bash
yarn lint
yarn format
```

---

### Known Limitations and Considerations

**Component stability**

The core OSB UI components used by frontend extensions may change as the platform evolves. There is no versioning or compatibility guarantee. Extension developers should anticipate and plan for maintenance work when the core application is updated.

**Additional Python packages**

Installing extra dependencies into the extensions API Python environment requires explicit steps in version 2.5 of OpenStudyBuilder, either via `pip install` in a Docker override command or directly in the development environment. These dependencies are not declared in the main OSB project and must be managed separately per extension. Please checkout the extension documentation as this might change in future versions.

---

## Technical References

For detailed technical documentation on the extension APIs and file structure, refer to the official OpenStudyBuilder source repository:

- [Frontend Extensions README](https://github.com/NovoNordisk-OpenSource/openstudybuilder-solution/blob/main/studybuilder/src/extensions/README.md){target=_blank} - structure, routing, component access
- [API Extensions README](https://github.com/NovoNordisk-OpenSource/openstudybuilder-solution/blob/main/clinical-mdr-api/extensions/README.md){target=_blank} - FastAPI integration, environment configuration, testing

---

## Example Extensions

### Hello Extension

The **Hello extension** is a minimal "Hello World" example included directly in the OpenStudyBuilder core repository. It demonstrates the complete structure of both a frontend and an API extension in their simplest form.

- **Frontend**: adds a single page under the Studies section using the `hello_extension` feature flag. The view uses standard OSB UI components, serving as a clean starting template for new frontend extensions.
- **API**: exposes one endpoint (`GET /nodes-count`) that queries the Neo4j database via the OSB internal service layer and returns the total node count. It demonstrates how to wire up a FastAPI router, access the database, and apply OSB authentication and RBAC.

The source code is part of the official OpenStudyBuilder solution repository:

- [Frontend extension](https://github.com/NovoNordisk-OpenSource/openstudybuilder-solution/tree/main/studybuilder/src/extensions/hello){target=_blank}
- [API extension](https://github.com/NovoNordisk-OpenSource/openstudybuilder-solution/tree/main/clinical-mdr-api/extensions/hello){target=_blank}

---

### ELOBS Word Updater

The **ELOBS Word Updater** is a full-featured extension that auto-populates content controls in Word protocol templates with live study data from OpenStudyBuilder - no Microsoft Office required.

It is structured as three independent parts: a standalone Python CLI (`core`), a backend API extension, and a frontend extension. The frontend adds a page under Studies where users select a study, choose a version, upload a Word template, and download the populated document. The API extension wraps the core library as a single FastAPI endpoint.

The source code and documentation are available in [GitHub](https://github.com/KatjaGlassConsulting/elobs-word-updater){target=_blank}.

---

## Notes on extension licenses

!!! warning

    The following reflects general open-source understanding and is **not legal advice**. If you are building extensions for commercial or third-party use, consult a lawyer familiar with open-source licensing.

OpenStudyBuilder is licensed under **GPLv3**, which is a copyleft license. GPLv3 copyleft obligations are triggered when you **distribute** software - not when you use it internally. This shapes how extensions can be licensed depending on how and to whom they are shared.

**Recommended: open-source licenses compatible with GPLv3**

The simplest and safest approach is to release extensions under an open-source license compatible with GPLv3, such as MIT, Apache 2.0, or GPLv3 itself. This is what the example extensions in this guide do.

**Internal / private use within one organization**

GPLv3 does not restrict private use. An organization can develop and run proprietary extensions for internal purposes without any obligation to publish the source code. The copyleft only applies when the software is distributed to others outside the organization.

**Distributing a proprietary extension to a third party**

This is where GPLv3 copyleft becomes relevant. OSB API extensions integrate tightly with the OSB internal service layer - they run in-process and import OSB Python modules directly. Frontend extensions embed into the OSB Vue.js application and reuse its components. Both forms of integration are likely to be considered a combined work with the GPLv3-licensed OSB code. Distributing such a combined work to a third party would generally require making the source available under GPLv3 terms, which is incompatible with keeping the extension proprietary.

**Building at the customer's site**

A possible approach for organizations wanting to deliver proprietary functionality is to provide the extension source code to the customer and have the customer build and run it themselves. In this case, the customer is not receiving a distributed binary - they are the ones building and using it internally. This may preserve more flexibility, but the boundaries here are not clearly established and depend on the specifics of the arrangement.

**Providing a prebuilt extension binary**

Distributing a compiled or packaged extension that forms a combined work with GPLv3 code is considered distribution under GPLv3. The source would need to be made available under GPL-compatible terms.