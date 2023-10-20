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

### Business Drivers

1. enable organizations to get value from their datasets

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

### Business Drivers

1. reduce risk
1. improve processes


**Data policies** are directives that codify principles and management intent into fundamental rules governing the creation, acquisition, integrity, security, quality, and use of data and information.

1. **Data Architecture**: Enterprise data models, tool standards, and system naming conventions
1. **Data Modelling & Design**: Data model management procedures, data modelling naming conventions, definition standards, standard domains, and standard abbreviations.
1. **Data Storage and Operations**: Tool standards, standards for database recovery and business continuity, database performance, data retention, and external data acquisition.
1. **Data Security**: Data access security standards, monitoring and audit procedures, storage security standards, and training requirements
1. **Data integration and Interoperability**: Standard methods and tools used for data integration and interoperability.
1. **Documents and Content**: Content management standards and procedures, including use of enterprise taxonomies, support for legal discovery, document and email retention periods, electronic signatures, and report distribution approaches.
1. **Reference and Master Data**: Reference Data Management control procedures, systems of data records, assertions establishing and mandating use, standards for entity resolution.
1. **Data Warehousing and Business Intelligence**: Tool standard, processing standards and procedures, report and visualization formatting standards, standards for Big Data handling.
1. **Metadata**: Standard business and technical Metadata to be captured, Metadata integration procedures and usage
1. **Data Quality**: Data quality rules, standard measurement methodologies, data remediation standards and procedures
1. **Big Data and Data Science**: Data source Identification authority, acquisition, system of record, sharing and refresh.

### Strategic Alignment

1. **Accountability**: roles and responsibilities in the defined organizational chart
1. **Data Inventory**: data stewards to maintain the inventory of all data using metadata tools
1. **Business Processes**: establish processes for managing data in massive storage and data analysis tools
1. **Policies, Procedures and Rules**: define data protection, classification, compliance and security

### Principles

1. **Leadership and Strategy**: Successful Data Governance starts with visionary and committed leadership. Data management activities are guided by a data strategy that is itself driven by the enterprise business strategy.
1. **Business-driven**: Data Governance is a business program, and, as such, must govern IT decisions related to data as much as it governs business interaction with data.
1. **Shared responsibility**: Across all Data Management Knowledge Areas, data governance is a shared responsibility between business data stewards and technical data management professionals.
1. **Multi-layered**: Data governance occurs at both the enterprise and local levels and often at levels in between.
1. **Framework-based**: because data governance activities require coordination across functional areas, the DG program must establish an operating framework that defines accountabilities and interactions.
1. **Principle-based**: Guiding principles are the foundation of DG activities, and especially of DG policy. Often, organizations develop policy without formal principles - they are trying to solve particular problems. Principles can sometimes be reverse-engineered from policy. However, it is best to articulate a core set of principles and best practices as part of policy work. Reference to principles can mitigate potential resistance. Additional guiding principles will emerge over time within an organization. Publish them in a shared internal environment along with other data governance artifacts.

![Data Governance Organization Parts]({{site.baseurl}}/assets/img/2023-10-17/data-governance-organization-parts.png)

### Metrics

1. Value
1. Effectiveness
1. Sustainability

## Metadata Management

### Business Drivers

1. increase confidence in data by providing context and enabling the measurement of data quality
1. increase the value of strategic information by enabling multiple uses
1. improve operational efficiency by identifying redundant data and processes
1. prevent the use of out-of-date or incorrect data
1. reduce data-oriented research time
1. improve communication between data consumers and IT professionals
1. create accurate impact analysis thus reduce the risk of project failure
1. improve time-to-market by reducing system development life-cycle time
1. reduce training cost and lower the impact turnover through thorough documentation of data context, history, and origin
1. support regulatory compliance

Metadata includes information about technical and business processes, data rules and constraints, and logical and physical data structures. It describes the data itself(e.g., databases, data elements, data models), the concepts the data represents (e.g., business processes, application systems, software code, technology infrastructure), and the connections (relationships) between the data and concepts. Metadata helps an organization understand its data, its systems, and its workflows.

A **Metadata Repository** refers to the physical tables in which the Metadata is stored.

![Metadata Repository MetaModel]({{site.baseurl}}/assets/img/2023-10-17/metadata-repository-metamodel.png)

### Types

1. Business Metadata
1. Technical Metadata
1. Operational Metadata

### Principles

1. **Organizational commitment** Secure organizational commitment (senior management support and funding) to Metadata management as part of an overall strategy to manage data as an enterprise asset.
1. **Strategy**: Develop a Metadata strategy that accounts for how Metadata will be created, maintained, integrated, and accessed. The strategy should drive requirements, which should be defined before evaluating, purchasing, and installing Metadata management products. The Metadata strategy must align with business priorities.
1. **Enterprise perspective**: Take an enterprise perspective to ensure future extensibility, but implement through iterative and incremental delivery to bring value.
1. **Socialization**: Communicate the necessity of Metadata and the purpose of each type of Metadata; socialization of the value of Metadata will encourage business use and, as importantly, the contribution of business expertise.
1. **Access**: Ensure staff members know how to access and use Metadata.
1. **Quality \| Accountability**: Recognize that Metadata is often produced through existing processes (data modelling, SDLC, business process definition) and hold process owners accountable for the quality of Metadata.
1. **Audit**: Set, enforce, and audit standards for Metadata to simplify integration and enable use.
1. **Improvement \| Standards**: Create a feedback mechanism so that consumers can inform the Metadata Management team of Metadata that is incorrect or out-of-date.

![Metadata Repository MetaModel]({{site.baseurl}}/assets/img/2023-10-17/metadata-repositiory-metamodel.png)

### Metrics

1. **Metadata repository completeness**: Compare ideal coverage of the enterprise oMetadata to actual coverage.
1. **Metadata Management Maturity**:
1. **Stweard reprensentation**
1. **Metadata usage**
1. **Business Glossary activity**
1. **Master Data service data compliance**
1. **Metadata documentation quality**
1. **Metadata repository availability**

## Reference & Master Data

1. **Master Data Management**: entails control over Master Data values and identifiers that enable consistent use, across systems, of the most accurate and timely data about essential business entities. The goals of MDM include ensuring availability of accurate, current values while reducing risks associated with ambiguous identifiers (those identified with more than one instance of an entity and those that refer to more than one entity).
1. **Reference Data Management**: entails control over defined domain values and their definition. The goal of RDM is to ensure the organization has access to a complete set of accurate and current values for each concept represented.

![Key Processing Steps For MDM]({{site.baseurl}}/assets/img/2023-10-17/key-processing-steps-for-mdm.png)

### Business Drivers

1. meet organizational data requirements
1. manage data quality
1. manage the costs of data integration
1. reduce risk

### Principles

1. **Shared Data**
1. **Ownership**
1. **Quality**
1. **Stewardship**
1. **Controlled Change**
1. **Authority**

## Data Modelling & Design

### Business Drivers

1. provide common vocabulary
1. capture and document explicit knowledge about an organization's data and systems
1. server as a primary communications tool during projects
1. provide the starting point for customization, integraion, or even replacement of an application

### Principles

1. **Formalization**: A data model documents a concise definition of data structures and relationships. It enables assessment of how data is affected by implemented business rules, for current as-is states or desired target states. Formal definition imposes a disciplined structure to data that reduces the possibility of data anomalies occurring when accessing and persisting data. By illustrating the structures and relationships in the data, a data model makes data easier to consume.
1. **Scope Definition**: A data model can help explain the boundaries for data context and implementation of purchased application packages, projects, initiatives, or existing systems.
1. **Knowledge retention/documentation**: A data model can preserve corporate memory regarding a system or project by capturing knowledge in an explicit form. It serves as documentation for future projects to use as the as-is version. Data models help us understand an organization or business area, an existing application, or the impact of modifying an existing data structure. The data mode becomes a reusable map to help business professionals, project managers, analysts, modellers, and developers understand data structure within the environment. In much the same way as mapmaker learned and documented a geographic landscape for others to use for navigation, the modeller enables others to understand an information landscape.

### 1NF (First Normal Form) Rules

- A table is referred to as being in its First Normal Form if atomicity of the table is 1.
- Here, **atomicity** states that a single cell cannot hold multiple values. It must hold only a single-valued attribute.
- The First normal form disallows the multi-valued attribute, composite attribute, and their combinations.

example:

| roll_no |name | course | age |  gender |
|:---|:---|:---|:---|:---|
|  1 |  Rahul |  c/c++ | 22  | m  |
|  2 |  Harsh |  java/php |  24 | f  |
|  3 |  Sahil |  python/c++ |  19 | f  |
|  4 |  Adam |  javascript |  40 |  m |

In the students record table, you can see that the course column has two values. Thus it does not follow the First Normal Form. Now, if you use the First Normal Form to the above table, you get the below table as a result.

### 2NF (Second Normal Form) Rules

- The first condition for the table to be in Second Normal Form is that the table has to be in First Normal Form.
- The table should not possess partial dependency.
- The partial dependency here means the proper subset of the candidate key should give a non-prime attribute.

example:

| cust_id  | store_id  | store_location  |
|:---|:---|:---|
|  1 | D1  |  Toronto |
|  2 |   D3|  Miami |
|  3 | T1  |  Florida |

The Location table possesses a composite primary key cust_id, storeid. The non-key attribute is store_location. In this case, store_location only depends on storeid, which is a part of the primary key. Hence, this table does not fulfill the second normal form.

### 3NF (Third Normal Form) Rules

- The first condition for the table to be in Third Normal Form is that the table should be in the Second Normal Form.
- The second condition is that there should be no transitive dependency for non-prime attributes, which indicates that non-prime attributes (which are not a part of the candidate key) should not depend on other non-prime attributes in a table. Therefore, a transitive dependency is a functional dependency in which A → C (A determines C) indirectly, because of A → B and B → C (where it is not the case that B → A).
- The third Normal Form ensures the reduction of data duplication. It is also used to achieve data integrity.

| stu_id  |  name | sub_id  |  sub | address |
|:---|:---|:---|:---|:---|
| 1  | Arun  |  11 | SQL  | Delhi  |
|  2 | Varun  | 12  |  Jave | Bangalore  |
| 3  |  Harsh |  21 | Python  | Delhi  |
| 4  | Keshav  | 21  | Python  |  Kochi |

In the above student table, stu_id determines subid, and subid determines sub. Therefore, stu_id determines sub via subid. This implies that the table possesses a transitive functional dependency, and it does not fulfill the third normal form criteria.

### Boyce CoddNormal Form (BCNF)

- The first condition for the table to be in Boyce Codd Normal Form is that the table should be in the third normal form. 
- Secondly, every Right-Hand Side (RHS) attribute of the functional dependencies should depend on the super key of that particular table.

|  stu_id |  subject |  professor |
|:---|:---|:---|
|  1 |  Python |  Prof. Mishra |
|  2 |  C++ |  Prof. Anand |
|  3 |  JAVA | Prof. Kanth  |
|  4 | DBMS  | Prof. James  |

The subject table follows these conditions:

- Each student can enroll in multiple subjects.
- Multiple professors can teach a particular subject.
- For each subject, it assigns a professor to the student.

In the above table, student_id and subject together form the primary key because using student_id and subject; you can determine all the table columns.

Another important point to be noted here is that one professor teaches only one subject, but one subject may have two professors.

Which exhibit there is a dependency between subject and professor, i.e. subject depends on the professor's name.

to

The table is in 1st Normal form as all the column names are unique, all values are atomic, and all the values stored in a particular column are of the same domain.

The table also satisfies the 2nd Normal Form, as there is no Partial Dependency.

And, there is no Transitive Dependency; hence, the table also satisfies the 3rd Normal Form.

This table follows all the Normal forms except the Boyce Codd Normal Form.

As you can see stuid, and subject forms the primary key, which means the subject attribute is a prime attribute.

However, there exists yet another dependency - professor → subject.

BCNF does not follow in the table as a subject is a prime attribute, the professor is a non-prime attribute.

To transform the table into the BCNF, you will divide the table into two parts. One table will hold stuid which already exists and the second table will hold a newly created column profid.

|  stu_id   | pro_id  |
|:---|:---|
|  1 |  3 |
|  2 |  1 |
|  3 |  3 |
|  4 |  5 |

|  pro_id |  subject |  professor |
|:---|:---|:---|
|  1 |  Python |  Prof. Mishra |
|  2 |  C++ |  Prof. Anand |
|  3 |  JAVA | Prof. Kanth  |
|  4 | DBMS  | Prof. James  |


## Data Architecture

### Business Drivers

1. strategically prepare organizations to quickly evovle their products, services, and data to take advantage of business opportunities inherent in emerging technologies
1. translate business needs into data and system requirements so that processes consistently have the data they require
1. manage complex data and information delivery throughout the enterprise
1. facilitate alignment between business and IT
1. act as an agent for change, transformation, and agility

### Business Processes

1. A process is a set of interrelated actions and activities performed to achieve a pre-specified product, result, or service
1. Each process is characterized by its input, the tools and techniques that can be applied, and the resulting outputs
1. Each process can be broken down into other processes, and so on, until we reach a level in which simple activities are defined using measurable inputs and measurable outputs

1. Level 0 (“The Context”) shows the relationship between the main process and stakeholders
1. Level 1: Level 0 is broken down into major processes
1. Level 2: Level 1 is broken down into other processes/activities
1. Level n: level n-1 is broken down into measurable tasks

### Readiness Assessment/Risk Assessment

1. **Lack of management support**: Any reorganization of the enterprise during the planned execution of the project will affect the architecture process. For example, new decision makers may question the process and be tempted to withdraw from opportunities for participants to continue their work on the Data Architecture. It is by establishing support among management that an architecture process can survive reorganization. Therefore, be certain to enlist into the Data Architecture development process more than one member of top-level management, or at least senior management, who understand the benefits of Data Architecture.
1. **No proven record of accomplishment**: Having a sponsor is essential to the success of the effort, as is his or her confidence in those carrying out the Data Architecture function. Enlist the help of a senior architect colleague to help carry out the most important steps.
1. **Apprehensive sponsor**: If the sponsor requires all communication to pass through them, it may be an indication that that person is uncertain of their role, has interests other than the objectives of the Data Architecture process, or is uncertain of the data architect's capability. Regardless of the reason, the sponsor must allow the project manager and data architect to take the leading roles in the project. Try to establish independence in the workplace, along with the sponsor's confidence.
1. **Counter-productive executive decision**: It may be the case that although management understands the value of a well-organized Data Architecture, they do not know how to achieve it. Therefore, they may make decisions that counteract the data architect's efforts. This is not a sign of disloyal management but rather an indication that the data architect needs to communicate more clearly or frequently with management.
1. **Culture shock**: Consider how the working culture will change among those who will be affected by the Data Architecture. Try to imagine how easy or difficult it will be for the employees to change their behaviour within the organization.
1. **Inexperienced project leader**: Make sure that the project manager has experience with Enterprise Data Architecture particularly if the project has a heavy data component. If this is not the case, encourage the sponsor to change or educate the project manager
Dominance of a one-dimensional view: Sometimes the owner of one business application might tend to dictate their view about the overall enterprise-level Data Architecture at the expense of a more well-balanced, all-inclusive view.

## Data Integration & Interoperability

DII describes processes related to the movement and consolidation of data within and between data stores, applications and organizations. Integration consolidates data into consistent forms, either physical or virtual. Data interoperability is the ability for multiple systems to communicate.

### Business Drivers

1. the need to manage data movement efficiently

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

### Business Drivers

1. business continuity

### Goals

1. Managing the availability of data throughout the data lifecycle
1. Ensuring the integrity of data assets
1. Managing the performance of data transactions

### Principles

1. **Identify and act on automation opportunities**: automate database development processes, developing tools, and processes that shorten each development cycle, reduce errors and rework, and minimize the impact on the development team. In this way, DBAs can adapt to more iterative approaches to application development. This improvement work should be done in collaboration with data modelling and Data Architecture.
2. **Build with reuse in mind**: Develop and promote the use of abstracted and reusable data objects that prevent applications from being tightly coupled to database schemas. A number of mechanisms exist to this end, including database views, triggers, functions and stored procedures, application data objects and data-access layers, XML and XSLT, ADO.NET typed data sets, and web services. The DBA should be able to assess the best approach virtualizing data. The end goal is to make using the database as quick, easy, and painless as possible
3. **Understand and appropriately apply best practices**: DBAs should promote database standards and best practices as requirements, but be flexible enough to deviate from them if given acceptable reasons for these deviations. Database standards should never be a threat to the success of a project.
4. **Utilize database standards to support requirements**
5. **Set expectations for the DBA role in project work**

A **distributed system's components** can be classified depending on the autonomy of the component systems into two types: **federated (autonomous)** or **non-federated (non-autonomous)**

Federation provisions data without additional persistence or duplication of source data. A federated database system maps multiple autonomous database systems into a single federated database. The constituent  databases, sometimes geographically separated, are interconnected via a computer network. They remain autonomous yet participate in a federation to allow partial and controlled sharing of their data. Federation provides an alternative to merging disparate databases. There is no actual data integration in the constituent database because of data federation; instead, data interoperability manages the view of the federated databases as one large object. In contrast, a non-federated database system is an integration of component DBMS's that are not autonomous; they aer controlled, managed and governed by a centralized DBMS.

## Data Quality

The planning, implementation, and control of activities that apply quality management techniques to data, in order to assure it is fit for consumption and meets the needs of data consumers.

### Business Drivers

1. increase the value of organizational data and opportunities to use it
1. reduce risk and cost associate with poor data quality
1. improve organizational efficiency and productivity
1. protect and enhance the organization's reputation

### Goals

1. Develop a governed approach to meet consumers’ DQ requirements
1. Define standards, requirements, specifications, processes, metrics for DQ Monitoring and DQ level control
1. Identify, advocate for opportunities to improve data quality

### Principles

1. **Criticality**: A DQ program should focus on the data most critical to the enterprise and its customers. Priorities for improvement should be based on the criticality of the data and on the level of risk if data is not correct
1. **Lifecycle Management**: The quality of data should be managed across the data lifecycle, from creation or procurement through disposal. This includes managing data as it moves within and between systems.
1. **Prevention**: This focus of a DQ program should be on preventing data errors and conditions that reduce the usability of data; it should not be focus on simply correcting records.
1. **Root cause remediation**: Improving the quality of data goes beyond correcting errors. Problems with the quality of data should be understood and addressed at their root causes, rather than just their symptoms. Because these causes are often related to process or system design, improving data quality often requires changes to processes and the systems that support them.
1. **Governance**: Data Governance activities must support the development of high quality data and DQ program activities must support and sustain a governed data environment.
1. **Standards-driven**: All stakeholders in the data lifecycle have data quality requirements. To the degree possible, these requirements should be defined in the form of measurable standards and expectations against which the quality of data can be measured.
1. **Objective measurement and transparency**: Data quality levels need to be measured objectively and consistently. Measurements and measurement methodology should be shared with stakeholders since they are the arbiters of quality.
1. **Embedded in business processes**: Business process owners are responsible for the quality of data produced through their process. They must enforce data quality standards in their processes.
1. **Connected to service levels**: Data quality reporting and issues management should be incorporated into SLA.
1. **Systematically enforced**: System owners must systematically enforce data quality requirements.

### Policy

1. Purpose, scope and applicability of the policy
1. Definitions of terms
1. Responsibilities of the Data Quality program
1. Responsibilities of other stakeholders
1. Reporting
1. Implementation of the policy, including links to risk, preventative measures, compliance, data protection, and data security

## Data Security

Definition, planning, development, and execution of security policies and procedures to provide proper authentication, authorization access, and auditing of data and information assets

### Business Drivers

1. risk reduction
1. business growth
1. security as an asset

### Goals

1. Enabling appropriate, and preventing inappropriate access to data assets
2. Enabling compliance with regulations, policies for privacy, protection, and confidentiality
3. Ensuring that stakeholder requirements for privacy and confidentiality are met

### Principles

1. **Collaboration**: Data Security is a collaborative effort involving IT security administrators, data stewards/data governance, internal and external audit teams, and the legal department.
2. **Enterprise approach**: Data Security standards and policies must be applied consistently across the entire organization.
3. **Proactive management**: Success in data security management depends on being proactive and dynamic, engaging all stakeholders, managing change, and overcoming organizational or cultural bottlenecks such as traditional separation of responsibilities between information security, information technology, data administration, and business stakeholders.
4. **Clear accountability**: Roles and responsibilities must be clearly defined, including the 'chain of custody' for data across organizations and roles.
5. **Metadata-driven**: Security classification for data elements is an essential part of data definitions.
6. **Reduce risk by reducing exposure**: Minimize sensitive/confidential data proliferation, especially to non-production environments.

### Security Processes

4 A's

1. Access
1. Audit
1. Authentication
1. Authorization
1. entitlement

## Data Privacy

The exercise of monitoring, and enforcement, and shared decision-making over privacy of Data

### Goals

1. Identify sensitive data
2. Flag sensitive data within the metadata repository
3. Address privacy laws and restrictions by country, state or province
4. Manage situation where personal data crosses international boundaries
5. Monitor access to sensitive data by privileged users

### Principles

1. **Be Accountable. Be responsible, by contractual or other means, for all personal information under your control**.
2. **Identify the Purpose**
3. **Obtain Consent**
4. **Limit Collection**
5. **Limit Use, Disclosure and Retention**
6. **Be Accurate**
7. **Use Appropriate Safeguards**
8. **Be Open**
9. **Give Individuals Access**
10. **Provide Recourse**

![Principles of Data Privacy]({{site.baseurl}}/assets/img/2023-10-17/principles-of-data-privacy.png)

## Document & Content Management

Planning, implementation, and control activities for lifecycle management of data and information found in any form or medium.

Document and Content Management entails controlling the capture, storage, access, and use of data and information stored outside relational databases.

Its focus is on maintaining the quality, security, integrity of and enabling access to documents and other **unstructured** information.

### Business Drivers

1. regulatory compliance
1. ability to respond to litigation and e-discovery

### Goals

1. Comply with legal obligations, and customer expectations regarding Records management
2. Ensure effective, efficient storage, retrieval, use of documents and content
3. Ensure integration capabilities between structured, unstructured content

### Principles

1. Everyone must create, use, retrieve, and dispose of records in accordance with the established policies and procedures
2. Experts in handling of Records and Content should be fully engaged in policy and planning, and should comply with regulations and best practices

**E-discovery** is the process of finding electronic records that might serve as evidence in a legal action. As the technology for creating, storing, and using data has developed, the volume of ESI has increased exponentially. Some of this data will undoubtedly end up in litigation or regulatory requests.

![E-Discovery Reference Model]({{site.baseurl}}/assets/img/2023-10-17/e-discovery-reference-model.png)

Volume -> Relevance

As e-discovery progresses, the volume of discoverable data and information is greatly reduced as their relevance is greatly increased.

