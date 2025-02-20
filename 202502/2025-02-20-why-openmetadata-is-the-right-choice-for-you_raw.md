Title: Why OpenMetadata is the Right Choice for you - OpenMetadata

URL Source: https://blog.open-metadata.org/why-openmetadata-is-the-right-choice-for-you-59e329163cac

Published Time: 2021-12-02T20:44:38.607Z

Markdown Content:
[![Image 1: Suresh Srinivas](https://miro.medium.com/v2/resize:fill:88:88/1*IUowKbWJqeqxHGTEbaP-fQ.jpeg)](https://suresh-srinivas.medium.com/?source=post_page---byline--59e329163cac---------------------------------------)

[![Image 2: OpenMetadata](https://miro.medium.com/v2/resize:fill:48:48/1*NTBaKucQjjMGziseWsSF5Q.png)](https://blog.open-metadata.org/?source=post_page---byline--59e329163cac---------------------------------------)

We’ve had an overwhelming response for the [OpenMetadata project](https://open-metadata.org/) since it was launched [in the latter half of 2021](https://blog.open-metadata.org/announcing-openmetadata-20399b816e60). A frequent question from users who are deciding what system they should adopt is:

“What generation of metadata systems is OpenMetadata? Are you pull-based, push-based, or hybrid? How is it different from other open-source and commercial solutions?”

[![Image 3](https://miro.medium.com/v2/resize:fit:700/1*M_tTyWDVeLK72TX0_DklIg.png)](https://cloud.getcollate.io/?pk_campaign=14_day_trial&pk_source=medium&pk_medium=large_rectangle)

These questions have their roots in the excellent [blog](https://engineering.linkedin.com/blog/2020/datahub-popular-metadata-architectures-explained) that captured the evolution of the metadata systems at LinkedIn. But what if my answer is that these categorizations are not that important. There are other important factors to consider when you start your journey to lay a strong foundation of metadata.

What architecture is right for me?
----------------------------------

The current terminology used to distinguish generations of metadata system architectures makes sense in the context of how the system evolved at LinkedIn. This evolution could look entirely different in a different company, depending on the experience of the engineering teams building these systems and the availability of time and resources.

**Architectural choices in large tech companies like Uber and LinkedIn reflect Conway’s law. System designers seek to minimize friction across organizational boundaries, leverage existing know-how and technology choices, and reuse building blocks that are already available.**

For example, at Uber, multiple teams are dedicated to scalable compute infrastructure, Kafka as a service, ETL as a service, data warehouse services, database services, data lakes, data ingestion as a service, etc. In such an environment, the complexity of the architecture is secondary to reducing team friction with a clear division of responsibilities. As a result, no one worries about having a dependency (often unnecessary) on [Kafka](https://kafka.apache.org/), [HDFS](https://hadoop.apache.org/), or [Cassandra](https://cassandra.apache.org/) and splitting a service into a large number of microservices. The organization staffs teams with deep technical expertise for each type of service and can, therefore, operationalize complex architectures supported by a plethora of in-house tools and machinery.

**This is not possible for every organization. As a consequence, when a new company is formed to productize open-source software developed in larger companies, the team needs to revisit many architectural choices to make the software broadly usable.** These teams have a hard time understanding their real users. They continue to build the solutions as if their target customer is still their previous company (as happened in the case of Hortonworks, a company I co-founded). This is the main reason why we decided to build OpenMetadata from the ground up instead of open-sourcing [what we had built at Uber](https://www.uber.com/en-AU/blog/ubers-journey-toward-better-data-culture-from-first-principles/).

A second influence on the architecture is the **Metadata system as a Product vs as a Service**. The second-generation systems mentioned in the LinkedIn blog provide metadata systems as a product. Instead of optimizing for organizational boundaries and teams, the primary focus is on User Personas and Use Cases. The product team owns the responsibility for all aspects of the product: when pipelines that crawl and publish metadata break, or the Kafka cluster used for ingesting metadata has problems, or the database has issues. There is no luxury of a Kafka team, a pipeline team, a database team, or infra/SRE teams to run to. A product must be delivered as a whole instead of bits and pieces of services to fit into an existing stack. **The simplicity of these architectures is by design**. The architecture must rely on fewer proven dependencies that customers can understand and operate.

OpenMetadata takes the product approach to architecting the system. The dependencies are kept to an absolute minimum — [Jetty](https://eclipse.dev/jetty/), [MySQL](https://www.mysql.com/), and [ElasticSearch](https://www.elastic.co/elasticsearch) are proven and well-known technologies that our users can operationalize. All our schemas are already modeled, strongly typed, and available to our users out of the box, so they don’t need to spend time on modeling. Metadata is extensible — you can define your own tag categories and introduce extensions to existing entities. We provide rich APIs to publish/consume metadata and receive metadata events designed for developers to consume easily. **As a user, you have the flexibility to keep the architecture simple**. But, if you still want to use Kafka for publishing metadata to services that have interfaces to consume from it, there are integration points to do just that.

Push-based vs. Pull-based confusion
-----------------------------------

There is a lot of confusion around these terms. There are two directions in which metadata flows — metadata ingestion from metadata sources to metadata store and metadata consumption from metadata store by applications.

Metadata ingestion
------------------

In all metadata systems, including OpenMetadata, there are crawlers/ETL ingestion jobs that pull the metadata from the source and are pushed into the metadata store using APIs, resulting in a pull-push design.

![Image 4](https://miro.medium.com/v2/resize:fit:700/0*5PNHitnHQN5zxU_1)

Perhaps due to LinkedIn heritage, Datahub turns this into pull-push-pull-push — pull from metadata sources and push into Kafka, pull from Kafka, and then push into metadata store.

![Image 5](https://miro.medium.com/v2/resize:fit:700/0*3GRcM1fq0bZfUjrV)

This might be a reasonable choice for some companies that have a central Kafka team, and the architectural pattern commonly used is to throw data onto Kafka to get it from one place to the other. However, this extra dependency on Kafka adds no tangible benefit. On the contrary, it increases the system dependency, adds operational complexity, and reduces the overall availability of the system with complex failure modes. The often-cited benefit that ‘streaming metadata keeps metadata fresh and avoids ingestion job failures’ is not valid for most metadata sources (with the exception of [Apache Hive](https://hive.apache.org/)) as they don’t produce metadata change events. **No metadata system can be purely push-based**. Even if such metadata change events become available (don’t hold your breath), other metadata, such as queries, data profiling can’t be done in a streaming fashion. So, every system must run some kind of batch job against metadata sources and can’t avoid building strategies to keep the data as fresh as possible, handle job failures, and not overwhelm the sources with a heavy query load. Btw, Uber’s Databook is classified as a third-generation architecture that ingests metadata with a pull-push strategy.

**So, as far as ingestion of metadata is concerned, all systems are pull-based.**

Metadata consumption
--------------------

Applications consume metadata using APIs. They can keep in sync with the changing metadata by consuming metadata change events. This can be done in several ways:

*   Use APIs to pull the change events periodically. This is the simplest way to keep metadata in sync.
*   Use APIs to subscribe for change events and get notifications as webhook callbacks.
*   If your deployments need the change events from Kafka (where your backend systems already have Kafka clients), you can set up a change event sink to Kafka.

Here again, the dependency on Kafka is optional to keep the system as simple as possible. Even webhooks, which require a server to receive metadata POST events, are optional, and one can just use pull requests to keep the architecture simple.

**The architecture needs to support both pull and push-based metadata consumption.**

Schemas
-------

One of the biggest issues with data is that poorly designed schemas make data hard to use. Undocumented schemas mean that data consumers don’t understand their data or, worse, misunderstand their data. As data people, we should take these lessons to heart. We need to create well-modeled schemas that are strongly typed and well-documented with clear vocabulary. Metadata can’t be stored in property bags that no one other than the producer of metadata can understand. Even approaches where metadata is modeled using an array of facets or aspects are just another way to model data as key values with no clear indication of what key values can be expected and what are optional. Unlike a metadata-as-a-service solution, which leaves metadata modeling to the users, the metadata-as-a-product approach must understand the user’s needs, and model all the entities and types required by most users. For a few sophisticated users, schemas should be extensible with clear extension points.

OpenMetadata considers schemas to be the most important aspect of the system. We model schemas using [JSON Schema](https://open-metadata.org/schema), a powerful way to model generate language bindings to any language of your choice, and aided by [excellent tools](https://json-schema.org/implementations.html). Open metadata standards can help in the ubiquitous use of metadata to go beyond catalogs limited to discovery toward data collaboration and automation. You can look at our schemas [here](https://open-metadata.org/schema).

**Metadata must be well-modeled, strongly typed, and clearly documented in order to be shareable and ubiquitously used to power innovations.**

APIs
----

APIs are driving digital transformation across many industries. We believe metadata APIs are at the heart of transforming data. Metadata as a product approach must understand developer needs and provide well-designed, easy-to-use APIs to unlock the innovation. APIs are a key differentiating factor between metadata systems. Most systems lack APIs, and when available, they are unusable, and the best practices of API design are not adopted.

The following are critical for ubiquitous metadata use:

*   **CRUD APIs** to create/retrieve/update/delete entities and relationships
*   **Listing APIs** with pagination support
*   **Events API** for pulling metadata events or pushing metadata events with webhooks support
*   **Search APIs** for both keyword and advance searches
*   **Suggest APIs** for building user interfaces

Requests and responses should have well-designed, strongly typed schemas with clear descriptions. A lot of time and effort has gone into building OpenMetadata APIs, and all the above APIs are supported. You can see more details about our APIs [here](https://sandbox.open-metadata.org/docs).

**Well-designed, easy-to-use, and comprehensive APIs simplify metadata consumption and enable a new generation of metadata-powered applications and automation.**

Putting it all together
-----------------------

Metadata ingestions and consumption interfaces supported by OpenMetadata are shown below. As a user, you have the flexibility to use different ingestion/consumption interfaces depending on the level of complexity you can handle and the support afforded by your technology stack. For most users interested in simplicity, the ingestion and consumption paths shown in yellow are optional. Sophisticated users can set up more advanced ingestion and consumption features for better metadata freshness when sources can generate change events.

![Image 6](https://miro.medium.com/v2/resize:fit:700/0*fAuTykGhEZRioAUd)

Conclusion
----------

Let’s go back to the questions we started with. What generation of architecture is OpenMetadata? As you can guess, the terms currently used are not relevant beyond LinkedIn's evolution. OpenMetadata is a ‘metadata as a product’ system with the goal of centralizing all metadata using Open Standards and making it shareable by all the tools in the data ecosystem using well-modeled schemas and easy-to-use APIs. The focus is on architectural simplicity with minimum dependency to help our users easily operationalize the system.

Is OpenMetadata pull-based, push-based, or hybrid? Again, all systems are mainly pull-based to integrate with metadata sources. We support push-based ingestion when it is possible. As far as metadata consumption is concerned, we support both pull-based and push-based with Events API. Our focus is on serving user needs with the flexibility of push/pull with easy-to-use APIs.

OpenMetadata is a fresh start on how to do Metadata right from first principles. We are employing what we learned from attempts to build three metadata systems over a decade. Metadata should be a solved problem by now. However, many in-house systems and other solutions are still being built in this space. _If you are building an in-house system, join our fast-growing community so we can build it together. If you are stuck with a legacy system, let’s build a migration path together toward a better solution._ With metadata standards and APIs, the data ecosystem can go a long way to eliminate narrow tools, simplify architecture, and unlock innovation. It takes an open-source community to get there.

We have created [good first issues](https://github.com/open-metadata/OpenMetadata/issues?q=is%3Aissue+is%3Aopen+label%3A%22good+first+issue%22) to get you going. If you have any questions about [code](https://github.com/open-metadata/OpenMetadata), [installation](https://docs.open-metadata.org/install/run-openmetadata), or [docs](https://docs.open-metadata.org/), please contact us on [Slack](https://slack.open-metadata.org/). If you have feature requests, please file a [GitHub issue](https://github.com/open-metadata/OpenMetadata/issues) or contact us on [Slack](https://slack.open-metadata.org/).

[![Image 7](https://miro.medium.com/v2/resize:fit:700/1*B5kZqafplTME5JQ_COWGwA.png)](https://cloud.getcollate.io/?pk_campaign=14_day_trial&pk_source=medium&pk_medium=large_rectangle)
