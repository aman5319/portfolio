---
title: "Big Data Processing"
author: "Aman Pandey"
date: "10/06/2023"
editor: visual
categories: [big-data,aws]
description: "Exploring different tools and technologies to store, intergrate, analyse, process, visualize data at large scale in a batch and stream fashion both with custom tools and AWS managed services"
---

My day to day work revolves around understanding the data, analysis, Visualization and eventually Training model on it.

Most of the time the data which I am dealing with is huge in size and one of characteristics of any kind of data is

1. old data gets updated and refreshed
2. New data gets added

Now the frequency part can be from couple of seconds to days to weeks.

As a Data Scientist I never have to be bothered about how exactly the data that is getting generated on the client side is to be stored, updated, processed and it comes to me a ready, for me to train my model upon.

So in the Blog we will talking about some data engineering stuff.

## Basic Terminologies

Before we get into the core part there are some terminologies which are important to understand also I will using AWS services to give my examples.

1. `Data Lake` -> Any place where you can dump any type of data, video dump to s3(simple storage service), text dump to s3, files dump to s3, images dump to s3, blobs. Anything type data that can be stored at a place that place is called data lake.

2. `Data Warehouse` -> The Data which is stored in data lake has to be processed for some purpose may be training model, reporting, analysis it could be anything. Processing the unstructured data from data lake and storing it in a structured way at a particular place that place is Data Warehouse.

Now keen observers can say can't we store the process data to data lake rather than in data warehouse in that case you are absolutely correct, we can store it there.

For ex - Taking bunch of user queries stored in S3 process it create a single csv file and again store it in s3.

Also sometimes we want to store that processed data in Database which is absolutely fine.

3. `Batch Processing` ->  When data processing happens in batches or in scheduled interval for example If we take all the user queries for a particular day and on night if we process all of it that is called batch processing.
4. `Stream Processing` -> When data gets generated at that same time or near real time we try to process it. for example the moment user submits a query we do some processing.
5. `Event` -> when a user clicks a button, when a user writes a query, when a user scrolls , everything is an event and it depends completely on business on fundamental level what do they want to call an event.

## Data Streaming, Ingestion and Processing.

When a event happens how to process that event for example - when on instagram we do share, like and comment, couple of things need to be done, first the UI has to be updated with an increement, and the data has to stored in some type of database.

so when a event happens there can be n things which needs to be updated on the same time.

Here comes `Apache Kafka` to Rescue

Apache Kafka is a open-source distributed event streaming platform. i.e in simple terms when you million of users sending different types of queries now how exactly are suppose to handle each query which dashboard, database, UI component has to be update all is handled by Apache Kafka. It helps integrate data from a variety of sources and sinks

Kafka follows a publish-subscribe model, where data producers (publishers) send messages to Kafka topics, and data consumers (subscribers) can subscribe to those topics to receive and process the messages.

![](images/kafka_keyword_diagram.svg)

To draw an analogy we can take example of google map as we enter source and drop location map shows us the way but we have to go by ourself by driving or walking.

So, Kafka is often used in conjunction with stream processing frameworks like Apache Flink, Apache Beam, Kafka Streams etc to perform real-time data processing and store the processed data for something.

Now either we deploy apache kafka and manage by our own self our use `AWS Kinesis`. They both serve the same purpose.

`Apache Flink` - It is a framework and distributed processing engine for stateful computations over unbounded and bounded data streams. Flink has been designed to run in all common cluster environments, perform computations at in-memory speed and at any scale.

![](images/flink.png)

For example -

1. Take example of any Algo Trading bot for every news and fluctuation that happens inside the market the data has to be quickly processed and understand, flink would be a suitable candidate.
2. Calculate metrics like click-through rates (CTR) for products.
3. Detect patterns, such as identifying when users abandon their shopping carts.
4. Perform sessionization to group user interactions into sessions for personalized recommendations.
5. Detect anomalies or fraud in real-time, such as unusual purchase patterns.

To draw a similarity `Kinesis Data Streams` does the exact same thing.

Note:-  Neither Flink nor Kafka provide its own data-storage system, but provides data-source and sink connectors to systems.

If the data needs to stored some where directly may be Data Lake, Database, files.we can use `Kafka Connect` which connects Kafka with external data sources and sinks. It helps to ingest data.

![](images/inside-kafka-connect.jpg)

To draw a similarity `Amazon Kinesis Firehose` provides the same functionality.

Amazon Kinesis Firehose is a fully managed AWS service that simplifies the process of ingesting, transforming, and delivering real-time streaming data to other AWS services, such as Amazon S3, Amazon Redshift, or Amazon Elasticsearch.
Kinesis Firehose is primarily used for data ingestion and data loading into AWS services. It's particularly useful for situations where you want to move data from a streaming source to AWS data storage or analytics services without complex processing.

`Debezium` - It is an open-source platform for change data capture (CDC) that captures and streams database changes in real-time. It allows you to monitor and react to changes in your databases as they occur, making it a valuable tool for building real-time data pipelines, event-driven microservices, and other applications that require access to fresh, up-to-date data.


Just to sum it up we have seen how to stream events using kafka store it using kafka connect, some stream processing using Flink and react to certain changes in the database using debezium.

Now when it comes selection of the Database we have plathora of option with pros and cons of each and we will look the pros and cons of each one of them some other day.


## Data Storage and Querying

Storing the processed data in Data Lake vs. Data Warehouse, often depends on factors like data volume, query performance requirements, and cost considerations.

Now let suppose their is an example scenario where some data from data lake is processed 

1. Either we can store the processed data back to data lake.
2. Store the processed data onto some Database.

For case 1 - we can have a big csv file or parquet file which has all the processed data, now what all things we can do

1. Use directly in model training, analysis.
2. Or there is need to query that particular data which is stored in the data lake may be run a SQL query to fetch a subset of the data in that case use `Presto` a distributed SQL query engine. 

![](images/presto.svg)

Presto acts a single query engine for data engineers who struggle with managing multiple query languages and interfaces to siloed databases and storage, Presto is the fast and reliable engine that provides one simple ANSI SQL interface for all your data analytics and your open lakehouse.


on AWS `Athena`  gives the same functionality, actually `Athena` is built on top of presto.

Example - let suppose we store some processed data into TB scale into S3 now what we can do is use Athena to query that data using SQL syntax.


For case 2- It is simple that we directly dump the data on to a Database which is designed for business use case some one is looking for. `Amazon RedShift` is a good option.

Also many times we just need data integration service where we just want to move data from different sources to one single source for different application. for example we want to perform some ETL on data stored in 5 different DBs to one single source in that AWS provide `Glue`

![](images/glue.png)
We now know how to store the processed data and query it.



## Batch Processing

We looked that we can use Flink to do stream processing now when it comes to batch processing of data we can use
all time fav tool `Apache Spark`.

Based on the data size you might need processing power from 1 node to multiples clusters.

In case we have a single computer with lot of computer power we can do parallel processing by our selves, without requiring any special library but when it comes to distributed processing using a matured library is always recommended.

Now most of the distributed system works in a Master-Slave configuration where there is one master node and multiple slave nodes. the master schedules and monitors the job and slave nodes actually perform the job.

Since the work is distributed across cluster we need a cluster manager also, Apache Mesos or Kubernetes is used to manage the allocation of resources (CPU and memory) for Spark applications.

![](images/spark.png)


On AWS we can run spark jobs on `Amazon EMR(Elastic Map Reduce)`


![](images/s.png)


## Data visualization

The data is processed now stored in some place now there is need to visualize now before visualizing there is we need to query the data and the most important part of querying is
1. how quickly we need the data. 
2. how many people are querying it at a given moment.
3. what is the size of the database we are querying.


Take a step back on to data processing system,
when a database is designed either it is designed for heavy read operation OLAP or Write operation OLTP

OLTP (Online Transaction Processing):

1. OLTP databases are designed for transactional processing and day-to-day operations. 
2. They handle tasks like order processing, inventory management, and customer record updates.
3. OLTP systems are optimized for write-heavy operations and maintaining data integrity.
4. Data consistency is critical in OLTP databases, and transactions are ACID-compliant (Atomicity, Consistency, Isolation, Durability).
5. OLTP queries are relatively simple and involve tasks like INSERT, UPDATE, DELETE, and SELECT of individual records. Response times are typically low to ensure efficient transaction processing.
6. So banks, atms, ecommerce websites are using OLTP for many write operations.



OLAP (Online Analytical Processing):
1. OLAP databases are designed for complex queries and reporting.
2. They are used for business intelligence, data analysis, and decision support systems.
3. OLAP systems are optimized for read-heavy operations and are well-suited for data analysis tasks.
4. The data is denormalized, meaning redundant data is stored to optimize query performance.

Now that we know about data processing databases Let's take two OLAP scenario-
1. Every morning some manager at linkedin wants to tracks some numbers may be Active users, interaction etc.
2. Uber Eats wants to show real time analytics to the restaurant owner, for the owner to take quick decision.

The difference in both the use case is given a query how much time it has before it returns a result.

In case 1 - query take 5hrs, 6hrs hell even 10hrs we don't care because it once everyday, here we can use any query engine or database we have Athena, Redshift, Postgres, MongoDb it doesn't matter.

In case 2 - query has to execute within milliseconds. Here something different is needed we can't rely on let say redshift to quickly query PB scale data and give result in milliseconds.

Let me introduce `Apache Pinot` - Realtime distributed OLAP datastore, designed to answer OLAP queries with low latency

![](images/pinot.png)

How do they do it to make any read operation fast we have to index the data, if we slice and dice the data to maintain 100's of indexes across different dimension we can make the read of different queries faster.

Apache Pinot allows RealTime user facing analytics.

Once the query is executed use any data visualization tool to visualize the data we have many options
Apache SuperSet, Amazon QuickSight, Tableau

![](images/superset.jpg)




## Job Scheduling

How do we schedule our ETL(Extract Transform Load) jobs, we saw how spark can do batch processing, superset can do visualization  for us but the question now is how do we schedule the job based on conditions, triggers and at particular time.

`Apache Airflow` is a full blown platform for orchestrating, scheduling, and monitoring complex workflows of data pipelines, ETL (Extract, Transform, Load) processes, and other data-related tasks. It provides a framework for defining, scheduling, and executing workflows as a directed acyclic graph (DAG), making it a powerful tool for automating and managing data workflows.

It also comes with parallel and distributed execution engine, error handling capabilites, monitoring and logging.

Apache Airflow can be said as Cron on steroids.

![](images/airflow.png)



## Bakup and Archival of Data

Backup and Archival are also important parts of Data Management

`AWS Backup` is a centralized, fully managed backup service that streamlines the process of protecting your data across various AWS resources, including EC2 instances, RDS databases, EBS volumes, and more. 

`AWS S3 Glacier` complements AWS Backup by offering a cost-effective solution for long-term data archiving. Here are the key features of AWS S3 Glacier:

Low-Cost Storage: S3 Glacier offers significantly lower storage costs compared to standard AWS S3 storage classes. It's ideal for archiving data that is rarely accessed but needs to be retained for compliance or historical purposes.

Tiered Storage Options: Glacier provides different storage tiers to meet specific retrieval time requirements. You can choose between three retrieval options: Expedited, Standard, and Bulk, depending on the urgency of accessing archived data.

Data Lifecycle Policies: Automate data lifecycle management with policies that transition data from frequently accessed S3 storage classes to Glacier after a certain period. This helps optimize storage costs without manual intervention.

Vaults and Archives: S3 Glacier organizes data into "vaults," and each vault can contain multiple "archives." Archives are individual objects or files that can be stored, retrieved, and managed as needed.


This below picture can give insights on to how different tools interact with each other.

![](images/overall.jpg)

## Thank you.
