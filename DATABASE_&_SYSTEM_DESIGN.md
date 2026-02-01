# DATABASE AND SYSTEM DESIGN

## Significane of Database Design
* Ensures data integrity & consistency
* Reduces redundancy & duplication
* Improves query performance & efficiency
* Supports scalability & flexibility
* Simplifies maintenance & updates
* Enhances security & access control

## NORMALIZATION
Database normalization is a database design principle for organizing data in an organized and consistent way. 

### Purpose of Normalization

* Avoid complexities
* Eliminate duplicates
* Organize data in a consistent way

### What is 1NF 2NF and 3NF?

1NF, 2NF, and 3NF are the first three types of database normalization. They stand for first normal form, second normal form, and third normal form, respectively.

There are also 4NF (fourth normal form) and 5NF (fifth normal form). There’s even 6NF (sixth normal form), but the common normal form you’ll see out there is 3NF (third normal form).

#### The First Normal Form – 1NF
For a table to be in the first normal form, it must meet the following criteria:

* Row order shouldn't convey any information
* All values in a column are of the same data type
* There must be a primary key for identification
* A single cell must not hold more than one value (atomicity)

### The Second Normal Form – 2NF
The 1NF only eliminates repeating groups, not redundancy. That’s why there is 2NF.

A table is said to be in 2NF if it meets the following criteria:

* It’s already in 1NF
* Has no partial dependency. That is, all non-key at Query Execution Plan in SQL attributes are fully dependent on a primary key.

### The Third Normal Form – 3NF
When a table is in 2NF, it eliminates repeating groups and redundancy, but it does not eliminate transitive partial dependency. 

This means a non-prime attribute (an attribute that is not part of the candidate’s key) is dependent on another non-prime attribute. This is what the third normal form (3NF) eliminates.

So, for a table to be in 3NF, it must:

* Be in 2NF
* Have no transitive partial dependency.

## Schema Design Best Practices

* Use normalization (up to 3NF) to avoid redundancy.
* Use consistent naming.
* Add constraints (NOT NULL, UNIQUE, CHECK).
* Use indexes for frequently searched columns.
* Use junction tables for many-to-many.

## Clustered Index & Non-Clustered Index

The table data itself is arranged (clustered) in order of the index. There can only be one clustered index per table (because data rows can only be physically sorted one way).

While non-clustered index is a separate structure that stores the index + a pointer (reference) to the actual row in the clustered index. You can have many non-clustered indexes. Faster lookups on non-PK columns, but requires an extra step to fetch the row.

## SQL Query Optimizations

* Use Indexes Wisely - primary index, secondary index, clustered index non clustered index
* Avoid SELECT *
* Limit Rows with WHERE and LIMIT
* Use smart joins
* Avoid N+1 Query Problems
* Consider denormalization fIn SQL Server, execution plans can be viewed as Graphical, Text, or XML formats.or performance
* Partition large tables

## Query Execution Plan in SQL

An execution plan is a roadmap that shows how SQL Server retrieves the data for a query. It breaks down the exact steps—like which indexes to use, how tables are joined, and in what order operations are performed. The query optimizer creates this plan, evaluates multiple options, and chooses the most efficient one. Once generated, plans are stored in the plan cache for reuse. 

In SQL Server, execution plans can be viewed as Graphical, Text, or XML formats.

### Types of Execution Plan in SQL

1. Actual Execution Plan

The actual execution plan is produced after the query has been executed. It reflects the real operations carried out by SQL Server, along with runtime performance details. Displays the actual rows processed and other execution statistics.

Usage: `EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'a@b.com';`

2. Estimated Execution Plan

The estimated execution plan is created before the query executes. It represents the query optimizer's prediction of how the query will run, based on database statistics, schema and indexes.

Usage: `EXPLAIN SELECT * FROM users WHERE email = 'a@b.com';`

## Database sharding and partitioning

Sharding and partitioning are techniques to divide and scale large databases. Sharding distributes data across multiple servers, while partitioning splits tables within one server. Partitioning and sharding are two common ways to improve performance, manageability, and availability of larger databases.

### What is sharding?

Sharding, also known as horizontal partitioning, is a database partition approach that divides the database schema and distributes them across multiple instances or servers into smaller parts that are faster and easier to manage. When a database is sharded, a replica of the schema is created. This is then used to divide data to be stored in a shard based on a shard key. To make this possible, a special logic or identifier called a "shard key" is used to determine which specific instance or server holds the data to query.

**What are the advantages of using sharding?**

* Improved response time
* Maintenance tasks, like backups, take less time to complete
* Schema migrations complete faster
* Increased read/write throughput
* Increased storage capacity
* Improved availability
* Outages are more isolated and less impactful

### What is partitioning?

Partitioning is just a general term referring to the process of dividing tables in a database instance into smaller sub-tables or partitions. These partitions can be accessed and managed separately to enhance performance, maintainability, and availability of the database.

## Caching

In computing, a cache is a high-speed data storage layer which stores a subset of data, typically transient in nature, so that future requests for that data are served up faster than is possible by accessing the data’s primary storage location. Caching allows you to efficiently reuse previously retrieved or computed data.

The data in a cache is generally stored in fast access hardware such as RAM (Random-access memory) and may also be used in correlation with a software component. A cache's primary purpose is to increase data retrieval performance by reducing the need to access the underlying slower storage layer. Trading off capacity for speed, a cache typically stores a subset of data transiently, in contrast to databases whose data is usually complete and durable

Purpose - Speed up frequently accessed data retrieval process

### Redis

Redis is designed specifically for speed. It’s an “in-memory” database, storing data right in your computer’s active memory (RAM). This makes retrieving data lightning-fast because it’s already in the workspace where your computer actively operates.

On the other hand, traditional databases like MySQL or MongoDB store data on secondary storage, like a hard drive. While effective for many tasks, fetching data from secondary storage takes more time compared to fetching it from active memory (RAM). An additional advantage stems from the ease of implementing data structures in memory compared to their on-disk counterparts. Redis operates on a single-threaded model.

*In the realm of computer memory, registers and CPU cache are the speedsters, faster than RAM. Found within the processor, they operate at lightning speed. However, due to their high cost and limited capacity, they’re like the Formula 1 racers of memory — incredibly fast but reserved for the most critical tasks. For everyday use and storing larger amounts of data, RAM and in-memory databases like Redis take the lead.*

#### I/O Multiplexing

I/O multiplexing allows Redis to monitor multiple connections simultaneously without blocking its main thread. Instead of waiting for data on a single connection, Redis can keep an eye on multiple connections at once. Redis uses the select() or poll() system call to register interest in multiple sockets (connections) simultaneously. These calls allow it to specify a set of sockets it wants to monitor for specific events, such as readiness to read. This `select()` or `poll()` system calls fall under the umbrella of I/O monitoring system calls. Redis’s single thread invokes the `select()` or `poll()` system call and enters a state of waiting for events. During this time, Redis is not actively processing any specific connection; instead, it awaits notifications about events on the registered sockets and process the requests from the ready sockets one at a time. 
Redis beautifully exploits the fact that network I/O is much time taking than Redis’s in-memory operations (which are atomic) and thus redis can provide high throughput, low latency and this apparent but performant concurrency.

When an event occurs on any of the registered sockets (e.g., data becomes available for reading), the select() or poll() call returns. The return value indicates which sockets experienced events, allowing Redis to identify where data is ready to be processed. Redis then proceeds to handle the specific events on the sockets identified by the system call. For example, if data is ready to be read on a particular socket, Redis can initiate the read operation without waiting, addressing the blocking nature of traditional I/O. The event-driven approach is asynchronous; Redis doesn’t actively poll each socket but rather responds to events as they occur. This allows Redis to efficiently manage a large number of connections without wasting resources on constant polling. By waiting for events rather than blocking on individual sockets, Redis maximizes the utilisation of its single thread and system resources. This ensures that the server remains responsive to events across multiple connections without unnecessary delays.

#### I/O Monitoring System Calls

I/O monitoring system calls are functions provided by the operating system that allow your program to keep an eye on multiple input/output sources, like file descriptors or network sockets, at the same time.

### Memcached

Memcached is a general-purpose distributed memory caching system which was developed in 2003 and quickly found widespread use as a means to reduce the load on databases. It is a network-aware cache that store key-value data in RAM. Thus, it is not persistent storage and if the machine on which it runs crashes, the data is lost. It uses multiple cores, so it utilizes multi-threading

## SYSTEM DESIGN

System Design is the process of designing the architecture, components, and interfaces for a system so that it meets the end-user requirements. System design is important to build a robust, scalable, and efficient software application. Whether you are building a small-scale application or a large one, understanding system design allows you to architect solutions that can handle real-world complexities.

* Scalability and Reliability: System design ensures systems can grow and handle increased demand without failure.
* Efficient Resource Management: It helps in optimizing resource allocation, ensuring fast and responsive applications.
* Adaptability: System design enables the creation of systems that can evolve with changing business needs, reducing long-term costs.
* Architectural Understanding: Learning different system architectures (e.g., microservices, monolithic) helps in building applications suited to various needs.

The goal is to create a well-organized and efficient structure that meets the intended purpose while considering factors like scalability, maintainability, and performance. 

System Design can be divided into two complementary parts: Hight-Level Design (HLD) and Low-Level Design

### High-Level Design

It lays out the overall architecture of the system — how major components interact, what services or modules will exist, and how data flows among them.

* Gives you the big picture: how the system fits together, its core structure, and major decisions.
* Done by architects, stakeholders and managers
* Basic Coding Skills (Data Structures and Algorithms)
* Compared to Low Level Design, High Level Design is typically done by more senior people who have hands-on experience on software projects.
* Knowing the roles of components like databases (SQL and NoSQL), caches (Redis, Memcached, CDNs), and APIs.
* Low Level Design (Object Oriented Programming and Design Patterns)
* In-depth understanding of Functional Requirements (What the system must do e.g., user registration, streaming) and Non-Functional Requirements (How the system should perform—scalability, latency, availability, security, etc)
* Networking and Security Fundamentals like DNS, protocols (TCP/UDP, HTTP, WebSockets), OAuth, JWT, TLS/SSL, rate-limiting, API security, and basic DDOS protection
* Message queues and streaming tools like Kafka or RabbitMQ.
* Knowledge of Microservices vs. Monoliths (When to split services and how to manage dependencies), fault tolerance, fallback strategies, redundancy, Load Balancer Types & Algorithms.
* Observability tools like Prometheus, Grafana, ELK Stack (Elasticsearch, Logstash, Kibana) and Alerting systems (e.g., PagerDuty)

#### More on: https://www.geeksforgeeks.org/system-design/getting-started-with-system-design/

### Functional and Non Functional Requirements

**READ ON: www.geeksforgeeks.org/software-engineering/functional-vs-non-functional-requirements/**
