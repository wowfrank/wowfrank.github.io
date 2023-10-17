---
layout: post
title: "Data Governance Framework Summary"
date: "2023-10-17 01:09:00 +0800"
description: "Data Governance Framework Summary" # (optional)
img: "2023-10-17-cover-image-data-governance-summary.png" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['Data Management']
categories: ['Data Management']
---

> Regards to Deniel Vitaver, who is the best professor I've experienced in my AI program at GBC.
> The only pity is that it lasted only 20 days, but the professor tried his best to keep us learning.

## Data Management 

### Principles

1. Data is an asset with unique properties
2. Data value should be expressed in economic terms
3. Managing data means managing the quality of data
4. It takes metadata to manage data
5. It takes planning to manage data (architecture, processes, etc.)
6. Data management is cross-functional, requires skills, expertise and leadership vision, commitment, purpose
7. Data management requires an enterprise perspective and requires data governance program to be effective
8. Data management is lifecycle management
9. Different types of data have different lifecycle characteristics
10. Data management includes risk associated with data
11. Data management requirements must drive IT decisions – Data is dynamic, it constantly evolves

### DAMA Wheel

1. Data Governance
1. Metadata
1. Data Quality
1. Data Architecture
1. Data Modelling & Design
1. Data Storage & Operations
1. Data Security
1. Data Integration & Interoperability
1. Document & Content Management
1. Reference & Master Data
1. Data Warehouse & Business Intelligence

## Data Governance

Data Governance is a system of rights and accountabilities, and the enforcement of authority over the management of all data and data-related assets of the organization.

### Strategic Alignment

1. Accountability: roles and responsibilities in the defined organizational chart
1. Data Inventory: data stewards to maintain the inventory of all data using metadata tools
1. Business Processes: establish processes for managing data in massive storage and data analysis tools
1. Policies, Procedures and Rules: define data protection, classification, compliance and security

### Principles

1. Leadership and Strategy
1. Business-driven
1. Shared responsibility
1. Multi-layered
1. Framework-based
1. Principle-based

## Metadata Management

### Principles

1. Organizational commitment
1. Strategy
1. Enterprise perspective
1. Socialization
1. Access
1. Quality
1. Audit
1. Improvement

![Metadata Repository MetaModel]({{site.baseurl}}/assets/img/2023-10-17/metadata-repositiory-metamodel.png)

## Reference & Master Data

### Principles

1. Shared Data
1. Ownership
1. Quality
1. Stewardship
1. Controlled Change
1. Authority

## Data Modelling & Design

### Principles

1. Formalization: A data model documents a concise definition of data structures and relationships. It enables assessment of how data is affected by implemented business rules, for current as-is states or desired target states. Formal definition imposes a disciplined structure to data that reduces the possibility of data anomalies occurring when accessing and persisting data. By illustrating the structures and relationships in the data, a data model makes data easier to consume.
1. Scope Definition: A data model can help explain the boundaries for data context and implementation of purchased application packages, projects, initiatives, or existing systems.
1. Knowledge retention/documentation: A data model can preserve corporate memory regarding a system or project by capturing knowledge in an explicit form. It serves as documentation for future projects to use as the as-is version. Data models help us understand an organization or business area, an existing application, or the impact of modifying an existing data structure. The data mode becomes a reusable map to help business professionals, project managers, analysts, modellers, and developers understand data structure within the environment. In much the same way as mapmaker learned and documented a geographic landscape for others to use for navigation, the modeller enables others to understand an information landscape.

## Data Architecture

### Business Processes

1. A process is a set of interrelated actions and activities performed to achieve a pre-specified product, result, or service
1. Each process is characterized by its input, the tools and techniques that can be applied, and the resulting outputs
1. Each process can be broken down into other processes, and so on, until we reach a level in which simple activities are defined using measurable inputs and measurable outputs

1. Level 0 (“The Context”) shows the relationship between the main process and stakeholders
1. Level 1: Level 0 is broken down into major processes
1. Level 2: Level 1 is broken down into other processes/activities
1. Level n: level n-1 is broken down into measurable tasks

### Readiness Assessment/Risk Assessment

1. Lack of management support: Any reorganization of the enterprise during the planned execution of the project will affect the architecture process. For example, new decision makers may question the process and be tempted to withdraw from opportunities for participants to continue their work on the Data Architecture. It is by establishing support among management that an architecture process can survive reorganization. Therefore, be certain to enlist into the Data Architecture development process more than one member of top-level management, or at least senior management, who understand the benefits of Data Architecture.
1. No proven record of accomplishment: Having a sponsor is essential to the success of the effort, as is his or her confidence in those carrying out the Data Architecture function. Enlist the help of a senior architect colleague to help carry out the most important steps.
1. Apprehensive sponsor: If the sponsor requires all communication to pass through them, it may be an indication that that person is uncertain of their role, has interests other than the objectives of the Data Architecture process, or is uncertain of the data architect's capability. Regardless of the reason, the sponsor must allow the project manager and data architect to take the leading roles in the project. Try to establish independence in the workplace, along with the sponsor's confidence.
1. Counter-productive executive decision: It may be the case that although management understands the value of a well-organized Data Architecture, they do not know how to achieve it. Therefore, they may make decisions that counteract the data architect's efforts. This is not a sign of disloyal management but rather an indication that the data architect needs to communicate more clearly or frequently with management.
1. Culture shock: Consider how the working culture will change among those who will be affected by the Data Architecture. Try to imagine how easy or difficult it will be for the employees to change their behaviour within the organization.
1. Inexperienced project leader: Make sure that the project manager has experience with Enterprise Data Architecture particularly if the project has a heavy data component. If this is not the case, encourage the sponsor to change or educate the project manager
Dominance of a one-dimensional view: Sometimes the owner of one business application might tend to dictate their view about the overall enterprise-level Data Architecture at the expense of a more well-balanced, all-inclusive view.

## Data Integration & Interoperability

DII describes processes related to the movement and consolidation of data within and between data stores, applications and organizations. Integration consolidates data into consistent forms, either physical or virtual. Data interoperability is the ability for multiple systems to communicate.

### Goals

1. Make data available in the format and timeframe needed by data consumers, both human and system
1. Consolidate data physically and virtually into data hubs
1. Lower cost and complexity of managing solutions by developing shared models and interfaces
1. Identify meaningful events(opportunities and threats) and automatically trigger alerts and actions
1. Support Business Intelligence, analytics, Master Data Management, and operational efficiency efforts

### Principles

1. Take an enterprise perspective in design to ensure future scalability and extensibility
2. Balance local data needs with enterprise data needs
3. Ensure business accountability

## Data Storage & Operations

Data S&O includes the design, implementation and support of stored data, to maximize its value through its lifecycle, from creation/acquisition to disposal

### Goals

1. Managing the availability of data throughout the data lifecycle
1. Ensuring the integrity of data assets
1. Managing the performance of data transactions

### Principles

1. Identify and act on automation opportunities: automate database development processes, developing tools, and processes that shorten each development cycle, reduce errors and rework, and minimize the impact on the development team. In this way, DBAs can adapt to more iterative approaches to application development. This improvement work should be done in collaboration with data modelling and Data Architecture.
2. Build with reuse in mind: Develop and promote the use of abstracted and reusable data objects that prevent applications from being tightly coupled to database schemas. A number of mechanisms exist to this end, including database views, triggers, functions and stored procedures, application data objects and data-access layers, XML and XSLT, ADO.NET typed data sets, and web services. The DBA should be able to assess the best approach virtualizing data. The end goal is to make using the database as quick, easy, and painless as possible
3. Understand and appropriately apply best practices: DBAs should promote database standards and best practices as requirements, but be flexible enough to deviate from them if given acceptable reasons for these deviations. Database standards should never be a threat to the success of a project.
4. Utilize database standards to support requirements
5. Set expectations for the DBA role in project work

## Data Quality

The planning, implementation, and control of activities that apply quality management techniques to data, in order to assure it is fit for consumption and meets the needs of data consumers.

### Goals

1. Develop a governed approach to meet consumers’ DQ requirements
1. Define standards, requirements, specifications, processes, metrics for DQ Monitoring and DQ level control
1. Identify, advocate for opportunities to improve data quality

### Principles

1. Criticality: A DQ program should focus on the data most critical to the enterprise and its customers. Priorities for imporovement should be based on the criticality of the data and on the level of risk if data is not correct
1. Lifecycle Management: The quality of data should be managed across the data lifecycle, from creation or procurement through disposal. This includes managing data as it moves within and between systems.
1. Prevention: This focus of a DQ program should be on preventing data errors and conditions that reduce the usability of data; it should not be focus on simply correcting records.
1. Root cause remediation: Improving the quality of data goes beyond correcting errors. Problems with the quality of data should be understood and addressed at their root causes, rather than just their symptoms. Because these causes are often related to process or system design, improving data quality often requires changes to processes and the systems that support them.
1. Governance: Data Governance activities must support the development of high quality data and DQ program activities must support and sustain a governed data environment.
1. Standards-driven: All stakeholders in the data lifecycle have data quality requirements. To the degree possible, these requirements should be defined in the form of measurable standards and expectations against which the quality of data can be measured.
1. Objective measurement and transparency: Data quality levels need to be measured objectively and consistently. Measurements and measurement methodology should be shared with stakeholders since they are the arbiters of quality.
1. Embedded in business processes: Business process owners are responsible for the quality of data produced through their process. They must enforce data quality standards in their processes.
1. Connected to service levels: Data quality reporting and issues management should be incorporated into SLA.
1. Systematically enforced: System owners must systematically enforce data quality requirements.

### Policy

1. Purpose, scope and applicability of the policy
1. Definitions of terms
1. Responsibilities of the Data Quality program
1. Responsibilities of other stakeholders
1. Reporting
1. Implementation of the policy, including links to risk, preventative measures, compliance, data protection, and data security

## Data Security

Definition, planning, development, and execution of security policies and procedures to provide proper authentication, authorization access, and auditing of data and information assets

### Goals

1. Enabling appropriate, and preventing inappropriate access to data assets
2. Enabling compliance with regulations, policies for privacy, protection, and confidentiality
3. Ensuring that stakeholder requirements for privacy and confidentiality are met

### Principles

1. Collaboration
2. Enterprise approach
3. Proactive management
4. Clear accountability
5. Metadata-driven
6. Reduce risk by reducing exposure

## Data Privacy

The exercise of monitoring, and enforcement, and shared decision-making over privacy of Data

### Goals

1. Identify sensitive data
2. Flag sensitive data within the metadata repository
3. Address privacy laws and restrictions by country, state or province
4. Manage situation where personal data crosses international boundaries
5. Monitor access to sensitive data by privileged users

### Principles

1. Be Accountable. Be responsible, by contractual or other means, for all personal information under your control.
2. Identify the Purpose
3. Obtain Consent
4. Limit Collection
5. Limit Use, Disclosure and Retention
6. Be Accurate
7. Use Appropriate Safeguards
8. Be Open
9. Give Individuals Access
10. Provide Recourse

![Principles of Data Privacy]({{site.baseurl}}/assets/img/2023-10-17/principles-of-data-privacy.png)

## Document & Content Management

Planning, implementation, and control activities for lifecycle management of data and information found in any form or medium

### Goals

1. Comply with legal obligations, and customer expectations regarding Records management
2. Ensure effective, efficient storage, retrieval, use of documents and content
3. Ensure integration capabilities between structured, unstructured content

### Principles

1. Everyone must create, use, retrieve, and dispose of records in accordance with the established policies and procedures
2. Experts in handling of Records and Content should be fully engaged in policy and planning, and should comply with regulations and best practices

![E-Discovery Reference Model]({{site.baseurl}}/assets/img/2023-10-17/e-discovery-reference-model.png)