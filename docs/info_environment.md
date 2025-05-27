# Environments

## Pre-Requisites

To run the OpenStudyBuilder, you will need the following pre-requisites:

![OpenStudyBuilder Pre-requisites (Neo4j and a CDISC Library API Key)](./img/info_environment_01.png)

The current implementation required **CDISC library access** via a library API key to access CDISC standards (freely available also for non-CDISC-members). This is used to receive CDISC standards and processed in the MDR-Standards-Import scripts which load for example new CDISC Controlled Terminology into the OpenStudyBuilder.

As OpenStudyBuilder is using a Neo4j graph database, a **Neo4j database license** is required. We highly recommend using the enterprise or AuraDB license which comes along with services and features recommended. It is also possible to use the GPLv3 licensed community edition which is available at no cost. OpenStudyBuilder can also run on the community edition, but some features like consistency checks are not be available. The migration scripts we provide to update the database schema from an old to a new version will also not be running and would need manual updatings.

## Deployment Options

The OpenStudyBuilder is a modular solution consisting of several components, including a Neo4j database, a web application, and an API and more. The system can be deployed in various ways, depending on your needs and preferences. Access to the open-source code is available via [GitHub](https://github.com/NovoNordisk-OpenSource/openstudybuilder-solution){target=_blank}. The repository contains [installation instructions](https://github.com/NovoNordisk-OpenSource/openstudybuilder-solution/blob/main/README.md){target=_blank} using docker. Furthermore, each component has its own readme file with installation instructions and can be installed independently.

Option | Description
--|--
Local Desktop | The OpenStudyBuilder can be run on a local desktop environment
Server Installation | OpenStudyBuilder can be setup to run on a server
Cloud | The OpenStudyBuilder can be deployed on all major cloud platforms
Hosted Cloud Environment | The OpenStudyBuilder can be hosted by a vendor
Sandbox | A public sandbox environment is available for evaluation purposes

Access to a cloud-based or a dedicated evaluation environment can be provided by Neo4j free of charge. Please check the following section for more information.

## Neo4j Licenses

The OpenStudyBuilder docker setup is using the Neo4j Enterprise version. This can be used for evaluation and test purposes. To use OpenStudyBuilder in production, either purchase an Enterprise version or switch to the community edition (see [here](./info_osb_hub.md#neo4j-community-edition)).

Neo4j offers a number of commercial licensing options, including free licenses for development, startups, academic-educational uses and of course, evaluation.

## Neo4j Evaluation Environments

To evaluate the OpenStudyBuilder, Neo4j can provide you with an evaluation environment in different flavors, cloud-based and leveraging AuraDB.

This environment can be provided to you in different ways:

Type | Access | Complete initial dataset | Custom data | Usable for production | Pricing
--|--|--|--|--|--
Sandbox | Public | No | No - creating elements is possible, but this environment will be refreshed periodically | No | Free
Custom sandbox | Restricted | Optionally | Yes | No | Free - Limited time
Dedicated | Restricted | Yes | Yes | Optionally | See contact details below

All of the above can be made available in a modular fashion, for instance:

1. Neo4j AuraDB database + OpenStudyBuilder App and API
2. Neo4j AuraDB database + OpenStudyBuilder API (build your own Study Builder app on top of it)
3. Neo4j AuraDB database only (for exploration purposes)

Please note that Neo4j AuraDB exists in 3 flavors :

1. Free - Free forever, with limited number of nodes and relationships
2. Pro - Pay as you go, for medium scale applications
3. Enterprise - Advanced security and support

To learn more, visit the [AuraDB website here](https://neo4j.com/cloud/platform/aura-graph-database/), or see contact details below.

## Neo4j Contact details

For more information about the evaluation environment, please use the following contact information:

- Jan Aertsen - Professional Services EMEA
- +32(485)329828 & +39(339)8702150
- <a href="mailto:openstudybuilder@neotechnology.com">openstudybuilder@neotechnology.com</a>

## Public Sandbox Access

The public sandbox provides a convenient way for anyone to explore and test OpenStudyBuilder without the need to install any software. To request access to the sandbox environment, simply send an email to <a href="mailto:openstudybuilder@neotechnology.com?subject=Request%20Sandbox%20Access">openstudybuilder@neotechnology.com (Request Sandbox Access)</a>. 

The solution can then be accessed via the following link: [OpenStudyBuilder Sandbox](https://openstudybuilder.northeurope.cloudapp.azure.com/){target=_blank}.

Please be aware that the sandbox is refreshed periodically, which means any data you create will be lost after a refresh. Additionally, all data in the sandbox is publicly accessible to anyone with sandbox access. For transparency and traceability, all data entries are tracked with the user's email address and are visible to everyone using the sandbox. Therefore, please avoid entering any sensitive or confidential information. The sandbox is intended for evaluation and learning purposes only. 