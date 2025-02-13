Title: Data Mesh Architecture

URL Source: https://www.datamesh-architecture.com/

Markdown Content:
🚀 NEW: We launched Data Mesh Manager to build a data product inventory and manage data contracts. 🚀
WHY
WHAT
HOW
SPECIFICATIONS
OPEN SOURCE
TECH STACKS
START THE JOURNEY
DATA MESH MANAGER

DATA MESH
ARCHITECTURE

Data Mesh From an Engineering Perspective

Why You May Need a Data Mesh

Many organizations have invested in a central data lake and a data team with the expectation to drive their business based on data. However, after a few initial quick wins, they notice that the central data team often becomes a bottleneck. The team cannot handle all the analytical questions of management and product owners quickly enough. This is a massive problem because making timely data-driven decisions is crucial to stay competitive. For example: Is it a good idea to offer free shipping during Black Week? Do customers accept longer but more reliable shipping times? How does a product page change influence the checkout and returns rate?

The data team wants to answer all those questions quickly. In practice, however, they struggle because they need to spend too much time fixing broken data pipelines after operational database changes. In their little time remaining, the data team has to discover and understand the necessary domain data. For every question, they need to learn domain knowledge to give meaningful insights. Getting the required domain expertise is a daunting task.

On the other hand, organizations have also invested in domain-driven design, autonomous domain teams (also known as stream-aligned teams or product teams) and a decentralized microservice architecture. These domain teams own and know their domain, including the information needs of the business. They design, build, and run their web applications and APIs on their own. Despite knowing the domain and the relevant information needs, the domain teams have to reach out to the overloaded central data team to get the necessary data-driven insights.

With the eventual growth of the organization, the situation of the domain teams and the central data team becomes worse. A way out of this is to shift the responsibility for data from the central data team to the domain teams. This is the core idea behind the data mesh concept: Domain-oriented decentralization for analytical data. A data mesh architecture enables domain teams to perform cross-domain data analysis on their own and interconnects data, similar to APIs in a microservice architecture.

What Is Data Mesh?

The term data mesh was coined by Zhamak Dehghani  in 2019 and is based on four fundamental principles that bundle well-known concepts:

The domain ownership principle mandates the domain teams to take responsibility for their data. According to this principle, analytical data should be composed around domains, similar to the team boundaries aligning with the system’s bounded context. Following the domain-driven distributed architecture, analytical and operational data ownership is moved to the domain teams, away from the central data team.

The data as a product principle projects a product thinking philosophy onto analytical data. This principle means that there are consumers for the data beyond the domain. The domain team is responsible for satisfying the needs of other domains by providing high-quality data. Basically, domain data should be treated as any other public API.

The idea behind the self-serve data infrastructure platform is to adopt platform thinking to data infrastructure. A dedicated data platform team provides domain-agnostic functionality, tools, and systems to build, execute, and maintain interoperable data products for all domains. With its platform, the data platform team enables domain teams to seamlessly consume and create data products.

The federated governance principle achieves interoperability of all data products through standardization, which is promoted through the whole data mesh by the governance group. The main goal of federated governance is to create a data ecosystem with adherence to the organizational rules and industry regulations.


Watch a video instead.
How To Design a Data Mesh?

A data mesh architecture is a decentralized approach that enables domain teams to perform cross-domain data analysis on their own. At its core is the domain with its responsible team and its operational and analytical data. The domain team ingests operational data and builds analytical data models as data products to perform their own analysis. It may also choose to publish data products with data contracts to serve other domains’ data needs.

The domain team agrees with others on global policies, such as interoperability, security, and documentation standards in a federated governance group, so that domain teams know how to discover, understand and use data products available in the data mesh. The self-serve domain-agnostic data platform, provided by the data platform team, enables domain teams to easily build their own data products and do their own analysis effectively. An enabling team guides domain teams on how to model analytical data, use the data platform, and build and maintain interoperable data products.

Let’s zoom in to the core components of a data mesh architecture and their relationships:

Data Product

A data product is a logical unit that contains all components to process and store domain data for analytical or data-intensive use cases and makes them available to other teams via output ports. You can think of a module or microservice, but for analytical data.

Data products connect to sources, such as operational systems or other data products and perform data transformation. Data products serve data sets in one or many output ports. Output ports are typically structured data sets, as defined by a data contract. Some examples:

A BigQuery dataset with multiple related tables
Parquet files in an AWS S3 bucket
Delta files in Azure Data Lake Storage Gen2
Messages in a Kafka topic

A data product is owned by a domain team. The team is responsible for the operations of the data product during its entire lifecycle. The team needs to continuously monitor and ensure data quality, availability, and costs.

To design data products, we recommend to use the Data Product Canvas.

To manage data products and track costs and compliance, consider using Data Mesh Manager.

Data Contract

A data contract is a document that defines the structure, format, semantics, quality, and terms of use for exchanging data between a data provider and their consumers. It covers:

Data Product Provider, including owner and the output port to access
Terms and conditions of data usage
Schema and semantics of provided data attributes
Quality attributes, such as freshness and number of rows
Service-level objectives, such as availability and support times
Billing details for using data

While a data contract represents the interface specification, the actual implementation that provides the data is the output port of a data product.

Data contracts come into play when data is exchanged between different teams or organizational units. First, and foremost, data contracts are a communication tool to express a common understanding of how data should be structured and interpreted. They make semantic and quality expectations explicit. Later in development and production, they also serve as the basis for code generation, testing, schema validations, quality checks, monitoring, access control, and computational governance policies. Data contracts can also be used for the input port for consumer-driven contract testing to verify that the data is provided as specified.

The Data Contract Specification defines a YAML format to describe the terms of use and attributes of provided data sets.

Federated Governance

The federated governance group is typically organized as a guild consisting of representatives of all teams taking part in the data mesh. They agree on global policies, which are the rules of play in the data mesh. These rules define how the domain teams have to build their data products.

Policies on interoperability are the starting point. They allow other domain teams to use data products in a consistent way. For example, global policies could define that the standard way to provide data is as a CSV file on AWS S3 in a bucket owned by the corresponding domain team.

Next, there has to be some form of documentation to discover and understand available data products. A simple policy for this could be a wiki page with a predefined set of metadata, such as owner of the data product, location URL, and descriptions of the CSV fields.

A uniform way to access the actual data product in a secure way could be using role-based access in AWS IAM, managed by the domain team.

Global policies such as privacy and compliance are also common. Think about protection of personally identifiable information (PII) or industry-specific legal requirements.

Lots of example policies are available on our other website datamesh-governance.com that you easily use in the Data Mesh Manager, our tool for data mesh governance.

Transformations

Diving into the organization of data within a data product, we can see the different kind of data that flows through different stages. Operational data is often ingested as some kind of raw and unstructured data.

In a preprocessing step, raw data is cleaned and structured into events and entities. Events are small, immutable, and highly domain oriented, such as OrderPurchased or ShipmentDelivered. Entities represent business objects such as shipments or articles with their state changing over time. That’s why the entities often are represented as a list of snapshots, the history, with the latest snapshot being the current state.

In practice, we often see manually entered or imported data. For example, forecast data sent via email as CSV files or text descriptions for business codes.

Data from other teams are integrated as external data. When using data products from other teams that are well-governed, this integration might be implemented in a very lightweight way. In case of importing data from legacy systems, the external area acts as an anti-corruption layer .

Aggregations combine data to answer analytical questions. Domain data can be published to other teams by defining a data contract. The data contract is usually implemented by a view, that is stable, even when the underlying data models change.

Ingesting

How can domain teams ingest their operational data into the data platform? A software system designed according to domain-driven design principles contains data as mutable entities/aggregates and immutable domain events.

Domain events are a great fit to be ingested into the data platform as they represent relevant business facts. If there’s a messaging system in place domain events can be forwarded to the data platform by attaching an additional message consumer. Data can be collected, processed, and forwarded to the data platform in real time. With this streaming ingestion, data is sent in small batches when they arrive, so they are immediately available for analytics. As domain events are already well defined, there is little to do in terms of cleaning and preprocessing, except deduplication and anonymization of PII data. Sometimes, it is also advisable to define and ingest internal analytical events that contain information that is relevant only for analytical use cases so that domain events don’t have to be modified.
Examples for streaming ingestion: Kafka Connect, Kafka Streams, AWS Lambda

Many business objects are persisted as entities and aggregates in SQL or NoSQL databases. Their state changes over time, and the latest state is persisted in the database only. Strong candidates for entities with state are articles, prices, customer data, or shipment status. For analytical use cases, it is often required to have both the latest state and the history of states over time. There are several approaches to ingest entities. One way is to generate and publish an onCreate/onUpdate/onDelete event with the current state every time an entity is changed, e.g. by adding an aspect  or EntityListeners . Then streaming ingestion can be used to ingest the data as described above. When it is not feasible to change the operational software, change data capture (CDC) may be used to listen to database changes directly and stream them into the data platform.
Examples for CDC streaming: Debezium 

Lastly, traditional scheduled ELT or ETL jobs that export data to file and load them into the platform can be set up, with the downside of not having real-time data, not having all stage changes between exports, and some work to consolidate exported data again. However, they are a viable option for legacy systems, such as mainframes.

Clean Data

Clean data is the foundation for effective data analytics. With data mesh, domain teams are responsible for performing data cleaning. They know their domain and can identify why and how their domain data needs to be processed.

Data that is ingested into the data platform is usually imported in its original raw and unstructured format. When using a columnar database, this might be a row per event that contains a CLOB  field for the event payload, which may be in JSON format. Now it can be preprocessed to get data clean:

Structuring: Transform unstructured and semi-structured data to the analytical data model, e.g., by extracting JSON fields into columns.
Mitigation of structural changes: When data structures have changed, mitigate them, e.g., by filling null values with sensible defaults.
Deduplication: As most analytical storage systems are append-only, entities and events cannot be updated. Remove all duplicate entries.
Completeness: Ensure that data contain agreed periods, even when there were technical issues during ingestion.
Fix outliers: Invalid data that may occur through bugs get identified and corrected.

From an implementation perspective, these preprocessing steps can be implemented as simple SQL views that project the raw data. The queries may be organized through common table expressions  (CTEs) and may be enhanced with user-defined functions  (UDFs), e.g., for JSON processing. As an alternative, the cleaning steps can be implemented as lambda functions that operate on topics. More complex pipelines can be built with frameworks like dbt  or Apache Beam  that offer an advanced programming model, but also require more skills to master.

	-- Step 1: Deduplicate
	WITH inventory_deduplicated AS (
	    SELECT *
	    EXCEPT (row_number)
	    FROM (
	       SELECT *,
	              ROW_NUMBER() OVER (PARTITION BY id ORDER BY time DESC) row_number
	       FROM `datameshexample-fulfillment.raw.inventory`)
	    WHERE row_number = 1
	),
	-- Step 2: Parse JSON to columns
	inventory_parsed AS (
	    SELECT
	        json_value(data, "$.sku")                           AS sku,
	        json_value(data, "$.location")                      AS location,
	        CAST(json_value(data, "$.available") AS int64)      AS available,
	        CAST(json_value(data, "$.updated_at") AS timestamp) AS updated_at,
	    FROM inventory_deduplicated
	)
	-- Step 3: Actual Query
	SELECT sku, location, available, updated_at
	FROM inventory_parsed
	ORDER BY sku, location, updated_at
view raw
entities__inventory_history.sql hosted with ❤ by GitHub
Analytics

To gain insights, domain teams query, process, and aggregate their analytical data together with relevant data products from other domains.

SQL is the foundation for most analytical queries. It provides powerful functions to connect and investigate data. The data platform should perform join operations efficiently, even for large data sets. Aggregations are used to group data and window functions help to perform a calculation across multiple rows. Notebooks help to build and document exploratory findings.
Examples: Jupyter Notebooks, Presto

Humans understand data, trends, and anomalies much easier when they perceive them visually. There are a number of great data visualization tools that build beautiful charts, key performance indicator overviews, dashboards and reports. They provide an easy-to-use UI to drill down, filter, and aggregate data.
Examples: Looker, Tableau, Metabase, Redash

For more advanced insights, data science and machine learning methods can be applied. These enable correlation analyses, prediction models, and other advanced use cases. Special methodological, statistical, and technological skills are required.
Examples: scikit-learn, PyTorch, TensorFlow

Data Platform

The self-serve data platform may vary for each organization. Data mesh is a new field and vendors are starting to add data mesh capabilities to their existing offerings.

Looking from the desired capabilities, you can distinguish between analytical capabilities and data product capabilities: Analytical capabilities enable the domain team to build an analytical data model and perform analytics for data-driven decisions. The data platform needs functions to ingest, store, query, and visualize data as a self-service. Typical data warehouse and data lake solutions, whether on-premise or a cloud provider, already exist. The major difference is that each domain team gets its own isolated area.

A more advanced data platform for data mesh also provides additional domain-agnostic data product capabilities for creating, monitoring, discovering, and accessing data products. The self-serve data platform should support the domain teams so that they can quickly build a data product as well as run it in production in their isolated area. The platform should support the domain team in publishing their data products so that other teams can discover them. The discovery requires a central entry point for all the decentralized data products. A data catalog can be implemented in different ways: as a wiki, git repository, or there are even already vendor solutions for a cloud-based data catalog such as Select Star, Google Data Catalog, or AWS Glue Data Catalog. The actual usage of data products, however, requires a domain team to access, integrate, and query other domains' data products. The platform should support, monitor, and document the cross-domain access and usage of data products.

An even more advanced data platform supports policy automation. This means that, instead of forcing the domain team to manually ensure that the global policies are not violated, the policies are automatically enforced through the platform. For example, that all data products have the same metadata structure in the data catalog, or that the PII data are automatically removed during data ingestion.

Efficiently combining data products from multiple domains, i.e., having large cross-domain join operations within a few seconds, ensures developer acceptance and happiness. That's why the query engine has a large influence on the architecture of the data platform. A shared platform with a single query language and support for separated areas is a good way to start as everything is highly integrated. This could be Google BigQuery with tables in multiple projects that are discoverable through Google Data Catalog. In a more decentralized and distributed data mesh, a distributed query engine such as Presto can still perform cross-domain joins without importing data, but they come with their own limitations, e.g., limited pushdowns require that all underlying column data need to be transferred.

Enabling Team

The enabling team spreads the idea of data mesh within the organization. In the beginning of data mesh adoption, a lot of explanatory efforts will be required and the enabling team can act as data mesh advocates. They help domain teams on their journey to become a full member of the data mesh. The enabling team consists of specialists with extensive knowledge on data analytics, data engineering, and the self-serve data platform.

A member of the enabling team temporarily joins a domain team for a limited time span like a month as an internal consultant to understand the team’s needs, establish a learning environment, upskill the team members in data analytics, and guide them on how to use the self-serve data platform. They don’t create data products by themselves.

In between their consulting engagements, they share learning materials such as walking skeletons, examples, best practices, tutorials, or even podcasts.

Mesh

The mesh emerges when teams use other domain's data products. Using data from upstream domains simplifies data references and lookups (such as getting an article's price), while data from downstream domains enables analyzing effects, e.g. for A/B tests (such as changes in the conversion rate). Data from multiple other domains can be aggregated to build comprehensive reports and new data products.

Let's look at a simplified e-commerce example:

Domains can be classified by data characteristics and data product usage. We adopt Zhamak Dehghani’s classification:

Source-aligned

In this example, an online shop is subdivided into domains along the customer journey, from product search over checkout to payment. In a data mesh, these domains publish their data as data products, so others can access them. The engineers do analytics on their own data to improve their operational systems and validate the business value of new features. They use domain neighbor’s data to simplify their queries and get insights on effects in downstream domains. These domain data can be referred to as source-aligned, as most of their published data products correspond closely to the domain events and entities generated in their operational systems.

Aggregate

For complicated subsystems , it can be efficient that a team focuses solely on delivering a data product that is aggregated of various data products from other domains. A typical example is a 360° customer view that includes relevant data from multiple domains, such as account data, orders, shipments, invoices, returns, account balance, and internal ratings. With respect to different bounded contexts, a comprehensive 360° customer view is hard to build, but it might be useful for many other domains. Another example for a complicated subsystem is building sophisticated ML models that require enhanced data science skills. It may be sensible that a data scientists team develops and trains a recommendation model by using data from checkout and the 360° customer view, and another team uses this model and focuses to present the calculated recommendations in the online shop or in promotional emails.

Consumer-aligned

In a company, there are also business departments that need data from the whole value stream to make sensible decisions, with people working in these departments are business experts but not engineers or technology-savvy. Management and controlling requires detailed reports and KPIs from all domains to identify strengths and deviations. Marketing does funnel and web analysis over all steps in the customer journey in their own optimized tools, such as Google Analytics or Adobe Analytics. In these domains, the data model is optimized for a specific department's needs and can therefore be described as consumer-aligned. Consumer-aligned reports were often one of the main tasks of central data teams. With data mesh, (new) consumer-aligned domain teams focus on fulfilling data needs of one specific business domain, allowing them to gain deep domain knowledge and constantly develop better analytical results. Business and IT grow closer together, either by building integrated domain teams or by having engineering teams that provide domain data as a service for the business, e.g., to support C-level or controlling. Their data are typically used for their analytics and reports, but does not need to be published and managed as data products for other domains.

Tech Stacks
Data mesh is primarily an organizational approach, and that's why you can't buy a data mesh from a vendor. Technology, however, is important still as it acts as an enabler for data mesh, and only useful and easy to use solutions will lead to domain teams' acceptance. The available offerings of cloud providers already provide a sufficient set of good self-serve data services to let you form a data platform for your data mesh. We want to show which services can be used to get started.

There are a lot of different ways to implement a data mesh architecture. Here is a selection of typical tech stacks that we saw:

Google Cloud BigQuery
AWS S3 and Athena
Azure Synapse Analytics
dbt and Snowflake
Databricks (How To Build a Data Product with Databricks)
MinIO and Trino
SAP
Kafka and RisingWave
Starburst Enterprise (TBD)

If you want to share your tech stack here, feel free to reach out to us.

Domain Team’s Journey

Just as the data team has a journey to go on, each of your domain teams has to go on a journey to become a contributing part of your data mesh as well. Each team can start their journey whenever they are ready and at their own pace. The benefits arise already along the journey. Teams will quickly gain from first data-driven decisions, starting an avalanche to use more and better data for even deeper insights. The data mesh evolves with each team that shares their data as products, enabling data-driven innovation.

To make this journey successful, the team needs three things: a clear data mesh vision from top management to get everybody moving in the same direction, a supportive environment including an easy-to-use self-serve data platform to get the engineering team on a learning path toward data analytics, and a high trust environment to walk the journey in their own way and pace.

So let’s start your journey!

Level 0 No Data Analytics

Your team is responsible for a domain and builds and operates self-contained systems  including the necessary infrastructure. It was quite an effort to build these systems, and you were highly focused on delivery excellence. These operational systems now generate domain data.

Data analytics was just not relevant.

Level 1 Operational Database Queries 

Being in production, you probably have to investigate an incident and need to analyze how many customers are affected. Also, some stakeholders might have questions regarding your data, such as "Which in-stock articles haven’t been sold in the last six months?" or "What were the shipping times during the last Black Week?" To answer all these questions, you send analytical queries to your operational database. Over time, you also do some first explorative analytics to get a deeper understanding of your system’s behavior.

This increases load on your production database, and you might be tempted to change the production database to better support your analytical queries, like creating additional indices. You might offload the additional load to read replicas. But analytical queries are still slow and cumbersome to write.

Level 2 Analyze Own Data

With the pains of slow and hard-to-write analytical queries in the back of your mind, you try out the self-serve data platform that’s being promoted by the data platform team. For example, you now have access to Google BigQuery. On this platform, your team starts to build an analytical data model by ingesting messages from Kafka. This is your first data product. The data platform allows you to analyze data covering your own systems with maintainable and fast queries, while keeping the schemas of your operational databases untouched. You learn how to structure, preprocess, clean, analyze, and visualize analytical data—that’s a lot to learn even though most is SQL, which you are already familiar with.

As questions regarding your own data can now be answered quickly, you and your product owner now enter the cycle of making data-driven decisions: define hypotheses and verify with data.

Level 3 Analyze Cross-domain Data 

Analyzing your own domain data is a great start, but combining it with data from other domains is where the magic begins. It allows you to get a comprehensive view despite the decentralization of data. Examples are A/B tests of the effect of a UI change to the conversion rate or building up machine learning models for fraud detection that include previous purchasing history and current click stream behavior. This requires that other teams share their data products in a way that your team can discover, access, and use it. This is when the mesh begins to form itself.

When a team becomes a consuming member of the data mesh, it starts to gain interest in the interoperability and governance of the data mesh. Ideally, the team will send a representative to the data mesh governance group.

In case you are the first team, you may have to skip this step for now and move on to level 4 and be the first to provide data for others.

Level 4 Publish Data Contracts 

Based on other teams' needs, you share your data with others by publishing data contracts. For example, you provide the confirmed, rejected, and aborted orders so others can correlate their events to the conversion rate. Instead of just being a consumer of data products, you become a publisher of data products. You generate value for other teams. But at the same time, it increases your responsibility and operational duties in the long term.

Published data products must comply with the global policies defined by the federated governance group. You have to know and understand the current global policies. Now, at the latest, you need to participate in and contribute to the federated governance group.

Data Team’s Journey

Data mesh is primarily an organizational construct and fits right into the principles of team topologies . It shifts the responsibilities for data toward domain teams which are supported by a data platform team and a data enabling team. Representatives of all teams come together in a federated governance group to define the common standards.

Today, in many organizations a central data team is responsible for a wide range of analytical tasks, from data engineering and managing data infrastructure to creating C-level reports. Such a central data team suffers from cognitive overload, including domain, technical, and methodical knowledge. data mesh mitigates this.

Data mesh offers new perspectives for members of the central data team as their analytical and data engineering skills remain highly necessary. For example, they are a perfect fit to establish the data platform for people that prefer to work on the infrastructure. Some of them can form a data enabling team to act as internal consultants, helping domain teams on their journey. Regardless of their new roles, many of them will meet again in the data mesh federated governance group to shape the future of the data mesh.

The real mind shift, however, happens when founding new data-centric domains as shown in the figure above. Let’s look at typical management reports that large central data teams usually produce based on monolithic data warehouses or data lakes. With data mesh, the data engineers who created those management reports build a new domain team together with a dedicated product owner. As engineers of the new domain team, they now can focus on their new domain and their consumers. This allows them to gain deep domain knowledge over time, resulting in better reports and continuous optimizations. In addition, they switch from using that monolith data warehouse to data products from other domains. This switch is a gradual process driven by the demand for data products, accelerating the forming of a data mesh. The product owner negotiates with other domain teams about the required data products and makes sure that the reports and other products the new domain team will build in the future fulfill the needs of the business.

As existing domain teams on their journey do more and more data analytics, another perspective for members of the central data team is to join one of those teams. With their existing knowledge, they can accelerate the domain teams’ journeys toward a data mesh by spreading and teaching their knowledge and skills to the others in the team. It is important that they become full members of the team and not founding a data sub-team within the domain team. In addition to their knowledge and skills, the data engineers may also bring responsibilities and artifacts from the central data team to their domain teams. For example, customer profiling, which was previously done by the central data team, will move into the responsibility of the recommendation domain team.

The data scientists, typically, are centrally organized as well. That’s why their future, organizational-wise, is quite similar to that of the central data team. The data products in the data mesh they focus on are machine learning features and models. When joining an existing domain team, such a machine learning model might be fully integrated in a microservice. So, data mesh enables such machine-learning-based services because the required MLOps  capabilities can be easily built on top of the data mesh.

FAQ
So, what's really behind the hype?

Data mesh is primarily an organizational change. The responsibilities of data are shifted closer to the business value stream. This enables faster data-driven decisions and reduces barriers for data-centric innovations.

Who has actually implemented a data mesh?

There is a comprehensive collection of user journey stories from the Data Mesh Learning community that covers data mesh examples from many different industries.

Is Data Mesh for my company?

It depends, of course. There are a few prerequisites that should be in place: You should have modularized your software system following domain-driven design principles or something similar. You should have a good number (5+) of independent domain teams that have their systems already running in production. And finally, you should trust your teams to make data-driven decisions on their own.

How to get started?

Start small and agree on the big picture. Find two domain teams (that are around level 2) that have a high value use case where one team needs data from the other team. Let one team build a data product (level 4) and another team use that data product (level 3). You don’t need a sophisticated data platform yet. You can start sharing the files via AWS S3, a Git repository, or use a cloud-based database, such as Google BigQuery.

When should I avoid a Data Mesh?

There are some indicators when a data mesh approach might not be suitable for you, including:

You are too small and don’t have multiple independent engineering teams.
You have low-latency data requirements. Data Mesh is a network of data. If you need to optimize for low-latency, invest in a more integrated data platform.
You are happy with your monolithic highly integrated system (such as SAP). It might be more efficient to use their analytical platform.
What is Data Mesh not?
Data Mesh is not a Silver Bullet.
Data Mesh is not a religion.
Data Mesh is not plug-and-play.
Data Mesh is not a product you can just buy.
Data Mesh is not a data-only platform.
Data Mesh cannot be implemented by the data team alone.
Data Mesh is not a concept for operational data.
Data Mesh is not data virtualization.
Data Mesh is not the successor to Data Warehouse or Data Lake.
Data Mesh cannot be rapidly implemented as Big Bang.
Data mesh is not a service mesh for data.
And data mesh has absolutely nothing to do with blockchain.
Is the Data Mesh a generic solution to a distributed data architecture?

No.

By definition, data mesh does not include data products used for serving real-time needs. Data mesh focuses on analytical use cases.

Data Product: What data to include in a data product? Should a data product include other domain's data?

Data that is created and owned by a domain are prime candidates, and the domain team should be encouraged to publish them in an appropriate, cleaned and managed form.

For source-aligned domains, we mostly would argue to include reference IDs. It is OK to include other domain's data, if the data was transformed, is the basic for business decisions or the exact state of the data at a processing time was relevant. In fact, these are cases, when the processing domain takes ownership for these data based on business cases.

Aggregate domains and consumer-aligned domains can include all foreign data that are relevant for their consumers' use cases.

What's the difference between data mesh and data fabric?

At first, data fabric  looks similar to data mesh because it offers a similar self-serve data platform. Looking deeper, it turns out that data fabric is a central and domain-agnostic approach, which is in strong contrast to the domain-centric and decentralized approach of data mesh. More in this comparison article .

What might a journey be for teams who operate commercial off-the-shelf (COTS) systems?

Many COTS systems (such as Salesforce, SAP, Shopify, Odoo) provide domain optimized analytical capabilities. So the journey for domain teams starts directly from level 2.

The challenge is to integrate data products from other domains (level 3, which may be skipped if not needed) and to publish data products for other domains (level 4). The system’s data need to be exported to the data platform and managed as data products, conforming the global policies. As data models evolve with system updates, an anti-corruption layer is a must, e.g., as a cleaning step.

How might externally acquired datasets be part of a data mesh?

Typical examples: Price-Databases or Medical Studies. A team needs to own this dataset and bring it into the datamesh. If this is not a very technical team, the data-platform should offer an easy self-service to upload files and provide Meta-Data. An Excel API or Google Sheets might also be an option here.

How did you draw the diagrams?

We got this question quite a lot, so we are happy to share our tooling:
We use diagrams.net  with "Sketch" style and Streamline Icons . We automate the conversion to PNG, SVG and WebP with a little script.

What are your questions?

If you have any more questions, we encourage you to discuss with us on GitHub or reach out to us directly. But be warned: Your question might end up in the FAQ. :-)

Learn more

Data Mesh Learning Community

by Scott Hirleman

Scott started the Data Mesh Learning community website, a Slack channel, and issues a newsletter every two weeks. He collects articles and experience reports about data mesh, and even has a podcast on the topic. It is worth subscribing to stay informed.

How to Move Beyond a Monolithic Data Lake to a Distributed Data Mesh

by Zhamak Dehghani

In this article, Zhamak Dehghani introduced the idea and core principles of data mesh. It is a must-read for everyone in this field. In her follow-up article, Zhamak gets into more detail.

Data Mesh

by Zhamak Dehghani

Zhamak's book about data mesh. The book not only discusses the principles of data mesh, but also presents an execution strategy.

Data Mesh

by Zhamak Dehghani

Zhamak's book about data mesh, but in German and in color. Translated by Jochen and Simon, who are co-authors of this website.

Data Mesh in Action

by Jacek Majchrzak, Sven Balnojan, and Marian Siwiak

Data Mesh in Action is a great hands-on book. We really liked the MVP hat shows how to start implementing a data mesh.

Data Mesh: Decentralized Data Analytics for Software Engineers

by Jochen Christ

Intro article to data mesh with examples in the Google Cloud.
Authors

Jochen

@jochen_christ

Jochen Christ works as tech lead at INNOQ and is a specialist for self-contained systems and data mesh. Jochen is maintainer of HTTP Feeds, Which JDK, and co-author of Remote Mob Programming.

Larysa

@visenger

Dr. Larysa Visengeriyeva received her doctorate in Augmented Data Quality Management at the TU Berlin. At INNOQ she is working on the operationalization of Machine Learning (MLOps).

Simon

@simonharrer

Dr. Simon Harrer is a curious person working at INNOQ who likes to share his knowledge. He's a serial co-author of Java by Comparison, Remote Mob Programming, GitOps, and, most recently, Data Mesh.
Contributors

A ton of people helped us curate our content through their great feedback. Special thanks to: Anja Kammer, Benedikt Stemmhildt, Benjamin Wolf, Eberhard Wolff, Gernot Starke, Jan Bode, Jan Schwake, Julian Schikarski, Jörg Müller, Markus Harrer, Matthias Geiger, Philipp Beyerlein, Rainer Jaspert, Stefan Tilkov, Tammo van Lessen, and Theo Pack.

And if you have feedback for us as well, feel free to discuss with us on GitHub or reach out to us directly!

Workshop  Training  Legal Notice  Privacy
