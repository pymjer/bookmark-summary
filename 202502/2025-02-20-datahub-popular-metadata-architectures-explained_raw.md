Title: DataHub: Popular metadata architectures explained

URL Source: https://www.linkedin.com/blog/engineering/data-management/datahub-popular-metadata-architectures-explained

Markdown Content:
When I started my journey at LinkedIn ten years ago, the company was just beginning to experience extreme growth in the volume, variety, and velocity of our data. Over the next few years, my colleagues and I in LinkedIn’s data infrastructure team built out foundational technology like [Espresso](https://www.cs.cmu.edu/~pavlo/courses/fall2013/static/papers/p1135-qiao.pdf), [Databus](https://engineering.linkedin.com/data-replication/open-sourcing-databus-linkedins-low-latency-change-data-capture-system), and [Kafka](https://kafka.apache.org/), among others, to ensure that LinkedIn would survive and thrive through the next wave of growth. A few years later, I became the tech lead for what was then a pretty small “data analytics infrastructure” team that ran and supported LinkedIn’s Hadoop usage, and also maintained a hybrid data warehouse spanning Hadoop and Teradata.

One of the first things I noticed was how often people were asking around for the “right dataset” to use for their analysis. It made me realize that, while we had built highly-scalable specialized data storage, streaming capabilities, and cost-efficient batch computation capabilities, we were still wasting time in just finding the right dataset to perform analysis.

Data discovery: One problem, many solutions 
--------------------------------------------

Fast forward to today and we’re living in the golden age of data. When a data scientist joins a data-driven company, they expect to find a data discovery tool (i.e., data catalog) that they can use to figure out which datasets exist at the company, and how they can use these datasets to test new hypotheses and generate new insights. Most data scientists don’t really care about how this tool actually works under the hood, as long as it enables them to be productive.

In fact, there are numerous data discovery solutions available: a combination of proprietary software available for purchase, open source software contributed by a particular company, and software built in-house. In the past few years, [LinkedIn](https://engineering.linkedin.com/blog/2016/03/open-sourcing-wherehows--a-data-discovery-and-lineage-portal), [Airbnb](https://medium.com/airbnb-engineering/democratizing-data-at-airbnb-852d76c51770), [Lyft](https://eng.lyft.com/amundsen-lyfts-data-discovery-metadata-engine-62d27254fbb9), [Spotify](https://engineering.atspotify.com/2020/02/27/how-we-improved-data-discovery-for-data-scientists-at-spotify/), [Shopify](https://shopify.engineering/solving-data-discovery-challenges-shopify), [Uber](https://eng.uber.com/metadata-insights-databook/), and [Facebook](https://engineering.fb.com/2020/10/09/data-infrastructure/nemo/) have all shared details of their own data discovery solutions. This begs the question: how are each of these platforms different, and which option is best for companies thinking of adopting one of these tools?

The architecture of your data catalog will influence how much value your organization can truly extract from your data. Additionally, catalogs are sticky, taking a long time to integrate and implement at a company. As a result, it’s important to choose your data discovery solution carefully.

In this post, I will describe three generations of architectures that the industry has produced so far for data discovery tools, as well as explain where along this spectrum many of the most well-known options fall. This progression between generations is also mirrored by the evolution of the architecture of DataHub at LinkedIn, as we’ve driven the latest best practices (first open sourced and shared with the world as [WhereHows](https://engineering.linkedin.com/blog/2016/03/open-sourcing-wherehows--a-data-discovery-and-lineage-portal) in 2016, and then rewritten completely and re-shared with the open source community in 2019 as [DataHub)](https://github.com/linkedin/datahub).

Hopefully, this post will help you make the best decision possible as you choose your own data discovery solution.

What is a data catalog?
-----------------------

Before we dive into the different architectures, let’s get our definitions in order. One of the simplest definitions for a data catalog I’ve found is from the [Oracle website](https://www.oracle.com/big-data/what-is-a-data-catalog/#:~:text=Simply%20put%2C%20a%20data%20catalog,support%20data%20discovery%20and%20governance): “Simply put, a data catalog is an organized inventory of data assets in the organization. It uses metadata to help organizations manage their data. It also helps data professionals collect, organize, access, and enrich metadata to support data discovery and governance.”

Thirty years ago, a data asset was likely a table in an Oracle database. In a modern enterprise, though, we have a dazzling array of different kinds of assets that comprise the landscape: tables in relational databases or in NoSQL stores, streams in your favorite stream store, features in your AI system, metrics in your metrics platform, dashboards in your favorite visualization tool, etc. The modern data catalog is expected to contain an inventory of all these kinds of data assets and enable data workers to be more productive at getting things done with those assets.

Why do you need a catalog?
--------------------------

Before you decide to buy or adopt a specific data catalog solution or build your own, you should first ask what things you want to enable for your enterprise with a data catalog. A related and important question concerns what kinds of metadata you want to store in your data catalog, because that directly influences the kinds of use cases you can enable.

Here are a few common use cases and a sampling of the kinds of metadata they need:

*   **Search and Discovery:** Data schemas, fields, tags, usage information
*   **Access Control:** Access control groups, users, policies
*   **Data Lineage:** Pipeline executions, queries, API logs, API schemas
*   **Compliance:** Taxonomy of data privacy/compliance annotation types
*   **Data Management:** Data source configuration, ingestion configuration, retention configuration, data purge policies (e.g., for GDPR “Right To Be Forgotten”), data export policies (e.g., for GDPR “Right To Access”)
*   **AI Explainability, Reproducibility:** Feature definition, model definition, training run executions, problem statement
*   **Data Ops:** Pipeline executions, data partitions processed, data statistics
*   **Data Quality:** Data quality rule definitions, rule execution results, data statistics

One interesting observation is that each individual use case often brings in its own special metadata needs, and yet also requires connectivity to existing metadata brought in by other use cases. We’ll refer back to this insight as we dive into the different architectures of these data catalogs and their implications for your success.

First-generation architecture: Monolith everything
--------------------------------------------------

The figure below describes the first generation of metadata architectures. It is typically a classic monolith frontend (maybe a Flask app) with connectivity to a primary store for lookups (typically MySQL/Postgres), a search index for serving search queries (typically Elasticsearch), and, for generation 1.5 of this architecture, maybe a graph index for handling graph queries for lineage (typically Neo4j) once you hit the limits of relational databases for “recursive queries.”

_First-generation architecture: Pull-based ETL_

Metadata is typically ingested using a crawling approach by connecting to sources of metadata like your database catalog, the Hive catalog, the Kafka schema registry, or your workflow orchestrator’s log files, and then writing this metadata into the primary store, with the portions that need indexing added into the search index and the graph index. This crawling is typically a single process (non-parallel), running once a day or so. During this crawling and ingestion, there is often some transformation of the raw metadata into the app’s metadata model, because the data is rarely in the exact form that the catalog wants it. Typically, this transformation is embedded into the ingestion job directly.

Slightly more advanced versions of this architecture will also allow a batch job (e.g., a Spark job) to process metadata at scale, compute relationships, recommendations, etc., and then load this metadata into the store and the indexes.

It typically takes a couple of engineers two weeks or so to stand up the first prototype of this basic backend architecture and load data into it. Separately, it can take a few weeks to stand up a simple frontend that can surface this metadata and support simple search.

**The benefits  
**Here are the good things about this architecture.

*   **Few moving parts:** With just a lookup store, a search index, and a few crawlers, you can quickly aggregate metadata and build a useful application that makes data workers productive. There isn’t a lot of infrastructure that you have to stand up and operate to prove this out.
*   **One team can do a lot:** This architecture is geared towards a single team that has the ability to access metadata sources and can build an application to serve it. 

**The downsides  
**However, there are some things that this architecture really struggles with. I’ll just highlight the top two.

*   **Push versus pull:** While it is easy to get started with crawling data sources as a way to collect metadata and aggregate it in a single place, very soon these ingestion pipelines start showing signs of fragility. The crawler runs in a different environment than the data source and its configuration needs to be managed separately by the central team. So, one set of problems in these pipelines is operational hurdles like network connectivity (firewall rules) or credential sharing (passwords can change without notifying the central team).  
    Another set of problems is more subtle but also operational in nature. Crawling-based ingestion typically leads to workloads that are batchy (how often are you calling the source?) and non-incremental (how many records should we retrieve in a single pull?). This makes your data source’s operations team extremely unhappy, because no one likes to be woken up in the middle of the night with a melting database or a non-responsive [HDFS](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html) Namenode and find that it is croaking because the metadata crawler has tipped it over the edge. The first casualty of such operational problems is usually the metadata crawler pipeline, whether or not it was actually responsible! Your metadata ingestion pipeline will be paused, and while the operations team works on nursing your system back to health, they will often ask for the metadata crawler to remain paused for an extended time, even if the system is back. Meanwhile, your metadata gets “staler and staler,” leading to diminished trust in your catalog. This brings us to the second problem. 
*   **Metadata freshness:** Closely related to this decision to push versus pull is the issue of data (or in this case, metadata) freshness. In the beginning of your metadata journey, it might seem completely okay to crawl your Hive metastore (or S3 buckets) once per night and populate the catalog. After all, you’re just trying to make data scientists more productive than they were before. However, once you start getting into the data creation workflow (e.g., once you have created some data, you can come here and provide data classification tags) or providing operational metadata (e.g., the data quality manifest for your latest partition), then the freshness of the metadata starts mattering a lot more. If you just have a crawl-based metadata catalog, you’re mostly out of luck at that point.   

**What does this mean for me?  
**As a reader, you might be thinking, “So, what are the first-generation metadata systems out there?” [Amundsen](https://www.amundsen.io/) employs this architecture, as did the original version of WhereHows that we open sourced in 2016. Among in-house systems, Spotify’s [Lexikon](https://engineering.atspotify.com/2020/02/27/how-we-improved-data-discovery-for-data-scientists-at-spotify/), Shopify’s [Artifact](https://shopify.engineering/solving-data-discovery-challenges-shopify), and Airbnb’s [Dataportal](https://medium.com/airbnb-engineering/democratizing-data-at-airbnb-852d76c51770) also follow the same architecture.

These systems play an important role in making humans more productive with data, but can struggle underneath to keep a high-fidelity data inventory and to enable programmatic use cases of metadata.

Second-generation architecture: 3-tier app with a service API
-------------------------------------------------------------

The figure below describes what I would classify as a second-generation metadata architecture. The monolith application has been split into a service that sits in front of the metadata storage database. The service offers an API that allows metadata to be written into the system using push mechanisms, and programs that need to read metadata programmatically can read the metadata using this API. However, all the metadata accessible through this API is still stored in a single metadata store, which could be a single relational database or a scaled out key-value store.

_Second-generation architecture: Service with Push API_

**The benefits  
**Let’s talk about the good things that happen with this evolution.

*   **Better contracts lead to better outcomes:** Providing a push-based schema-ed interface immediately creates good contracts between producers of metadata and the “central metadata team.” You still have to convince the producing team to emit metadata and take the dependency, but it is so much easier to do that with an agreed-upon schema.
*   **Programmatic use cases enabled:** With a service API, the central team can finally enable programmatic use cases for metadata. For example, if your data portal application allows for tagging your datasets and fields with tags that specify the semantic-type of the field (e.g., email\_address, customer\_identifier) and stores that information in your metadata system, your data management infrastructure can start using this metadata to automatically delete data assets for customer ids that have requested the right to be forgotten, or to automatically create pseudonymised versions of these datasets for data scientists. In fact, at LinkedIn, we use [Apache Gobblin](https://gobblin.apache.org/) to do exactly that, driven by metadata from DataHub.

**The downsides  
**However, there are still problems that this architecture has that are worth highlighting.

*   **There is no change log:** The second-generation architecture offers a micro-service based API for reading and writing metadata, but there is no built-in support for streaming in changes to metadata from external systems or for subscribing to the stream of metadata changes happening to the data catalog.  
    You may be familiar with this [popular blog post](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying) on why logs should be at the heart of your data ecosystem. Turns out, the same is actually true about metadata as well. A modern data catalog  should enable real-time subscription to changes in the metadata as a first-class offering.  
    Without a metadata log, it is much harder to bootstrap (re-create) or fix your search and graph index reliably when something goes wrong. Without a real-time metadata change log, it is also impossible to build efficient reactive systems, like data triggers or access control abuse detection systems, on top of the central metadata platform. To build something like this, you would be forced to access the metadata using either the key-value API through excessive polling or full scans. Alternately, you would need to wait for a nightly ETL of the metadata database to be able to process the snapshot eventually. We’ve lived through that painful journey in data, and we would really like to skip it for metadata! Yet modern metadata systems often forget to design for this important capability.
*   **The problem with centralized teams:** The other big problem with this architecture is that it continues to depend on a centralized team for too many things: owning the metadata model, running the central metadata service and data stores and indexes, and supporting all the downstream consumers and the different ways in which they want to access the metadata. This severely limits the central system’s ability to power the diversity of use cases (productivity, governance, AI explainability, and so on) that exist in the company. At LinkedIn, for example, when we were still in the second generation of our metadata architecture, we had our data quality team build a separate UI and metadata store to store rules and display data quality results on datasets.  
    The operational impact of a service-based integration also results in tight coupling of the availability of the producer and the central service, which makes adopters nervous about adding one more source of downtime to their stack. 

> _“The central metadata team runs into the same issues that a central data warehouse team runs into.”_ 

Data engineering itself is evolving into a different model—decentralization is becoming the norm. Therefore, the central metadata team should not make the same mistake of trying to succeed at keeping pace with the fast evolving complexity of the metadata ecosystem.

**What does this mean for me?  
**Among the commercial metadata systems, [Collibra](https://www.collibra.com/) and [Alation](https://www.alation.com/) appear to have second-generation architectures. Among the open source metadata systems, [Marquez](https://github.com/MarquezProject) has a second-generation metadata architecture.

My experience is that second-generation metadata systems often can become reliable search and discovery portals for data assets at a company, so they do fill the productivity needs for data workers. They can also start to offer service-based integration into programmatic workflows such as access-control provisioning. We actually went through exactly this journey when we evolved WhereHows from Gen 1 to Gen 2 by adding a push-based architecture and a purpose-built service for storing and retrieving this metadata.

However, the centralization bottleneck can often result in new, separate catalog systems being built or adopted for different use cases, which dilutes the power of a single, consistent metadata graph. Companies that have built or adopted a search and discovery portal for their data scientists sometimes also end up installing a different data governance product with its own metadata backend for their business department. In order to keep dataset definitions and glossaries in sync, these companies have to build and monitor new data pipelines to reliably copy metadata, which are represented using different metadata models from one catalog to another. The problem isn’t limited to large companies, but can affect any organization that has reached a certain level of data-literacy and has enabled diverse use cases for metadata.

Third-generation architecture: Event-sourced metadata
-----------------------------------------------------

The key insight leading to the third generation of metadata architectures is that a “central service”-based solution for metadata struggles to keep pace with the demands that the enterprise is placing on the use cases for metadata. To remedy this problem, there are two needs that must be met. The first is that the metadata itself needs to be free-flowing, event-based, and subscribable in real-time.The second is that the metadata model must support constant evolution as new extensions and additions crop up—without being blocked by a central team. This will allow metadata to be always consumable and enrichable, at scale, by multiple types of consumers.

**Step 1: Log-oriented metadata architecture  
**The metadata provider can push to a stream-based API or perform [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) operations against the catalog’s service API, depending on their preference. The resulting mutations to the metadata will, in turn, generate the metadata changelog. This metadata log can be automatically and deterministically materialized into the appropriate stores and indexes (e.g., search index, graph index, data lake, olap store) for all the query patterns needed. This results in an unbundled metadata database architecture that is ready for the modern enterprise, as shown in the figure below. Now that the log is the center of your metadata universe, in the event of any inconsistency, you can bootstrap your graph index or your search index at will, and repair errors deterministically.

_Third-generation architecture: Unbundled metadata database_

**Step 2: Domain-oriented decoupled metadata models  
**In addition to a stream-first architecture, the third-generation catalog enables extensible, strongly-typed metadata models and relationships to be defined collaboratively by the enterprise. Strong typing is important, because without that, we get the least common denominator of generic property-bags being stored in the metadata store. This makes it impossible for programmatic consumers of metadata to process metadata with any guarantee of backwards compatibility.

In the metadata model graph below, we use DataHub’s terminology of Entity Types, Aspects, and Relationships to describe a graph with three kinds of entities: Datasets, Users, and Groups. Different aspects such as Ownership, Profile, etc. can be attached to these entities by different teams, which results in relationships being created between these entity types. Note that there can be various ways to describe these graph models, from [RDF-based](https://en.wikipedia.org/wiki/Resource_Description_Framework) models to full-blown [ER models](https://en.wikipedia.org/wiki/Entity%E2%80%93relationship_model) to [custom hybrid approaches](https://github.com/linkedin/datahub/blob/master/docs/how/metadata-modelling.md) like DataHub uses.

_An example metadata model graph: Types, aspects, relationships_

This sort of modeling gives teams the ability to evolve the global metadata model by adding domain-specific extensions, without getting bottlenecked by the central team. For example, the compliance team might check-in the Ownership aspect, while the core metadata team might check-in the Schema aspect for a Dataset entity. Meanwhile, the data ingestion team might design and check-in the ReplicationConfig aspect for a Dataset entity. All these additions to the model can happen independently, with minimal friction points. Of course, the core Entity Types need to be governed and agreed on before we introduce them into the graph.

**The benefits  
**With this evolution, clients can interface with the metadata database in different ways depending on their needs. They get a stream-based metadata log (for ingestion and for change consumption), low latency lookups on metadata, the ability to have full-text and ranked search on metadata attributes, and graph queries on metadata relationships, as well as full scan and analytics capabilities. Different use cases and applications with different extensions to the core metadata model can be built on top of this metadata stream without sacrificing consistency or freshness. You can also integrate this metadata with your preferred developer tools, such as git, by authoring and versioning this metadata alongside code. Refinements and enrichments of metadata can be performed by processing the metadata change log at low latency or by batch processing the compacted metadata log as a table on the data lake.

The figure below shows what a fully realized version of this architecture looks like:

_Third-generation architecture: End-to-end data flow_

Any global enterprise metadata needs, such as global lifecycle management, audits, or compliance, can be solved by building workflows that query this global metadata either in streaming form or in its batch form.

> “Good metadata architectures” are eerily similar to “good data architectures”

The typical signs of a good third-generation metadata architecture implementation are that you are always able to read and take action on the freshest metadata, in its most detailed form, without loss of consistency. When we transitioned from WhereHows (Gen 2) to DataHub (Gen 3) at LinkedIn, we found that we were able to improve the trust in our metadata tremendously, leading to the metadata system becoming the center of the enterprise. It is now well on its way to becoming the starting point for data workers as they work on new hypotheses, discover new metrics, manage the lifecycle of their existing data assets, etc.

**The downsides  
**Sophistication often goes hand in hand with complexity. A third-generation metadata system will typically have a few moving parts that will need to be set up for the entire system to be humming along well. Companies with a small number of data engineers might find themselves turned off by the amount of work it takes to get a simple use case off the ground and wonder if the initial investment in time and effort is worth the long term payoff. However, third-generation metadata systems like DataHub are starting to make big advances in usability and out-of-the-box experience for adopters to ensure that this doesn’t happen.

**What does this mean for me?  
**Out of all the systems out there that we’ve surveyed, the only ones that have a third-generation metadata architecture are [Apache Atlas](https://atlas.apache.org/), [Egeria](https://egeria.odpi.org/), [Uber Databook](https://eng.uber.com/metadata-insights-databook/), and [DataHub](https://github.com/linkedin/datahub). Among these, Apache Atlas is tightly coupled with the Hadoop ecosystem. Some companies are experimenting with [attaching Amundsen on top of Atlas](https://medium.com/wbaa/facilitating-data-discovery-with-apache-atlas-and-amundsen-631baa287c8b) to try to get the best of both worlds, but it seems like there are several challenges with this integration. For example, you must ingest your metadata and store it in Atlas’s graph and search index, bypassing Amundsen’s data ingestion, storage, and indexing modules completely. This means any new concepts you want to model need to be introduced as Atlas concepts, and then bridged with Amundsen’s UI, leading to quite a bit of complexity. Egeria supports an integration of different catalogs through a metadata event bus, but it doesn’t seem to be feature complete yet as of this writing. Uber Databook seems to be based on very similar design principles as DataHub, but is not available as open source. Of course, we are biased due to our personal experience with DataHub, but the open-sourced DataHub offers all the benefits of a third-generation metadata system with the ability to support multiple types of entities and relationships and a stream-first architecture.

At LinkedIn, DataHub’s deployment includes datasets, schemas, streams, compliance annotations, GraphQL endpoints, metrics, dashboards, features, and AI models, making it truly third-generation in terms of its proven scale and battle readiness. It routinely handles upwards of ten million entity and relationship change events in a day and, in aggregate, indexes more than five million entities and relationships while serving operational metadata queries with low millisecond-level SLAs, enabling data productivity, compliance, and governance workflows for all our employees.

Here is a simple visual representation of the metadata landscape today.

Of course, this is just a current snapshot of where different systems are today. With the growing demands for metadata in enterprises, there will likely be further consolidation in Gen 3 systems and updates among others.

Is a good architecture enough?
------------------------------

It appears that with the third-generation architecture as implemented by DataHub, we have attained a good metadata architecture that is extensible and serves our many use cases well. Are there other things left to solve in this area? The short answer is “yes.” The third-generation metadata architecture ensures that you are empowered to integrate, store, and process metadata in the most scalable and flexible way possible. But that is not enough.

> “Putting metadata to work is harder than just putting metadata together.” 

You first need to have the right metadata models defined that truly capture the concepts that are meaningful for your enterprise. Then, you need an AI-enabled pathway to transition from this complete, reliable inventory of data assets to a trusted knowledge graph of metadata. This will allow you to truly unlock productivity and governance for your enterprise. But that’s another blog post for another day!

Join the conversation
---------------------

We are collaborating with some leading thinkers and influencers to host a [virtual Metadata Summit](https://metadataday2020.splashthat.com/) on Dec. 14 that will delve into all these issues and more. Join the interactive event to learn about the diversity of projects, ideas, and use-cases around metadata and hear from leading practitioners and thought leaders on the challenges with putting metadata in production and the way forward. We’re looking forward to engaging with you!
