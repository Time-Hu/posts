# When database meets stream computing: the birth of the streaming database!

## Database history review

The database, the cornerstone of modern software systems, has been studied for a long time, and although the database field has so far produced four Turing Award winners, today it remains one of the most dynamic and innovative areas of computer science. Since the 1970s, when Edgar F. Codd proposed the relational model, relational databases based on the IBM SystemR prototype have grown rapidly and have been successful in many industrial applications, and even today they remain the absolute mainstream of the database market.

With the rise and boom of the Internet in the 21st century, traditional relational databases began to struggle to meet the needs of the fast-growing Internet business. Against this background, the NoSQL campaign was launched, starting the era of database systems from being the only relational databases to the various database systems. This period saw the birth of many excellent database systems, including but not limited to:

- Document databases represented by MongoDB, CouchDB
- Wide column databases represented by HBase, Cassandra
- A distributed, strongly consistent relational database represented by Google Spanner, including its open source implementations CockroachDB, TiDB
- Analytical databases based on columnar storage represented by Apache Druid, ClickHouse
- Time series databases represented by InfluxDB, TimescaleDB
- Full-text indexing databases represented by Elasticsearch
- Graph databases represented by Neo4j

![image.png](https://static.emqx.net/images/0b642fae86c9de4dcf90ee0195ab9316.png)

Figure 1: The number of different types of databases counted on db-engines

> Figure source: https://db-engines.com/en/ranking_categories

Behind the continuous development and evolution of database systems is the process of the continuous penetration of software systems into various industries of society under the wave of global informatization in recent decades, while at the same time, **different industries, different needs, and different scenarios are constantly putting forward new challenges to database systems. **

So far, no database system can meet the business needs of an entire industry or scenario - or even the entire needs of a company or organization. This is because current real-world business needs are increasingly complex and diverse, often with different requirements for database systems in terms of data storage models, access models, the size and timeliness of the data stored, computational and analytical capabilities, and operational and deployment costs. **Single database systems are generally developed based on particular storage and computational model and thus generally perform well only in their target scenario. **

**With the development of new industries, the opening of new business markets, and new business needs emerge in the future, new database systems are bound to emerge.** For example, the rise of the IoT industry and the AI industry in recent years has led directly to the creation of many new time series and graph databases.

![image2.png](https://static.emqx.net/images/316ab8bd26598bf9f9a05fae921beaf6.png)

Figure 2: Change in prevalence of various databases since 2013

> Figure source: https://db-engines.com/en/ranking_categories

**New data storage and computing requirements are always a source of motivation for the development of database systems.**


## The rise of real-time stream computing

With the rapid development and penetration of computer and network technology into all industries, data is now generated in a much richer way and from a wider variety of sources than ever before, such as data from sensors, user activity on websites, data from mobile terminals and smart devices, real-time trading data from financial markets, data from various monitoring programs and so on. **Much of this data is generated continuously in the form of continuous streams of data from multiple external data sources, and in most cases we have no control over the order in which these streams arrive and the rate at which they are generated. **

According to a report by leading international analyst firm IDC[^1], **this real-time stream data occupies an increasingly large proportion of the overall data size.** 

![image3.png](https://static.emqx.net/images/0b7d29218cd1679d1bda44e70bebd0df.png)

Figure 3: Trends in the growth of real-time data [1]

Traditional data processing systems generally calculate and analyze the intact static data sets already stored in database systems, file systems, and other storage systems, which are unsuitable for this type of continuously generated, infinite, and dynamic data stream. Moreover, traditional batch processing techniques usually have a relatively long time interval between data generation and data being processed. However, in today's highly competitive and complex business environment, **marketing opportunities are fleeting, risk prevention and control must be a matter of seconds, and business decisions must be made quickly and accurately. Therefore, data must be processed in a shorter time to get results, preferably in real-time.** 

Against this backdrop, real-time stream processing techniques are beginning to gradually replace batch processing techniques in an increasing number of scenarios and are taking a central place in the modern data analysis technology stack. **Unlike querying and analyzing static data, stream processing can naturally model continuous streams of data and update computational results in real-time as new data comes in. This allows there to be no delay between 「data generation」 -> 「gaining data insights」-> 「taking action」,** thus allowing companies to remain proactive and ahead of the curve in a highly competitive marketplace.

Today, stream processing systems are widely deployed and used by various enterprises and organizations. All major public cloud providers offer hosted services for streaming data storage and real-time processing, such as Amazon Kinesis, Google Dataflow, and Azure Stream Analytics. The open source community has also seen the emergence of many software systems related to stream processing such as Apache Storm, Apache Flink, Apache Beam, and many more. Over the last few years, more and more companies have built their own internal streaming data platforms to enable the whole organization to take advantage of the power of real-time computing, such as Uber's AthenaX, Netflix's Keystone platform, and Facebook's Turbine. 


## Challenges and opportunities for databases in the streaming era

Although more and more enterprises and organizations today are already recognizing the huge value of stream processing and are beginning to apply and deploy related systems, there is a wide range of open source and commercial stream processing software available. However, it is not easy to implement a full stream processing technology stack. **In the process of actual implementation, it is found that current solutions generally suffer from: complexity and difficulty in use, deployment and operation and maintenance problems, high barriers to entry, miscellaneous components, non-uniform APIs, migration difficulties, etc.** For example, current basic stream processing solutions involve numerous distributed systems and components, including but not limited to.

- Real-time data collection and capture systems  
- Real-time data storage systems
- Stream computing engines
- Downstream data and application systems

This not only brings a great challenge for development and maintenance, but the more different components are introduced, the less reliable the system becomes. At the same time, the components and systems involved in these solutions are not always suitable and efficient, and the integration between them is often not perfect. Therefore, it is often difficult to meet the needs of the business. For example, to achieve exactly once processing semantics and end-to-end consistency, the stream computing engine often relies on the support provided by the stream storage system, a requirement that is difficult to achieve with general integration solutions.

An interesting phenomenon is that we usually use a certain type of database system to meet certain data storage and computation needs. However, **when it comes to real-time data streams, database systems are rarely available.** The reason for this is that, on the one hand, it is difficult for a typical database system to meet the low latency storage requirements of large-scale real-time data streams due to the relatively high overhead of maintaining its storage and indexing structure, resulting in high write latency.

Another more important reason may be related to the 「stereotype」 of database systems: people think that database systems are all working under a request-response model, and is command-driven. In this model, the computation (command) is active and the data is passive. However, **the model of stream processing is the opposite. It is data-driven, i.e. the data is active and the computation is passive, with a constant flow of data through the computation and a continuous update of the results of the computation.** It is difficult to associate stream processing with databases due to this kind of difference.

However, is this true for all database systems? The answer is absolutely no. There is already part of the database that not only supports a request-response command-driven model but also supports a data-driven model. For example, MongoDB's Change Streams feature, RethinkDB's ability to push data in real-time, and TimescaleDB's continuous aggregation feature.

Taking this a step further, **we can fully expect to use a professional database system to solve all our needs for data flow management, such as easy flow calculation via the familiar SQL language.**


## The birth of streaming database

Actually, for storing and processing real-time data streams, we don't need a bunch of fragmented ETL tools, message middleware for temporary data storage, or isolated stream computing engines, we just need **a database system designed for streaming data, the streaming database.**

The streaming database is not a new concept that is invented by us. It was even explored in a paper entitled Continuous queries over append-only database[^2], published in SIGMOD in 1992. Although the concept of a streaming database was not explicitly introduced in this paper, our streaming database is in line with the main idea of the paper.  

Besides, we believe that unlike other database systems that use static data sets (tables, documents, etc) as the basic storage and processing unit, the streaming database uses dynamic continuous data streams as the basic object, and uses real-time as their main feature. Streaming databases are a re-architecture and redesign of databases in the streaming era.


## Summary

The development of database systems has already entered a new era. It's time to break away from the old concepts and create the future of streaming databases. In the era background of increasing needs for computation and storage, **we believe that the streaming database will have a great future. **

In our next article, we will continue our in-depth discussion on the storage of streaming databases, so we can explore what an efficient and reliable streaming database should look like.

Also, we welcome you to join us at [EMQ](https://www.emqx.io/) to discover and create infinite possibilities for the streaming database.


[^1]: Rydning, David Reinsel–John Gantz–John. "The digitization of the world from edge to core." Framingham: International Data Corporation (2018).
[^2]: Terry, Douglas, et al. "Continuous queries over append-only databases." Acm Sigmod Record 21.2 (1992): 321-330.