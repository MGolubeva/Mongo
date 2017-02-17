## About

This lessons starts the cycle of overall and interesting lessons about one of the most popular NoSQL database MongoDB. The short history of the appearance and development of both SQL and NoSQL databases, its kinds and properties, advantages and disadvantages are covered in the current lesson. Examples of how a SQL database's data structure to transform into a few various NoSQL databases' structures are also provided here. A lot of illustrations are designed to give a more deep understanding how these data are collected in various databases. This lesson contains an overview about MongoDB including basics of architecture, data storage particularities and fields of its application. It provides also the instruction of MongoDB installation and running on your computer. 

The content of this lesson has a more general nature, but it is very important for our future work with MongoDB.

## Introduction

Every day we deal with a wealth of information: watching TV, reading newspapers or magazines, posting on any social network, etc. It has become commonplace for us, so much so that we no longer notice how much information goes through us.<br/>

According to [IDC](https://en.wikipedia.org/wiki/International_Data_Corporation), in 2011 we created 1.8 zettabytes (or 1.8 trillion GBs) of information, which is enough data to fill about 57.5 billion 32GB Apple iPads. That's enough iPads to build a Great iPad Wall of China two times higher than the original one. In 2012 it reached 2.8 zettabytes and IDC now forecasts that we will generate 40 zettabytes (ZB) by 2020. Every day we create data. It comes from everywhere: sensors used to gather climate information, posts to social media sites, digital pictures and videos, and cell phone GPS signals to name just a few. On average, every minute of every day we create:
<ul>
	<li>More than 204 million email messages</li>
	<li>Over 2 million Google search queries</li>
	<li>48 hours of new YouTube videos</li>
	<li>684,000 bits of content shared on Facebook</li>
	<li>More than 100,000 tweets</li>
	<li>$272,000 spent on e-commerce, etc</li>
</ul>
These figures, to put it mildly, are overwhelming. So a natural question arises: where and how can we store all this data?<br/>

Databases have been created for this aim as an effort to organize collections of data. Today we're familiar with a lot of databases and systems of data management. They vary widely by type, structure, and architecture. The establishment of databases in this form, as we know and use them today, dates back to the 1950s. However, the need to store and sort the data appeared much earlier.<br/>

A database management system (DBMS) handles the way data is stored, maintained, and retrieved. The first large-scale DBMS  was the Information Management System (IMS) created by IBM in the mid to late 1960's to support the Apollo space missions (this was before the first hard drives were invented). IMS used a hierarchical tree structure to store data. IMS offers very fast transactional support and is still in use today.<br/>

The further development of programming languages, algebra, and relational calculus led to the creation of **SEQUEL** (**S**tructured **E**nglish **Q**uery **L**anguage). Promotion of this language to version 2 led to the emergence of a structured query language (**SQL**). This language was approved in 1987 and established as ISO and ANSI standard. This concept remained the king among other approaches for data storage and its management for many decades of the year and maintains its position even today in many leading fields. <br/>

At the turn of the millennium, there was a certain shift in the perception of data. Boundaries in approaches built on a structured data model (relational/object-oriented model) and object application are emerging that have led to dusting off the idea of the unstructured database (called today **NoSQL**). The boom of unstructured databases is associated with Google, with the introduction of their proposal called BigTable database, which is intended for the large amounts of data. 

The biggest difference between SQL and NoSQL was that the last ones were not line-oriented like SQL databases, but column-oriented. <br/>

The modern world was divided into two camps concerning SQL and NoSQL concepts, and today there are discussions about what kind of database is better. But it has become clear that, in an era of tremendous prosperity and the need for technology to work with Big Data, NoSQL is the favorite in this competition.<br/>

This lesson opens the door to the world of one of the most commonly used NoSQL databases: MongoDB. Our journey through this world will last 8 weeks, and during this time you will come to know and understand all its beauty. Good luck!<br/>

## Relational (SQL) and non-relational (NoSQL) databases

So, as mentioned above, in the world of database technology there are two main types of databases: SQL and NoSQL - or relational databases and non-relational databases. The difference speaks to how they are built, the type of information they store, and how they store it.

Relational databases are structured like phone books that store phone numbers and addresses or like the well-known Excel table. Non-relational databases are mostly document-oriented and distributed like file folders that hold everything from a person’s address to his shopping history. SQL and NoSQL databases are named based on whether or not they’re written solely in structured query language (SQL). 

### SQL databases
The relational (SQL) database is a more rigid, structured way of storing data. It consists of one or more tables with columns (fields) and rows (records), where each row represents an entry, and each column sorts a very specific type of information like name, age, profession, address, or phone number. The relationship between tables and field types is called a *schema*. A relational database schema has to be clearly defined before any information can be added.<br/>

The relational database was born in 1970 when *E.F. Codd*, an IBM researcher, wrote a paper outlining the process. Since that time, relational databases have grown in popularity to become a standard.<br/>

With a relational database, you can quickly compare information because of the arrangement of data in columns. The relational database model takes advantage of this uniformity to build completely new tables out of required information from existing tables. In other words, it uses the relationship of similar data to increase the speed and versatility of the database.<br/>

Today there are a variety of Relational Database Management Systems (RDBMS), but [Oracle](https://en.wikipedia.org/wiki/Oracle_Corporation), [MySQL](https://en.wikipedia.org/wiki/MySQL), [Microsoft SQL Server](https://en.wikipedia.org/wiki/Microsoft_SQL_Server), [PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL), [MariaDB](https://en.wikipedia.org/wiki/MariaDB) and [IBM DB2](https://en.wikipedia.org/wiki/IBM_DB2) are the most popular. <br/>

So let’s look how data can be structured and represented in an SQL database. Suppose we have a list of books by several authors we have read or want to read. We can write data about each book’s name, author, genre and year of writing to the table like this

|author|book|genre|year|
|------|----|-----|----|
|Jack London|The Sea Wolf|adventure|1904|
|Jack London|Martin Eden|adventure|1909|
|Agatha Christie|The Mystery of the Blue Train|detective|1928|
|Mark Twain|The Adventures of Tom Sawyer|adventure|1876|
|Mark Twain|The Prince and the Pauper|children literature|1881|

For this table we can apply a variety of operations - sort rows, filter records, find the earliest publication or other aggregation, etc.<br/>

This table contains more complicated records than the first example with people’s addresses and phones because it deals with multiple records. With this short example, we want to demonstrate how such data may be organized in a SQL database. 

The basic concept of the data storage is the **identifier** or **ID**. Each record in the table automatically gets its own ID when it is created. Usually, tables contain many records that share the value of a particular field; in the previous table, they were “author” columns, containing two pairs of the same authors names (Jack London and Mark Twain) and the “genre” column. 

Best practice, in this case, is to separate one table into several tables with unique records for each column with duplicates. For the current example, a SQL database will contain three tables: “authors,” “books,” and “genres” (see the picture below).<br/>

<center><img class="resize" src="https://study.moderndeveloper.com/wp-content/uploads/2016/08/1470660798942.png"></center>

The table “authors” consists of all unique authors mentioned above, and the table “genres” contains all unique genres. In the “books” table, instead of the column “author” the column “author_id” was defined, which is related to the “authors” table by the pair of fields “author_id” (“books”) ↔ “id” (“authors”). Relationships of this nature between two tables are called one-to-many (one2m) because one author can write several books (we suppose that a book may have only one author). 

The field determining the relationship of one table with another one is called **Primary Key**. It is one of the fundamental concepts of relational databases. The “books” table,  in particular, contains two Primary Keys - “author_id” and “genre_id”.<br/>

This was a very simple yet informative example how data can be organized in some SQL databases. In order for the relational database to be effective, it must have a very organized data storage structure. A well-designed schema minimizes data redundancy and prevents tables from becoming out-of-sync, a critical feature for many businesses, especially those that record financial transactions. 

### NoSQL databases
If your data requirements aren’t clear at the outset, or if you’re dealing with massive amounts of unstructured data, you may not have the luxury of developing a relational database with clearly defined schema. These are the situations in which non-relational databases become useful. 

Unstructured data from the web can include sensor data, social sharing, personal settings, photos, location-based information, online activity, usage metrics, and more. Trying to store, process, and analyze all of this unstructured data led to the development of schema-less alternatives to SQL. 

Taken together, these alternatives are referred to as NoSQL, meaning “Not only SQL.” While the term NoSQL encompasses a broad range of alternatives to relational databases, what they all have in common is that they allow you to treat data more flexibly. NoSQL technology was originally created and used by Internet leaders such as Facebook, Google, Amazon, and others, who required database management systems that could write and read data anywhere worldwide while scaling and delivering performance across massive data sets and millions of users.<br/>

Instead of tables, NoSQL databases are document-oriented. This way, non-structured data (such as articles, photos, social media data, videos, or content within blog posts) can be stored in a single document that can be easily found but isn’t necessarily categorized into fields like a relational database does. It’s more intuitive, but note that storing data in bulk like this requires extra processing effort and more storage than highly organized SQL data.<br/>

NoSQL databases do not have a common way to query the data (i.e. similar to SQL or relational databases) and each solution provides its own query system.<br/>

There are four main types of NoSQL databases:

<ol>
	<li><strong>Key-value model</strong> - the least complex NoSQL option. It stores data in the schema-less way that consists of indexed keys and values. The most popular key-value databases are: <a href="https://en.wikipedia.org/wiki/Apache_Cassandra">Cassandra</a>, <a href="https://en.wikipedia.org/wiki/LevelDB">LevelDB</a>, <a href="https://en.wikipedia.org/wiki/Amazon_DynamoDB">DynamoDB</a>, and <a href="https://en.wikipedia.org/wiki/Riak">Riak</a>.</li>
	<li><strong>Column store</strong> - stores data tables as columns rather than rows. It’s more than just an inverted table - sectioning out columns allows you to get excellent scalability and high performance. Examples: <a href="https://en.wikipedia.org/wiki/Apache_HBase">HBase</a>, <a href="https://en.wikipedia.org/wiki/Bigtable">BigTable</a>, <a href="https://en.wikipedia.org/wiki/Hypertable">HyperTable</a>.</li>
	<li><strong>Document database</strong> - takes the key-value concept and adds more complexity, each document in this type of database has its own data and its own unique key, which is used to retrieve it. It’s a great option for storing, retrieving and managing data that is document-oriented but still somewhat structured. Examples: <a href="https://en.wikipedia.org/wiki/MongoDB">MongoDB</a>, <a href="https://en.wikipedia.org/wiki/CouchDB">CouchDB</a>.</li>
	<li><strong>Graph database</strong> - has data that is interconnected and best represented as a graph. This method is capable of lots of complexity when many complex relationships between data exist. Examples: <a href="https://en.wikipedia.org/wiki/Neo4j">Neo4J</a>, <a href="http://titan.thinkaurelius.com/">Titan</a>, <a href="https://en.wikipedia.org/wiki/OrientDB">OrientDB</a>.</li>
</ol>

For your general understanding, let’s demonstrate how the records for books from the last example are represented in a graph database. The records of each SQL table represent a single node (circle on the image below). Records from different tables possess each other according to the specific type of nodes, in general. 

We have used different colors to highlight various types of nodes. The relationships implemented due to primary keys between SQL tables and their records are represented as connections of some type between nodes in graph databases. We’ve called these types “wrote” (an author wrote a book) and “is_genre_of.”

<center><img class="resize" src="https://study.moderndeveloper.com/wp-content/uploads/2016/08/1470665323139.png" width="80%"></center><br/>

With this example we wanted only to show that the same data can be structured in many different ways, i.e. there is some freedom of choice in data organization. The idea that SQL and NoSQL are in direct opposition and competition with each other is flawed one, not in the least because many companies opt to use them concurrently. If your data needs are changing rapidly, you need high throughput to handle viral growth, or if your data is growing fast and you need to be able to scale out quickly and efficiently - maybe NoSQL is for you. But if the data you have isn’t changing in structure, and you’re experiencing moderate, manageable growth - your needs may be best met by SQL technologies.<!--COMMENT; BRAD: we need exercises at the end of each h2 section-->

## Please, meet MongoDB
We have taken a quick look at two main methods of data storage: SQL and NoSQL databases. We've considered their types and mentioned the most popular examples. As you could see, there are a variety of databases and DBMSs. So why we have chosen MongoDB for this course? This chapter will explain why.<br/>

MongoDB is an open source database written in the C++ programming language that uses a document-oriented data model. Instead of using tables and rows as in relational databases, MongoDB is built on an architecture of collections and documents. Documents comprise sets of key-value pairs and are the basic unit of data in MongoDB. Collections contain sets of documents and function as the equivalent of relational database tables.<br/>

MongoDB is designed for problems without heavy transactional requirements that aren't easily solved by traditional RDBMSs, including problems which require the database to span a lot of servers. Like other NoSQL databases, MongoDB supports dynamic schema design, allowing the documents in a collection to have different fields and structures.<br/>

MongoDB was created in 2009 by Dwight Merriman and Eliot Horowitz, who had encountered development and scalability issues with traditional relational database approaches while building Web applications at DoubleClick, an Internet advertising company that is now owned by Google Inc. According to Merriman, the name of the database was derived from the word humongous to represent the idea of supporting large amounts of data.<br/>

The data environment has changed a lot since SQL was first designed in the 1970s, and a few of MongoDB’s most popular uses are great examples of how its NoSQL capabilities can meet the challenges of modern data.<br/>
<ol>
	<li><strong>Location-based data analytics and operations.</strong> If you gather location-based data, MongoDB has built-in spatial functions that allow you to harvest this data from specific locations and put it to use without complicated extraction processes.</li>
	<li><strong>The Internet of Things.</strong> Billions of sensors and connected assets and devices create millions of data points, and it’s a challenge for relational databases to absorb and analyze them without time-consuming ETL (extract, transform, and load) processes. MongoDB can analyze data of any kind within the database itself.</li>
	<li><strong>Get real-time data reporting and analytics.</strong> Pull together data from across different silos seamlessly, and get a real-time, snapshot view of your data that you can use. Trying to consolidate different types of data like this with a relational database is a challenge.</li>
	<li><strong>Powering content management systems (CMS).</strong> Whether your CMS is designed for e-commerce or publishing content, MongoDB is an ideal partner because it can house so many different types of data - the data that drives the necessity of a CMS-powered site. </li>
	<li><strong>Push out new versions of mobile apps fast.</strong> MongoDB’s ability to support fast iterations means you can scale up, make modifications, and give your customers new and better apps fast, and without the cost of updating your relational database management system (RDBMS).</li>
</ol>
Today many large companies use MongoDB. Take a look at a few examples below:
<ul>
	<li>Bosch has built its Internet of Things suite on MongoDB, bringing the power of big data to a new range of Industrial Internet applications including manufacturing, automotive, retail, energy, and many others (in total it collects data from about 40 billion sensors);</li>
	<li>The CERN physics lab is using MongoDB for data aggregation;</li>
	<li>The New York Times newspaper is using MongoDB to support a form-building application for photo submissions;</li>
	<li>An insurance company MetLife is using MongoDB for customer service applications (it built a single view of 100M+ customers and 70 systems in 90 days using MongoDB. (They had tried to build the same application with a relational database for many years);</li>
	<li>The City of Chicago cuts crime and improves citizen welfare with a real-time geospatial analytics platform called WindyGrid. Using MongoDB, it analyzes data from 30+ different departments – like bus locations, 911 calls, and even tweets – to better understand and respond to emergencies;</li>
	<li>Otto, Europe’s second-largest e-commerce company, continually updates its catalog of over 2M products to provide a one-to-one shopping experience for 30M shoppers;</li>
	<li>Forbes gains critical insight into the social sharing of their articles to capitalize on stories going viral in real-time;</li>
	<li>Genentech, a biotechnology corporation, accelerates drug research using MongoDB to capture the variety of data generated by genetic tests.</li>
</ul> 
Those are only a few examples of all those who use MongoDB, but they show how popular, convenient, and functional MongoDB is. A larger list of MongoDB clients can be found at [the official site](https://www.mongodb.com/who-uses-mongodb).<!--COMMENT; BRAD: exercises here-->

## MongoDB architecture and data storage in simple words
MongoDB stores BSON documents, i.e. data records, which are similar in form, view, and structure to JavaScript objects, in collections; the collections are stored in databases. Let's take a look at what that means using our previous example of books and their authors?<br/>

We can organize the structure of MongoDB database in the following two ways. At the moment, it's not important for us which one will be better from technical and performance point of view, we only want to demonstrate how these data can be represented in a MongoDB database.<br/>

At first, we create three JSON-like objects for each author:
<ul>
	<li>The first one</li>
</ul>
<pre><code>
    {
          <span class="key">author</span>: <span class="text">"Jack London"</span>,
          <span class="key">books</span>: [
                   {
                         <span class="key">title</span>: <span class="text">"The Sea Wolf"</span>,
                         <span class="key">genre</span>: <span class="text">"adventure"</span>,
                         <span class="key">year</span>: <span class="num">1904</span>
                   },
                   {
                         <span class="key">title</span>: <span class="text">"Martin Eden"</span>,
                         <span class="key">genre</span>: <span class="text">"adventure"</span>,
                         <span class="key">year</span>: <span class="num">1909</span>
                   }
                 ]
    }
</code></pre>

<ul>
	<li>The second one</li>
</ul>	
<pre><code>
    {
          <span class="key">author</span>: <span class="text">"Agatha Christie"</span>,
          <span class="key">books</span>: [
                   {
                         <span class="key">title</span>: <span class="text">"The Mystery of the Blue Train"</span>,
                         <span class="key">genre</span>: <span class="text">"detective"</span>,
                         <span class="key">year</span>: <span class="num">1928</span>
                   }
                 ]
    }
</code></pre>

<ul>
	<li>The last one</li>
</ul>
<pre><code>
    {
          <span class="key">author</span>: <span class="text">"Mark Twain"</span>,
          <span class="key">books</span>: [
                   {
                         <span class="key">title</span>: <span class="text">"The Adventures of Tom Sawyer"</span>,
                         <span class="key">genre</span>: <span class="text">"adventure"</span>,
                         <span class="key">year</span>: <span class="num">1876</span>
                   },
                   {
                         <span class="key">title</span>: <span class="text">"The Prince and the Pauper"</span>,
                         <span class="key">genre</span>: <span class="text">"children literature"</span>,
                         <span class="key">year</span>: <span class="num">1881</span>
                   }
                 ]
    }
</code></pre>

Above we’ve highlighted the text data in blue, numerical data in green, and the key fields representing the data in red. Each of the above JSON-like objects is a document in MongoDB. They consist of array records “books” containing nested JSON-like objects. All three documents may be joined into a collection of documents; let’s call it “authors”. And in this case a MongoDB database (call it “autors_db”) will contain only this one collection:
<center><img class="resize" src="https://study.moderndeveloper.com/wp-content/uploads/2016/08/1470724137678.png"></center>
Another way we can collect the above data is the following. Let’s create a separate document for each book:
<pre><code>
    {
          <span class="key">title</span>: <span class="text">"The Sea Wolf"</span>,
          <span class="key">genre</span>: <span class="text">"adventure"</span>,
          <span class="key">year</span>: <span class="num">1904</span>
    }
    {
          <span class="key">title</span>: <span class="text">"Martin Eden"</span>,
          <span class="key">genre</span>: <span class="text">"adventure"</span>,
          <span class="key">year</span>: <span class="num">1909</span>
    }
    {
          <span class="key">title</span>: <span class="text">"The Mystery of the Blue Train"</span>,
          <span class="key">genre</span>: <span class="text">"detective"</span>,
          <span class="key">year</span>: <span class="num">1928</span>
    }
    {
          <span class="key">title</span>: <span class="text">"The Adventures of Tom Sawyer"</span>,
          <span class="key">genre</span>: <span class="text">"adventure"</span>,
          <span class="key">year</span>: <span class="num">1876</span>
    }
    {
          <span class="key">title</span>: <span class="text">"The Prince and the Pauper"</span>,
          <span class="key">genre</span>: <span class="text">"children literature"</span>,
          <span class="key">year</span>: <span class="num">1881</span>
    }
</code></pre>
and join books by its author into the various collection, i.e. the first two books we will put in the collection “Jack London”, the third document put into the collection “Agatha Christie” and the collection “Mark Twain” will include the last two documents. In this case, the database “authors_db2” has three collections:
<center><img class="resize" src="https://study.moderndeveloper.com/wp-content/uploads/2016/08/1470724461105.png"></center>
Thus, MongoDB documents are composed of field-and-value pairs and have the following structure:
<pre><code>
    {
          <span class="key">field1</span>: <span class="text">value1</span>,
          <span class="key">field2</span>: <span class="text">value2</span>,
          ...
          <span class="key">fieldN</span>: <span class="text">valueN</span>
    }
</code></pre>
The value of a field can be any of the BSON data types, including other documents, arrays, and arrays of documents. (We will learn more about data types available in MongoDB in the next lessons.) Documents have the following restrictions on field names:
<ul>
	<li>The field name <strong>_id</strong> is reserved for use as a primary key; its value must be unique in the collection and is immutable.</li>
	<li>Field names cannot start with the dollar sign (<strong>$</strong>) character because this sign is used at aggregation operations.</li>
	<li>Field names cannot contain the dot (.) character because this sign is usually used for concatenation with internal fields in nested documents.</li>
	<li>Field names cannot contain the null character (i.e. spaces, tabs).</li>
</ul>

BSON documents may have more than one field with the same name, but this may cause problems when working with database data. So, we strongly recommend using all unique names within a document.<br/>

MongoDB documents can vary in structure, so MongoDB possesses by the dynamic schema. For example, all documents that describe users might contain the user ID and the last date they logged into the system. However, only some of these documents might contain the user’s identity for one or more third-party applications. Fields can vary from document to document. If a new field needs to be added to a document, then the field can be created without affecting all other documents in the system.<br/>

Note, the maximum BSON document size is 16 megabytes. This helps ensure that a single document cannot use an excessive amount of RAM or, during transmission, excessive amount of bandwidth. MongoDB memory maps the database files. It allows the OS to control this and allocate the maximum amount of RAM to the memory mapping. 

As MongoDB updates and reads from the DB, it is reading and writing to RAM. All indexes on the documents in the database are held in RAM also. The files in RAM are flushed to disk every 60 seconds. To prevent data loss in the event of power failure, the default is to run with journaling switched on. The journal file is flushed to disk every 100ms and if there is power loss is used to bring the database back to a consistent state. 

An important design decision with mongo is the amount of RAM. You need to figure out your working set size, i.e if you are going to be reading and writing to only the most recent 10% of your data in the database then this 10% is your working set and should be held in memory for maximum performance. So if your working set is, for example, 10GB you are going to need 10GB for max performance. Otherwise, your queries/updates will run slower as pages of memory are paged from disk into memory.<br/>

Other important aspects of MongoDB are replication for backups and sharding for scaling.<br/>

MongoDB provides horizontal scale-out for databases on low cost, commodity hardware or cloud infrastructure using a technique called **sharding**, which is transparent to applications. 

Sharding distributes data across multiple physical partitions called shards. Sharding allows MongoDB deployments to address the hardware limitations of a single server, such as bottlenecks in RAM or disk I/O, without adding complexity to the application. MongoDB automatically balances the data in the sharded cluster as the data grows or the size of the cluster increases or decreases. Unlike relational databases, sharding is automatic and built into the database, minimizing the burden for developers and operations teams.<br/>

MongoDB maintains multiple copies of data, called replica sets, using native **replication**. A replica set is a fully self-healing shard that helps prevent database downtime and can be used to scale read operations. Replica failover is fully automated, eliminating the need for administrators to intervene manually. A replica set consists of multiple replicas. At any given time, one member acts as the primary replica set member and the other members act as secondary replica set members. 

MongoDB is strongly consistent by default. Reads and writes are issued to a primary copy of the data. If the primary member fails for any reason (e.g., hardware failure, network partition), one of the secondary members is automatically elected to primary and begins to process all writes. Replica sets also provide operational flexibility by providing a way to upgrade hardware and software without requiring the database to go offline. This is an important feature, because these types of operations can account for as much as one third of all downtime in traditional systems.<br/>

We will learn both sharding and replication in more detail in the next lessons.<!--COMMENT; BRAD: need exercises here-->

## Advantages and disadvantages of MongoDB
Now we can generalize the good and bad sides of MongoDB usage and underline when it is better to use it.<br/> 

### Advantages
Below we isolate five main features that make MongoDB better among many other databases:
<ol>
	<li><strong>Schema-less.</strong> MongoDB is a documented database in which one collection may hold absolutely different documents. The number of fields, content, and size of the document can differ from one document to another. Fields can be added to an existing document at any time, dynamically. No ALTER TABLE queries (which usually take place in SQL databases), no rebuild indexing.</li>
	<li><strong>Performance.</strong> Suppose we want to collect data about blog posts; each blog post consists of the title, content, and comments. In the relational model, the comment will be stored as an individual table and retrieved by joining the post table and comment table. In the document model, they are saved as one document. They are treated as a whole. When querying, you get everything from that one document, no reference to other documents, that one document can be identified by an id. Thus, you get a document that is a key value query, not a relational query. A Key value query is much faster than a relational query. Joins are the slowest actions in SQL.</li>
	<li><strong>Scalability.</strong> With MongoDB, it is very easy to scale horizontally. MongoDB supports auto-sharding and auto failover. When the data on one node exceeds the threshold, MongoDB automatically rearranges the data to an evenly distributed node.</li>
	<li><strong>Load balancing and sharding.</strong> If you have huge amounts of data or want to distribute the traffic of your database among various machines to balance the load, MongoDB carries a number of advantages over traditional databases. Moreover, sharding, which is MongoDB’s unique approach to fulfilling the requirements of growth in data, makes use of horizontal scaling and allows you to use multiple machines for the purpose of supporting the growth of data.</li>
	<li><strong>Speed.</strong> As all the data is generally at a single location, MongoDB is extremely quick. However, this only stands true when the data you are working on is actually a document. If you are working on a data that emulates a relational model, your code will be required to carry out multiple independent queries for the purpose of retrieving the single document, and this will make it slower than an RDBMS.</li>
</ol>

### Disadvantages
Despite the relatively big list of advantages, there are some instances when MongoDB doesn't work as well as some other databases. But today it is believed that it's impossible to build an ideal structure for data storage. This belief is known as [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem).<br/>

So here is a list of disadvantages:
<ol>
	<li><strong>Usage of memory.</strong> As MongoDB stores the key name along with every document, it naturally consumes more memory. MongoDB using memory mapped files, lets the Operating System handle the caching. The size of your database is limited by virtual memory provided by Operating System and hardware. On the 32bit machine, you can only save 4 GB data. For the production environment, a 64bits is a must. If you are using it on you own 32bit machine, don't save serious data into MongoDB. If you do, when data exceeds the capacity, your insertions may fail without any warnings.</li>
	<li><strong>Not support join operation.</strong> Like a relational database, joins are simply not possible in MongoDB. This is also the trade off in order to be able to easily scale horizontally. But you can always perform your join by making multiple queries.</li>
	<li><strong>Not support transaction.</strong> To achieve its performance and scalability, MongoDB ditches the transaction support. This makes MongoDB very easy to scale horizontally which utilizes a lot of cheap hardware to balance the load. Scaling horizontally is generally a hard task in a relational database like MySQL. MongoDB is the best fit if you have a lot of data, but the relation between them is weak. For example, a stream of independent events. Actually the early application of MongoDB was to record online ads clicking statistical information. It’s not fit for strongly related data like your bank account information, whenever you transfer money, large amounts of related bank data needs to be modified at the same time, and either all is done, or nothing is done. RDBMS do this by the transaction. A transaction is the propagation of one or more changes to the database. For example, if you are creating a record or updating a record or deleting a record from the table, then you perform the transaction on the table.</li>
	<li><strong>Still under development.</strong> While SQL was developed in the 1980s, MongoDB entered the market in 2009. As a result, MongoDB is not that extensively documented or tested and also lacks the availability of support and experts. But probably this is not a disadvantage, but vice versa.</li>
</ol>

As you can see, there are both advantages and disadvantages to MongoDB, and you should switch to it only if the disadvantages don’t make much of a difference to you. Also, while traditional databases have been used for a long time, MongoDB is pretty new in the industry and does carry a lot of potential for further development. There is a major possibility that MongoDB might be the most advantageous of all the databases in future.

### When we should use MongoDB
At the end of this segment we want to make few recommendations about when and why we should (or even must) use MongoDB:
<ul>
	<li>If you need to load tons of data lines with a low business value for each one</li>
	<li>When you need high availability in an unreliable environment (cloud and real life);</li>
	<li>If you need to partition and shard your database</li>
	<li>when you want to get good database and server administration, content management, and delivery;</li>
	<li>when your data is location based because of MongoDB has built-in spatial functions;</li>
	<li>when your dataset is going to be big (starting from 1 GB) and the schema is not stable.<br/></li>
</ul>


In the following lessons, we will show you a lot of instructive and clear examples (including real-world) after learning of which you will be able to easily decide when and (it is more important) how to use MongoDB in your projects. But now let’s install MongoDB server and all necessary elements on your computer.<!--COMMENT; BRAD: We need exercises here-->

## Installation and setup
We are approaching by small steps the stage in which we'll start using MongoDB in practice. But first you need to install MongoDB on your computer.<br/>

The instructions for installation of MongoDB on various operating systems are provided on [the official site of MongoDB](https://docs.mongodb.com/manual/installation/). Below we provide more universal instructions for MongoDB installation depending on your operating system.
### Install on Linux and OS X
1. Go to the MongoDB download page <a href="https://www.mongodb.com/download-center">https://www.mongodb.com/download-center</a> and choose your system.
2. Download the archive and unzip it on your computer. Let's suppose you’ve downloaded it to /home/`<USER>`/Downloads folder, where `<USER>` is your user name.
3. Open terminal (press Ctrl+Alt+T on Linux or Control+Option+Shift+T on MacOS) and run the following command (it starts a bash shell as a root level user. You need it because typically normal users can’t access /home/ directory):<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ sudo bash`
4. We need to create a folder where MongoDB will save databases. For this we go up to the top of the file system <br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ cd /`
5. Then create the folder for database storage<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ mkdir -p /data/db`
6. Also, we need to add a permission to write data if we're not logged in as root user<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ chmod 777 /data`
7. Same as before<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ chmod 777 /data/db`
8. Go to the folder with just downloaded installation file. In our case it is /home/user_name/Downloads/mongodb-linux-x86_64-ubuntu1404-3.2.8/ and open /bin folder<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ cd /home/<USER>/Downloads/mongodb-linux-x86_64-ubuntu1404-3.2.8/bin`
9. Copy all files into /usr/local/bin<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ cp * /usr/local/bin`<br/>
After running of all above steps you will have installed MongoDB on your Linux or MacOS.<br/>
Let’s check whether all works fine. In terminal window (open new one if you’ve already closed it) write:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ mongod`<br/>
You should see the following text in the terminal:<br/>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/08/1470905675428.png"></center><br/>
To stop MongoDB, press Control+C in the terminal where the mongod instance is running.

### Install on Windows
1. Go to the download page <a href="https://www.mongodb.com/download-center">https://www.mongodb.com/download-center</a>, choose your system.
2. Download the msi file and install it.
3. Go to Control Panel\System and Security\System → Advanced system settings → Advanced → Environment Variables
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/08/1470393375203.png"></center>
4. In System variables find “Path,” press “Edit,” and add the path of installed MongoDB. Suppose you’ve installed MongoDB to C:\Program Files, then you should paste<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `;C:\Program Files\MongoDB\Server\3.2\bin`<br/>
to the “Path” variable (yes, semicolon should be there).
5. Run console window (for example, press Win+R, type cmd and press Enter).
6. MongoDB requires a data directory to store all data. MongoDB’s default data directory path is \data\db. Create this folder using the following commands from a Command Prompt<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `> md \data`<br/>
and<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `> md \data\db`<br/>
Thus, you should have MongoDB installed.<br/>
To start MongoDB, run mongod.exe<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `> mongod.exe`<br/>
<center><img src="https://study.moderndeveloper.com/wp-content/uploads/2016/08/1470728724866.png"></center> <br/>
To stop MongoDB, press Control+C in the Command Prompt.<!--COMMENT; BRAD: need exercises here-->

## Mongo shell
Now you know what MongoDB is, what it can be used for, and how to install it on your computer. In this section we describe how to run the simplest commands to get access to MongoDB databases.<br/>

MongoDB provides an interactive interface for querying and updating data as well as performing administrative operations. It called **mongo shell.** The mongo shell is a component of the MongoDB distributions. Once you have installed and have started MongoDB, connect the mongo shell to your running MongoDB instance.<br/>

To run mongo shell open terminal or command line depending on your OS and type **mongo** to start mongo (see paragraph “Installation and setup“). It is a powerful interactive JavaScript shell interface, enabling system administration operations and easy testing of queries directly with database. When you invoke it, it connects to the MongoDB server instance, which runs on the localhost, listening on the port 27017. Then, the shell keeps track of the current database, you can check it with **db**. If you'd want to use another database (even if it still does not exist - in that case mongo will create it), do it with **use &#60;database&#62;**. After that, reference it with **db** like this: **db.myCollection.insert({my_doc: 01})**<br/>

To display the database you are currently using, write **db** in the terminal or command line. The operation should return **test**, which is the default database. To see all available databases created anywhere, type **show dbs** in the terminal or command line.<br/>

We will not show any other commands in this lesson because this is the material for the next lessons. The aim of the current lesson is to show where databases can be written. <!--COMMENT; BRAD: need exercises here-->
<div class="exercises-div">
<h4>Exercise 1</h4>
<p><code>use &lt;database_name&gt;</code> is a simple command for creation of a new database using mongo-shell. Create an empty databases "test1" and "empty_db" and display all databases in console.</p>
</div>

## Mongo API for Node.js
Besides mongo shell, MongoDB API supports many programming languages such as: Python, Java, C++, C#, JavaScript, etc. The full list of languages can be seen here <a href="http://api.mongodb.com">http://api.mongodb.com</a>. API (**A**pplication **P**rogramming **I**nterface) is a set of routine definitions, protocols, and tools for building software and applications. A good API makes it easier to develop a program by providing all the building blocks, which are then put together by the programmer. MongoDB provides an easy to use and very large set of commands for working with a database and its data.<br/>

In this course we suppose you have a good knowledge of JavaScript, that’s why we will use MongDB API for JavaScript. To be more precise, it is the API for Node.js - a very powerful JavaScript-based framework/platform built on Google Chrome's JavaScript V8 Engine. Node.js uses an event-driven, non-blocking I/O model that makes it lightweight and efficient. Node.js' package ecosystem, npm, is the largest ecosystem of open source libraries in the world.<br/>

Let's install Node.js. Below we provide the short instruction for installation of Node.js on Ubuntu, MacOS and Windows.
If you have different OS or you want to see another method of installation or have some trouble with installation please visit <a href="https://nodejs.org/en/download/package-manager/">https://nodejs.org/en/download/package-manager/</a>

### Install Node.js on Ubuntu
1. Type the following command in your terminal:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ sudo apt-get install nodejs`  <br/>
2. Then install the Node.js package manager, npm:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ sudo apt-get install npm`  <br/>
3. Create a symbolic link for node, as many Node.js tools use this name to execute.<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ sudo ln -s /usr/bin/nodejs /usr/bin/node` <br/>
4. Now we should have both the Node and npm commands working. Check this displaying the version of Node.js<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ node -v` <br/>
and npm<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ npm -v` <br/>

### Install Node.js on OS X and Windows
1. Download the installer from <a href="http://nodejs.org/en/download">http://nodejs.org/en/download</a>.
2. Run the installer Node.js and npm (the .msi or .pkg file you downloaded in the previous step) and follow the prompts in the installer.
3. Restart your computer. You won’t be able to run Node.js until you restart your computer.
4. Display the installed version of Node.js<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `> node - v`<br/>
and npm<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `> npm - v`<br/>

Now, when Node.js is installed we need to install the MongoDB Node.js driver:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`$ npm install mongodb` <br/>

Now we're ready to test our system. Run the mongodb if you turned it off:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ mongodb` <br/>
and connect to the MongoDB server through its API for Node.js. Create the file “test.js and post there the following code
<pre lang="javascript" line="1">    var MongoClient = require('mongodb').MongoClient;
    var assert = require('assert');
    var url = 'mongodb://localhost:27017/test';
    MongoClient.connect(url, function(err, db) {
         assert.equal(null, err);
         console.log("Connected correctly to server.");
         db.close();
    });</pre>
This code imports “mongodb” and “assert” modules. The second one is for displaying exceptions and error messages if you cannot connect to the MongoDB server. ***MongoClient*** creates a separate session for interaction with the MongoDB server. We define  ***url = 'mongodb://localhost:27017/test'*** to set connection to a MongoDB instance that runs on the localhost interface on port 27017 and switch to the “test” database. The ***connect*** method of MongoClient connects to MongoDB server with a specific URL. All we do in this session is to display message that we were able to get access to “test” database. Note, that after all actions with a database we must close it to save all changes ***db.close().***<br/>
If you create the file just type in terminal:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ node test.js` <br/>
&nbsp; If you did everything right you will see "Connected correctly to server" message in the terminal or command line.<br/>

MongoDB APIs for Node.js possess the full correspondence for native mongo shell syntax. There is a specific JavaScript library for mongoose that provides a straightforward, schema-based solution to model your application data. It includes built-in type casting, validation, query building, business logic hooks, and more, out of the box. We aren’t going to use it in the course, but mention it only to introduce you to alternatives for pure API for Node.js. To install **mongoose** you may use npm as it was done for mongodb library:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; `$ npm install mongoose` <br/><!--COMMENT; BRAD: need exercises here-->
<div class="exercises-div">
<h4>Exercise 2</h4>
<p>Mongo API for Node.js also allows to display the list of all databases present on your computer similarly to the <code>show dbs</code> mongo-shell command. To get access to databases we may use Admin() class that helps to access the admin functionality of MongoDB
<pre lang="javascript" line="5">
// Connect to any MongoDB database
MongoClient.connect('mongodb://localhost:27017/test2', function(err, db) {
 // Use the admin database for the operation
 var adminDb = db.admin();
 // List all the available databases
 adminDb.listDatabases(function(err, result) {
   // Fetch all databases
   var dbs = result.databases;
   db.close();
 });
});
</pre>
Display the list of all databases and count them. Compare this list with the one that was retrieved using `show dbs` command.</p>
</div>

## Conclusions
The problem of storing and fast work with data in the modern world is one of our most important problems. The time when the information storage on books was enough are far behind. They were changed by databases and today the struggle between SQL and NoSQL databases is waging - which one is better and when?<br/>

In this lesson, we met you with both kinds of databases and their subtypes, showed when they can be used and how they store data. But our attention was mainly focused on one of the most promising databases, MongoDB. You’ve seen the difference in how it collects data as opposed to, particularly, SQL databases; how data is written to documents, documents are joined into collections and collections are combined into a database. We’ve laid a good basis for our further work and in the next lesson we are going to show how to create new databases, collections, and documents, fill them with data and apply a simple transformation to these data. Lessons will become only more interesting!


<div class="questions-div">
<h4>Questions</h4>
<ol>
<li>Why the problem of data storage is so urgent today?</li>
<li>Which databases came first? What were their properties and appointment?</li>
<li>What led to the emergence of NoSQL databases?</li>
<li>How the data is stored in SQL databases? What are the main structural units of SQL tables?</li>
<li>What do SQL database allow doing with data?</li>
<li>Which management systems for relational databases do you know?</li>
<li>What is Primary Key?</li>
<li>How the data is stored in NoSQL databases?</li>
<li>Which types of NoSQL databases do you know?</li>
<li>Describe the main differences between SQL and NoSQL databases.</li>
<li>What is MongoDB? Open the main features of MongoDB</li>
<li>How are data stored in MongodB? What is BSON?</li>
<li>Where MongoDB is used? Which its properties did play the major role that MongoDB "won" all competitor these areas?</li>
<li>Describe the architecture of MongoDB.</li>
<li>What are sharding and replication? How do they work in MongoDB?</li>
<li>Count advantages of MongoDB.</li>
<li>Count disadvantages of MongoDB.</li>
<li>When should we use MongoDB and why?</li>
<li>How to run mongo shell? What is mongod?</li>
<li>How to connect to MongoDB server using API for Node.js?</li>
<ol>
</div>
