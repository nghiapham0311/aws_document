#### Relational (SQL) vs Non-Relational (NoSQL)
- Structured Query Language (**SQL**)
- Structure **in** & **between** tables of data - Rigid **Schema**
- …Relationships between tables
- NoSQL - **Not one single thing … different models**
- Generally a much more relaxed Schema
- Relationships handled differently
##### Relational Data Example
![[Database Refresher & Models.png]]

##### NoSQL
###### Key-Value

![[Database Refresher & Models-1.png]]
In general, key value databases adjust really fast, it’s simple data with no structure, there isn’t much that gets in the way, between giving the data to the database and it being written to disk.

**Use cases: Simple requirements, or mention data which is just name and values, or pairs or keys and values, also used for in-memory caching (EXAM)**
###### Wide Column Store
![[Database Refresher & Models-2.png]]
Each *row* or item has *one or more keys*, generally *one of them* is called the *partition key*. And then optionally you can have additional keys as well as the partition key.

The 2nd key is called the sort or the range key. It differs depending on the database but most examples of wide column stores generally have one key as a minimum which is the partition key, and then optionally every single row or item in that database can have additional keys

Every *item in a table* has to have the *same key layout*. So that’s one key or more keys, and *they just need to be unique to that table*. Wide column stores offer groupings of items called tables, but they’re still not the same type of tables as in relational database products, they’re **just groupings of data**.

Every *item in a table* can also have *attributes*, but they *don’t have to be the same* between items. Every item can have any attribute. It could have all of the attributes so all of the same attributes between all of the items, it could have a mixture, so mix and matching attributes on different items, or an item could even have no attributes.

There is **no schema, no fixed structure on the attribute side**, it’s normally partially opaque for most database operations.

The only thing that matters in a wide column store, is that **every item inside a table has to use the same key structure and it has to have a unique key**. So whether that’s a single partition key or whether it’s a composite key, so a partition key and something else, if it’s a single key, it has to be unique, if it’s a composite key, the combination of both of those values has to be unique. That’s the only rule for placing data into a table using wide column stores.
###### Document
![[Database Refresher & Models-3.png]]

Documents are generally formatted using a structure such as json or XML, but often the structure can be different between documents in the same database. You can think of a document database almost like an extension of a key value store where each document is interacted with via an ID that’s unique to that document. But the value, the document contents are exposed to the database, allowing you to interact with it.

Document databases work best for scenarios like order databases or collections or contact style databases, situations where you generally interact with the data as a document.

Document databases are also *great* when you **need to interact with deep attributes, so nested data items within a document structure**. The document model works well with use cases such as catalogs, user profiles, and lots of different content management systems where each document is unique but it changes over time. So it might have different versions, documents might be linked together in hierarchical structures, or when you’re linking different pieces of content in a content management system.
###### Column
![[Database Refresher & Models-4.png]]

Row based databases are where you interact with data based on rows.

For every order we have a row and those rows are stored on disk together. If you needed to read the price of one order from the database, you read the whole row from disk. If you don’t have indexes or shortcuts, you’ll have to find that row first, and that could mean scanning through rows and rows of data before you reach the one that you want to query.

If you want to do a query which operates over lots of rows, for example you wanted to query all of the sizes of every order, then you need to go through all of the rows, finding the size of each.

*Row based databases* are ideal **when you operate on rows, creating a row, or updating a row or deleting rows**. Row based databases are often called OLTP or online transaction processing databases

Use cases: for systems which are performing transactions, so audit databases, contact databases, stock databases, things which deal in rows and items where these rows and items are constantly accessed, modified and removed.

Column-based databases handle things very differently. Instead of storing data in rows on disk, they store it based on columns. The data is the same, but it’s grouped together on disk based on column. So every order value is stored together, every product item, every color, etc all grouped by the column that the data is in.

This means 2 things.

- First, it makes it very, very inefficient for transaction style processing, which is generally operating on whole rows at a time. But this very same aspect, makes column databases really good for reporting.
- if your query is related to just one particular column because that whole column is stored on disk grouped together, then that’s really efficient. You could perform a query to retrieve all products sold during a period, or perform query which looks for all sizes sold in total ever, and looks to build up some intelligence around which are sold most and which are sold least.

**Generally, what you’ll do is take the data from an OLTP database, a row-based database, and you’ll shift that into a column based database when you’re wanting to perform reporting or analytics.**

**Use cases: suited to reporting and analytics**
###### Graph
![[Database Refresher & Models-5.png]]


With graph databases, relationships between things are formally defined and stored in the database itself along with the data. They’re not calculated each and every time you run a query.

This makes them great for relationship driven data, for example, social media or HR systems.

With graph databases there are also relationships between the nodes which are known as edges. These edges have name and a direction. Relationships themselves can also have attached data, so name, value pairs.

A graph database can store a massive amount of complex relationships between data or between nodes, inside a database. And that’s what’s key. These relationships are actually stored inside the database, as well as the data.

A query to pull up details on all employees of XYZ Corp, would run much quicker than on a standard SQL database because that data of those relationships is just being pulled out of the database just like the actual data.

With relational style database, you’d have to retrieve the data and the relationships between the tables is computed when you execute the query. So it’s really inefficient process with relational database systems, these relationships are fixed and computed each and every time a query is run.

With *graph based* database, those **relationships are fluid, dynamic**, they’re stored in the database along with the data and it means when you’re interacting with data and looking to take advantage of these fluid relationships, it’s much more efficient to use a graph style database.

social media, complex relationship -> graph database first (**EXAM**)

[[ACID & BASE]]