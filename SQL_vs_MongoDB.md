# Overview

The relational database has been the foundation of enterprise applications for decades, and since MySQL’s release in 1995 it has been a popular and inexpensive option, especially as part of the ubiquitous LAMP stack underpinning early web applications.

Today, modern enterprises are thinking about better ways to store and manage their data -- whether it's to gain better customer insight, adapt to changing user expectations, or beat competitors to market with new applications and business models. As a result, many of the assumptions that drove the development of earlier relational databases have changed:

As a result, non-tabular databases, like MongoDB, have emerged in order to address the requirements of new applications, and modernize existing workloads. And with support for multi-document **ACID** transactions from MongoDB 4.0, it’s now even easier for developers to address use-cases that are now, or will in future, struggle with **MySQL**

# What is MySQL?

`MySQL is a popular open-source relational database management system (RDBMS) that is developed, distributed and supported by Oracle Corporation. Like other relational systems, MySQL stores data in tables and uses structured query language (SQL) for database access. In MySQL, you pre-define your database schema based on your requirements and set up rules to govern the relationships between fields in your tables. Any changes in schema necessitates a migration procedure that can take the database offline or significantly reduce application performance.`

# What is MongoDB?

MongoDB is an open-source, non-relational database developed by MongoDB, Inc. MongoDB stores data as documents in a binary representation called BSON (Binary JSON). Related information is stored together for fast query access through the MongoDB query language. Fields can vary from document to document; there is no need to declare the structure of documents to the system – documents are self-describing. If a new field needs to be added to a document, then the field can be created without affecting all other documents in the collection, without updating a central system catalog, and without taking the system offline. Optionally, schema validation can be used to enforce data governance controls over each collection.

Because documents can bring together related data that would otherwise be modeled across separate parent-child tables in a relational schema, MongoDB’s atomic single-document operations already provide transaction semantics that meet the data integrity needs of the majority of applications. One or more fields may be written in a single operation, including updates to multiple sub-documents and elements of an array. The guarantees provided by MongoDB ensure complete isolation as a document is updated; any errors cause the operation to roll back so that clients receive a consistent view of the document.

MongoDB 4.0 added support for multi-document transactions, making it the only open source database to combine the ACID guarantees of traditional relational databases, the speed, flexibility, and power of the document model, with the intelligent distributed systems design to scale-out and place data where you need it. Through snapshot isolation, transactions provide a consistent view of data, and enforce all-or-nothing execution to maintain data integrity. Transactions in MongoDB feel just like transactions developers are familiar with in MySQL. They are multi-statement, with similar syntax (e.g. start_transaction and commit_transaction), and therefore easy for anyone with prior transaction experience to add to any application.

**Unlike MySQL and other relational databases, MongoDB is built on a distributed systems architecture, rather than a monolithic, single node design. As a result, MongoDB offers out-of-the-box scale-out and data localization with automatic sharding, and replica sets to maintain always-on availability.**

# Terminology and Concepts

Many concepts in MySQL have close analogs in MongoDB. The table below outlines the common concepts across MySQL and MongoDB.

MySQL               |MongoDB
-------------------- -------------------
|ACID Transactions  |	ACID Transactions|
|Table	            |Collection|
|Row	              |Document|
|Column             |	Field|
|Secondary Index    |	Secondary Index|
|JOINs              |	Embedded documents, $lookup & $graphLookup|
|GROUP_BY           |	Aggregation Pipeline|
|-------------------|---------------------|





