# About the Project {: class="guideH1"}

(created 2026-06-23) 
{: class="guideCreated"}

## Overview

<iframe
  title="OpenStudyBuilder in a Nutshell"
  width=720
  height=400
  src="https://www.youtube.com/embed/X0UQrOGfEOc"
  frameBorder="0"
  allow="accelerometer; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
></iframe>

## Problem

The current solutions for managing clinical data standards and study design specifications are often characterised by:

*	Resource demanding double work
*	Parallel work done in silos resulting in discrepancies and need for rework 
*	Many handovers between different professional groups introducing lag-time
*	Many IT systems and tools
*	Error-prone and resource-intensive manual steps 
*	Expensive system maintenance and integration
    
Additionally, the current CDISC standards have gaps in standards metadata, limiting automation opportunities. The inherent flexibility provided by the standards supports a broad range of implementations, but that flexibility also allows for inconsistencies that makes automation scaling difficult. Furthermore, the lack of end-to-end awareness and use of the CDISC standards (e.g. for writing documents such as the protocol and clinical study reports) prohibits efficient end-to-end automation and introduces discrepancies between different presentations of the study specifications, such as e.g. discrepancies in terminology used between protocol, CRF and SDTM datasets.       

The vision is to solve this problem by implementing standards as linked metadata with a conceptual foundation providing the additional semantics needed to support metadata driven-automation across the end-to-end clinical research data lifecycle – as illustrated by the CDISC 360 PoC project (see more at [https://www.cdisc.org/cdisc-360](https://www.cdisc.org/cdisc-360){target=_blank}).

## Solution

The OpenStudyBuilder is a new approach to working with studies that once fully implemented will drive end-to-end consistency and more efficient processes - all the way from protocol development and CRF design - to creation of datasets, analysis, reporting, submission to health authorities and public disclosure of study information.

The OpenStudyBuilder contains:

* Standards and templates used for study specification
* Individual versioning of standard elements in the library 
* Real-time team collaboration around study design by study team
* Central storage of study design data for multiple down-stream usages ('one source of truth')
* Versioning and full audit trail of study definitions

The OpenStudyBuilder will cover the following study specification elements:

* Study purpose (objectives, endpoints)
* Population (disease area, indication, sex, age, etc)
* Selection criteria (eligibility, randomisation, dosing, treatment discontinuation, etc)
* Study type (interventional, observational, etc)
* Study design (randomisation, blinding, arms, etc)
* Interventions (drug, dose, route, devise, lifestyle interventions, etc.)
* Visit schedule (naming, timing, type, windows)
* Activities and assessments (what, how, when)
* All the terminology and syntax standards to follow
* Complete audit trail of what has changed over time
    
The OpenStudyBuilder consists of:

* The OpenStudyBuilder app (web-based user interface)
* The new clinical Metadata Repository (central repository for all study specification data)
* The API layer (allowing interoperability with other systems)
* The integrations to source and target systems
    
The conceptual design for the OpenStudyBuilder system with connected system integrations is illustrated in the following diagram.

![Diagram of conceptual design](./img/studyBuilderDesign.png)

* **OpenStudyBuilder** online documentation for the OpenStudyBuilder solution including introduction, user guides, system documentation and data model documentation.
* **OpenStudyBuilder App** Vuetify based Web application with the UI for creating the study definition specification.
* **Protocol Metadata Word add-in** holding the Protocol Template and import features of the structured study specification metadata that relates to the protocol content.
* **Up-stream integrations** to clinical systems like CTMS, Trial Supplies, EDC, Study Registries, etc. supporting also DDF API.
* **Down-stream integrations** to clinical data systems for SDTM, ADaM, analysis and reporting, supporting also DDF API.
* **Explore data** Using a biomedical concept used by a FAIR based study search and explore tool utilising the OpenStudyBuilder metadata with reference to systems holding study data.
* **OpenStudyBuilder API** and Standards Library API Python based web application based on FAST API framework supporting all CRUD actions to the database, access control, versioning, workflows and data integrity rules.
* **DDF API Adaptor** providing a Digital Data Flow compliant API connectivity enabling the Clinical MDR solution as a compliant Study Definition Repository (SDR) solution supporting the Unified Study Definition Model (USDM) standard data model as well.
* **Integration Service** Integration to UNIX based Statistical Computing Environment (SCE) with SAS and R.
* **Clinical MDR** Neo4j linked graph database and data model supporting the library standards, study definitions including fine granularity of versioning, audit trail, workflows and access control.
* **Standards Management** for integrated into the OpenStudyBuilder App as the Library module managing concepts, dictionaries, code lists, syntax templates, project and TA standards.


## System Components and Licenses

The OpenStudyBuilder system component architecture including software components and packages is illustrated in the following diagram.

![Diagram of component design](./img/studyBuilderComponents.png)

Core components are shared under MIT and applications are shared under the GPLv3 copyleft license. The Clinical MDR and OpenStudyBuilder system consist of the following components:

System Component (License) | Technology | Description
-- | -- | --
OpenStudyBuilder App (GPLv3) | Vue.js using Vuetify library | JavaScript based web application with the UI for creating the study definition specification, maintaining library standards. The OpenStudyBuilder app holds two main modules: Library and Studies
OpenStudyBuilder Documentation Portal (CC-BY-4.0 and MIT) | Vuepress | Markdown based documentation portal with OpenStudyBuilder Introduction, User Guides, System Documentation, Data Models and more.
Clinical MDR API (GPLv3) | Python using FAST API framework | Python based web application based on FAST API framework supporting all CRUD actions to the database, access control, versioning, workflows and data integrity rules.
Clinical MDR API Specification (MIT) | OpenAPI Specification / Swagger | Off-line documentation of the API.
Clinical MDR (MIT) | Cypher | Clinical MDR logical and physical data models, database constraint definitions, procedures and functions. Needs to run on a Neo4j database.
Graph Database (Neo4j free edition or licenced enterprise edition) | Neo4j native graph database (not part of repository, usable through neo4j.com) | A labelled property graph database engine, for more info see [Graph database concepts - Getting Started (neo4j.com)](https://neo4j.com/docs/getting-started/current/graphdb-concepts/) and [Neo4j Graph Platform - Developer Guides](https://neo4j.com/developer/)
Standards Import (GPLv3) | Python and Cypher | Import programs connecting to CDISC Library, downloading files to cloud storage, reading these into staging database, and then inserting data into Clinical MDR database.
Data Import (MIT) | Python and Cypher | Importing other data like sponsor specific data standards as well as sample and test data.

For full details on open-source strategy and rationale, see [Open Source](./info_open_source.md). For license details, see [Licenses](./info_licenses.md).


## Contributors

Contributions are much appreciated. Please check out the [contribution page](./info_contribution.md) and the community homepage to figure out how to contribute best.

The following companies are currently contributing to the system development:

Company | Website and contact | Description of contribution
-- | -- | --
Novo Nordisk | www.novonordisk.com <br> Henrik Lynge, Vice President, BDP MARS, GD, DRD <br> hlyl@novonordisk.com | Funding and license owner for the Clinical MDR and OpenStudyBuilder open-source project. Project leadership, product owner, system design, system development, cloud architecture, system documentation, system validation, …
Neo4j | www.neo4j.com <br> Jan Aertsen - Professional Services EMEA <br> openstudybuilder@neotechnology.com | Main solution vendor supporting data modelling, system design, system development, cloud architecture and cloud manage services.<br/><br/><ul><li>Documentation Portal</li><li>API Specification </li><li>API service </li><li>Data Model </li><li>Database</li></ul>
Katja Glass Consulting | www.glacon.eu <br/>Katja Gla&szlig;<br/>Consultant<br/>info@glacon.eu| Community Manager. Open-source project consulting, communication and support.
EvidentIQ  | www.evidentiq.com <br> J.David Renzelmann, Executive Director Business Development <br> david.renzelmann@evidentiq.com| Past Contribution<br/>Main solution vendor supporting system design, system documentation, system validation, open-source project support and services.
Microsoft | www.microsoft.com <br> Tianna M. Umann, Business Growth & Innovation Architect <br> Tianna.Umann@microsoft.com | Past Contribution<br/>Technical and architectural design review. 


## Background

This solution is based on a legacy bespoke MDR system at Novo Nordisk, and one of the goals for Novo Nordisk is to ensure that our current sponsor-defined data standards and portfolio of study definitions can be migrated into the new MDR and OpenStudyBuilder system.

The new solution design for the OpenStudyBuilder is deeply inspired by the CDISC 360 PoC project as well as the TransCelerate BioPharma Digital Data Flow (DDF) project. The OpenStudyBuilder aims to become a DDF reference implementation on top of many other features. Furthermore, additional community involvement is appreciated. Additional information how the OpenStudyBuilder can be seen in the DDF context is available [here](./info_ddf.md).

Currently the OpenStudyBuilder solution is only sponsored by Novo Nordisk. Novo Nordisk seeks additional stakeholders that would be interested in co-sponsoring this open-source project.

The first minimal viable product release for PoC was started in Q3 2022. The initial business go-live for the minimal viable product was Q4 2023.
