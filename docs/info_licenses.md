# Licenses used in OpenStudyBuilder {: class="guideH1"}

(created 2026-01-26) 
{: class="guideCreated"}

OpenStudyBuilder is built as an open, collaborative ecosystem. Different components serve different purposes - from lightweight helper scripts to complex core processes and features - therefore different open-source licenses are used.

A full list of licenses for our components can be found in the repository [here](https://github.com/NovoNordisk-OpenSource/openstudybuilder-solution/blob/main/LICENSE.md){target=_blank}.

This page explains how and why licenses are applied in OpenStudyBuilder, and what this means for users, contributors, and organizations building on top of it. 

Rather than applying a single license to everything, OpenStudyBuilder follows a component-based licensing model.

---

## Our licensing philosophy

We decided that OpenStudyBuilder should be an open-source project to enable:

- open collaboration across companies and vendors  
- sustainable community contributions  
- safe industrial adoption  
- and transparent, auditable software foundations  

We intentionally use a dual approach for the components in OpenStudyBuilder:

- **MIT License** for simple, reusable, extension-oriented components  
- **GPLv3 License** for core modules that embody most of the logic and behavior  

This combination supports both wide reuse and experimentation, and long-term protection of the OpenStudyBuilder core as a shared open asset.

---

## What this means for organizations and contributors

OpenStudyBuilder is designed to be used, extended, and embedded into real operational environments - in pharma and CRO companies, by vendors, academia and in other collaborative initiatives. You are explicitly encouraged to:

- **Use OpenStudyBuilder in research and operations** - for internal operationalization, pilots, production systems, and innovation initiatives.  
- **Build tools, automations, and integrations around it** - including integrations to sponsor or commercial tools, commercial offerings, internal services, and workflows.  
- **Extend and customize both MIT- and GPL-licensed components** - to fit your technical and organizational needs.

### Working with MIT-licensed components

MIT-licensed parts are intentionally lightweight and permissive. You may:

- reuse them in other projects  
- modify and redistribute them  
- embed them into proprietary or open systems  

The only obligation is to retain the original copyright and license notice in your distributions.

### Working with GPLv3 licensed components

GPL-licensed components are also explicitly meant to be used, configured, extended, and integrated - including in productive and commercial environments. Currently, core components such as the user interface, API, and industry standards import scripts are released under GPLv3.

When GPL-covered components (or derivatives of them) are **redistributed**, the GPL requires that:

- recipients receive the same rights to use, study, modify, and share the software  
- the corresponding source code is made available  
- and the GPL license is preserved.

The intention is that improvements to OpenStudyBuilder core components remain part of the shared ecosystem and continue to benefit the broader community.

The GPLv3 requires you to share modifications only if you distribute the modified software to others outside your organization. If you use a modified version strictly internally within your organization and do not distribute it externally, you are not required to publish or share your modifications. Furthermore, GPLv3 obligations apply to the software source code only. They do not apply to the study content, metadata, or other data created with OpenStudyBuilder.

---

## Video overview

This video explains what licenses are, why they matter, which licenses apply to OpenStudyBuilder, and what GPLv3 means in the context of OpenStudyBuilder.

<iframe
  title="Licenses in OpenStudyBuilder"
  width=720
  height=410
  src="https://www.youtube-nocookie.com/embed/diMhCTd7pEY"
  frameBorder="0"
  allow="accelerometer; encrypted-media; gyroscope; picture-in-picture"
  allowFullScreen
></iframe>

---

## Legal notice and third-party prerequisites

This page is provided for informational purposes only and does not constitute legal advice.  
For project-specific or organization-specific questions, please consult your legal or compliance experts.

Please note that the licenses described on this page apply to OpenStudyBuilder components only.

OpenStudyBuilder uses Neo4j as an underlying database technology.  
The Neo4j software itself is not part of OpenStudyBuilder and is licensed separately under its own terms and conditions.

Using OpenStudyBuilder therefore requires a compatible Neo4j license as a prerequisite.  
It is the responsibility of each organization to ensure compliance with the applicable Neo4j licensing model for their intended usage.


