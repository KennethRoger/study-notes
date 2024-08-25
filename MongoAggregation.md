# MONGO DB #

## JSON ##

JSON (JavaScript Object Notation) is a lightweight data-interchange format that is easy for humans to read and write, and easy for machines to parse and generate. It is primarily used to transmit data between a server and a web application or within applications for structured data exchange.
JSON (JavaScript Object Notation) is a key format used to represent documents. MongoDB, a NoSQL database, stores data in a flexible, JSON-like format called BSON (Binary JSON). This format allows for the representation of complex nested structures and supports more data types than standard JSON.

### Data types of JSON ###

1. String
2. Number
3. Object
4. Array
5. Boolean
6. Null
7. Undefined

## Extended JSON ##

Extended JSON is a MongoDB-specific format that provides a way to represent BSON data types using a standard JSON format. This is necessary because standard JSON does not support all BSON types.

### Structure: ###

 Extended JSON adds special notation to represent BSON types that JSON does not natively support, such as Date, ObjectId, Binary, Decimal128, Long, etc.
It comes in two modes: ** Relaxed Mode ** and ** Strict Mode **.

Relaxed Mode: is more human-readable, with values often represented in their natural forms (e.g., dates as strings).
Strict Mode: uses explicit representations to capture all the nuances of BSON data types (e.g., dates with $date).

## Usage in MongoDB: ##

Extended JSON is mainly used in contexts where MongoDB needs to serialize BSON data into JSON, such as in MongoDB's shell output, export/import tools, and drivers.


## BSON (Binary JSON) ##
 BSON is a binary-encoded serialization format used to store documents and make remote procedure calls in MongoDB. It is designed to be lightweight, traversable, and efficient in both size and speed.
BSON extends JSON with additional data types (e.g., Date, ObjectId, Binary, Decimal128) and is binary-encoded, making it more efficient for storage and transmission.
BSON supports integer types of different sizes (e.g., int32, int64), which is more efficient for certain numeric operations.

### Usage in MongoDB: ###

Internally, MongoDB stores data in BSON format. This allows MongoDB to support additional data types that are not natively supported in standard JSON.
BSON is optimized for speed and space; thus, it is more efficient for MongoDB's internal data operations.

### Most common BSON types ###

1. String - "String sample"
2. Object - {"type": "Phone", "price":120}
3. Boolean - true
4. Array - [10, 5, 15]
5. Double - 10.5
6. 32-bit Integer - NumberInt(150)
7. ObjectId - ObjectId("594e773099cd0001699a4d8bd")
8. Date - ISODate("2024-07-15T15:33:33.732Z")

## MongoDB indexes ##

In MongoDB, ** indexes ** are special data structures that store a small portion of the data set in an easy-to-traverse form. Indexes are crucial for improving the performance of search queries and efficiently retrieving data. Without indexes, MongoDB must perform a collection scan, i.e., scan every document in a collection to find those that match the query criteria, which can be very slow, especially with large collections.
Indexes are created on the fields that are most often used like postId from earlier example
Indexes takes up more memory and slows insert, update and remove operations

To create index for a field:
```
db.<collection>.createIndex({field: 1}) // 1 for ascending;
```

To get index:
```
db.<collection>getIndex();
```

To remove/drop an index:
```
db.<collection>.dropIndex('indexName);
```
 ## MongoDB utilities ##

MongoDB utilities are command-line tools provided by MongoDB to perform various administrative and maintenance tasks. These utilities help manage databases, perform backups and restores, import and export data, monitor performance, and interact with MongoDB data and server configurations. Below are some of the most commonly used MongoDB utilities:

1. mongodump
Purpose: Creates a binary backup (dump) of a MongoDB database or collection.
Usage: Used for backing up data from MongoDB for disaster recovery or migration.

Example: mongodump --db mydatabase --out /backup/mydatabase

2. mongorestore
Purpose: Restores data from a binary backup created by mongodump.
Usage: Used to restore data from a backup into a MongoDB database.

Example: mongorestore --db mydatabase /backup/mydatabase

3. mongoexport
Purpose: Exports data from a MongoDB collection to a JSON, CSV, or TSV file.
Usage: Useful for data export for analysis or data transfer between environments.

Example: mongoexport --db mydatabase --collection mycollection --out mydata.json

4. mongoimport
Purpose: Imports data from a JSON, CSV, or TSV file into a MongoDB collection.
Usage: Used to import data into MongoDB from external sources or other database systems.

Example: mongoimport --db mydatabase --collection mycollection --file mydata.json

5. mongostat
Purpose: Provides a quick overview of the status of a MongoDB instance, showing statistics like queries per second, inserts per second, memory usage, etc.
Usage: Useful for monitoring the health and performance of MongoDB instances.

Example: mongostat --host localhost

6. mongotop
Purpose: Displays the amount of time a MongoDB instance spends reading and writing data.
Usage: Helps in identifying performance bottlenecks by showing read and write activity on collections.

Example: mongotop 5

7. mongo
Purpose: The MongoDB shell (mongosh) used to interact with MongoDB instances from the command line.
Usage: Useful for performing database operations, scripting, and running administrative commands.

Example: mongo or mongosh (MongoDB Shell)

8. bsondump
Purpose: Converts BSON data files (used in MongoDB) to a human-readable format, usually JSON.
Usage: Used to view or analyze BSON files generated by mongodump.

Example: bsondump mycollection.bson

9. mongofiles
Purpose: Allows interaction with GridFS, a specification for storing and retrieving large files in MongoDB.
Usage: Used for uploading and downloading files stored in GridFS.

Example: mongofiles --db mydatabase list

10. mongosh (MongoDB Shell)
Purpose: An interactive JavaScript shell for MongoDB. It is used to query and update data as well as to perform administrative tasks.
Usage: The primary tool for interacting with MongoDB from the command line.

Example: mongosh to start the shell.

11. mongoreplay
Purpose: Captures and replays traffic going to a MongoDB server.
Usage: Useful for testing and development environments to replay the production traffic and see how it behaves.

Example: mongoreplay record --uri mongodb://localhost:27017

12. mongofresh
Purpose: Helps in setting up fresh test environments by copying data from an existing database and cleaning it.
Usage: Typically used in development and testing scenarios.

Example: This is a relatively less commonly used utility, so specific usage would depend on the need.

## MongoDB replica sets ##

MongoDB Replica Sets are a mechanism for ensuring high availability and data redundancy(presence of multiple data copies) in a MongoDB deployment. A replica set is a group of MongoDB servers (nodes) that maintain the same data set

MongoDB replica sets provide redundancy by ensuring that all members have copies of the data, preventing data loss if a server fails. They feature automatic failover, where a new primary is automatically elected if the current primary fails. Data consistency is maintained through the oplog, which replicates changes from the primary to secondary nodes. Additionally, read preferences can be configured to distribute read operations across secondary nodes, balancing the read load.

### Components: ###
Primary Node: The primary node handles all write operations and reads (by default). It is the only node that accepts writes and updates.

Secondary Nodes: Secondary nodes replicate data from the primary node. They can be used for read operations (if configured) and provide redundancy.

Arbiter Node: An arbiter is a special type of node that does not hold data but participates in elections to help maintain a majority for primary elections.

## MongoDB drivers ##

MongoDB drivers are libraries that enable applications to interact with MongoDB databases. They provide an interface for connecting, querying, and managing data. Native MongoDB drivers are specific to programming languages like JavaScript, Python, Java, and more, ensuring optimized performance and full feature support. They handle the communication between your application and the MongoDB server, leveraging the MongoDB Wire Protocol to execute commands and process results efficiently. Each driver is designed to integrate seamlessly with its respective language, offering language-specific abstractions and optimizations.

## Aggregation framework ##

The ** aggregation framework ** in MongoDB is a powerful tool used to perform data processing and transformation operations on documents within a collection. Using aggregation framework you can find the subsets of documents by which you can create new documents.

### The aggregation framework is a set of MongoDB operations that allows you to: ###

1. Filter documents (like SQL's WHERE clause).
2. Group documents by some field(s) and perform operations like counting, summing, averaging, etc. (similar to SQL's GROUP BY clause).
3. Project fields to reshape documents (similar to SQL's SELECT clause).
4. Sort documents (like SQL's ORDER BY clause).
5. Join documents from different collections (using the $lookup operator).
6. Unwind arrays to output a document for each element (similar to SQL's UNNEST).

### Key Components of the Aggregation Framework ###
Pipeline: A sequence of stages, each of which performs an operation on the data and passes the result to the next stage.
Stages: Each stage in the pipeline represents an operation, like $match, $group, $project, $sort, $lookup, etc.
Operators: Specific keywords used within stages to perform computations or transformations (e.g., $sum, $avg, $push).

** aggregation syntax **

```
db.<collection>.aggregate([
  {$operator:{}}]}
```
eg: 
```
cmd: blog> db.posts.find()

out:
[
  {
    _id: ObjectId('66c97dbe8fe15265585e739c'),
    title: 'What is the best way to learn JavaScript from the ground up?',
    postId: 3511,
    comments: 10,
    shared: true,
    tags: [ 'JavaScript', 'programming' ],
    author: { name: 'Mike Forester', nickname: 'mikef' }
  },
  {
    _id: ObjectId('66c97dbe8fe15265585e739d'),
    title: 'My thoughts about 12-factor App Methodology',
    postId: 2618,
    comments: 0,
    shared: false,
    tags: [],
    author: { name: 'Emily Watson', nickname: 'emily23' }
  },
  {
    _id: ObjectId('66c97dbe8fe15265585e739e'),
    title: 'Who can suggest best computer coding book for beginners?',
    postId: 8451,
    comments: 2,
    shared: false,
    tags: [ 'programming', 'coding' ],
    author: { name: 'Emily Watson', nickname: 'emily23' }
  },
  {
    _id: ObjectId('66c97dbe8fe15265585e739f'),
    title: 'I want to start my own business. What I need to do first?',
    postId: 3015,
    comments: 25,
    shared: true,
    tags: [ 'business', 'money' ],
    author: { name: 'Bob Hutchinson', nickname: 'bob1995' }
  },
  {
    _id: ObjectId('66c97dbe8fe15265585e73a0'),
    title: 'What is the average salary of the junior frontend developer?',
    postId: 1151,
    comments: 0,
    shared: false,
    author: { name: 'Mike Forester', nickname: 'mikef' }
  }
]

cmd: blog> db.posts.aggregate([
... {$group: {_id: "author.name"}}
... ])

out: [ { _id: 'author.name' } ]

cmd: blog> db.posts.aggregate([ { $group: { _id: "$author.name" } }] )

out:
[
  { _id: 'Mike Forester' },
  { _id: 'Emily Watson' },
  { _id: 'Bob Hutchinson' }
]

cmd: blog> db.posts.aggregate([ { $group: { _id: "$author.nickname" } }] )

out: [ { _id: 'emily23' }, { _id: 'bob1995' }, { _id: 'mikef' } ]
```

##3 cursor ###

Cursor is a MongoDB object used to iterate over a set of results returned by a query or aggregation. Instead of returning all results at once, MongoDB uses cursors to optimize the retrieval of large datasets, handling data in manageable batches.

## How aggregation framework works? ##

### Syntax: ##3
db.<collection>.aggregate([
  <stage1>,
  <stage2>,
  ...
  <stageN>
])

Aggregation framework uses a special method called `aggregate`. This method needs one argument which is an array array of stages. Each stage is an object and all stages are separated by comma.
At the beginning all documents in the collection are passed to the stage one then those documents are processed in this stage and result of processing is passed to stage two. 
Stage two takes those documents from the stage one or from each operation and then passes to the next stage and so on. 
when last stage and its operation results are returned back to the client and those
results are returned as a ** cursor **.

## Aggregation Stage ##

Each stage takes documents and returns some document based on the stage operator. Stages dont depend on each other, only the result of a stage are passed to another stage. ie, Stages are independent.



### Types of stage operator ###

`{ $<stageOperator> : {} }`


In MongoDB, aggregation pipelines use various stage operators to process data. Each stage in the pipeline performs a specific operation on the data and passes the result to the next stage. Here’s a list of some of the most commonly used types of stage operators:

1. ### $match ###

Filters documents to pass only those that match the specified condition(s) to the next stage.
``` { $match: { <query> } } ```

Example: { $match: { status: "A" } }

2. ### $project ###

Reshapes each document in the stream, such as by including or excluding specific fields, adding new fields, or computing values.

Example: { $project: { title: 1, author: 1, _id: 0 } }

3. ### $group ###

Groups documents by a specified identifier expression and applies an accumulator function (e.g., sum, avg) to each group.
``` { $group: { _id: <expressio>, <field>: { <accumulator> : <expression> }, ... }} ```

Example: { $group: { _id: "$cust_id", totalAmount: { $sum: "$amount" } } }

4. ### $sort ###

Sorts all input documents and outputs them in sorted order.

Example: { $sort: { age: -1 } } (sort by age in descending order)

5. ### $limit ###

Limits the number of documents passed to the next stage.

Example: { $limit: 5 }

6. ### $skip ###

Skips a specified number of documents before passing the remaining documents to the next stage.

Example: { $skip: 10 }

7. ### $unwind ###

Deconstructs an array field from the input documents to output a document for each element in the array.

Example: { $unwind: "$tags" }

8. ### $lookup ###

Performs a left outer join to another collection in the same database to filter in documents from the "joined" collection for processing.

Example:
```
{
  $lookup: {
    from: "orders",
    localField: "cust_id",
    foreignField: "cust_id",
    as: "orderdetails"
  }
}
```

9. ### $out ###

Writes the result of the aggregation pipeline to a specified collection. Replaces the output collection completely if it exists.

Example: { $out: "filteredResults" }

10. ### $count ###

Counts the number of documents that are passed to it and outputs the count as a document.

Example: { $count: "num_docs" }

11. ### $facet ###

Allows for multiple concurrent aggregation pipelines within a single stage to process the same input documents in different ways.

Example:
```
{
  $facet: {
    "priceBucket": [
      { $match: { price: { $exists: true } } },
      { $bucket: { groupBy: "$price", boundaries: [0, 100, 200, 300], default: "Other" } }
    ],
    "yearlySales": [
      { $match: { status: "A" } },
      { $group: { _id: { $year: "$date" }, total: { $sum: "$amount" } } }
    ]
  }
}
```

12. ### $merge ###

Merges the resulting documents into a specified collection, modifying the existing documents in the target collection if they match the conditions or inserting new documents if they do not.

Example:
```
{
  $merge: {
    into: "outputCollection",
    whenMatched: "merge",
    whenNotMatched: "insert"
  }
}
```

13. ### $addFields ###

Adds new fields to documents. Similar to $project, but does not reshape the documents; it only adds new fields.

Example: { $addFields: { total: { $sum: ["$price", "$tax"] } } }

14. ### $replaceRoot ###

Replaces the input document with the specified embedded document. Useful to promote a field to the top level.

Example: { $replaceRoot: { newRoot: "$address" } }

15. ### $bucket ###

Categorizes incoming documents into groups, called buckets, based on a specified expression, and outputs a document per each bucket.
Example:
json
Copy code
{
  $bucket: {
    groupBy: "$price",
    boundaries: [0, 100, 200, 300],
    default: "Other"
  }
}

16. ### $bucketAuto ###

Automatically categorizes documents into a specified number of buckets based on a specified expression.
Example:
```
{
  $bucketAuto: {
    groupBy: "$price",
    buckets: 4
  }
}
```
17. ### $sample ###

Randomly selects a specified number of documents from its input.

Example: { $sample: { size: 5 } }

## Aggregation expression ##

Expression refers to the name of the field in input documents
"$<fieldName>"

Eg: 
{ $group: { _id: "$age"} }
{ $group: { _id: "$company.location.country"} }
{ $group: { _id: "$name", total" { $sum: "$price" }}}

## Different query operators ##

MongoDB provides a rich set of query operators that are used to filter documents during queries. These operators allow you to perform various operations such as comparison, logical operations, and element-based filtering, among others. Here’s a breakdown of the different types of query operators in MongoDB:

1. ### Comparison Operators ###

These operators are used to compare field values in documents.

$eq: Matches values that are equal to a specified value.
$ne: Matches all values that are not equal to a specified value.
$gt: Matches values that are greater than a specified value.
$gte: Matches values that are greater than or equal to a specified value.
$lt: Matches values that are less than a specified value.
$lte: Matches values that are less than or equal to a specified value.
$in: Matches any of the values specified in an array.
$nin: Matches none of the values specified in an array.

2. ### Logical Operators ###

These operators are used to combine multiple query conditions.

$and: Joins query clauses with a logical AND, returns all documents that match the conditions of both clauses.
$or: Joins query clauses with a logical OR, returns all documents that match the conditions of either clause.
$not: Inverts the effect of a query expression, returns documents that do not match the query expression.
$nor: Joins query clauses with a logical NOR, returns all documents that fail to match both clauses.

3. ### Element Operators ###

These operators are used to match fields that have specific types or conditions.

$exists: Matches documents that have the specified field.
$type: Matches documents that have a field of a specified type.

4. ### Evaluation Operators ###

These operators are used to evaluate expressions or apply conditions.

$expr: Allows the use of aggregation expressions within the query language.
$jsonSchema: Validates documents against the given JSON Schema.
$mod: Performs a modulo operation on the value of a field and matches documents with a specified result.
$regex: Provides regular expression capabilities for pattern matching strings in queries.
$text: Performs text search.
$where: Matches documents that satisfy a JavaScript expression.

5. ### Array Operators ###

These operators are used to perform operations on arrays.

$all: Matches arrays that contain all elements specified in the query.
$elemMatch: Matches documents that contain an array field with at least one element that matches all the specified query criteria.
$size: Matches any array with the specified number of elements.

6. ### Bitwise Operators ###

These operators are used to perform bitwise operations on field values.

$bitsAllClear: Matches numeric or binary data in which a set of bit positions all have a value of 0.
$bitsAllSet: Matches numeric or binary data in which a set of bit positions all have a value of 1.
$bitsAnyClear: Matches numeric or binary data in which any bit from a set of bit positions has a value of 0.
$bitsAnySet: Matches numeric or binary data in which any bit from a set of bit positions has a value of 1.

7. ### Geospatial Query Operators ###

These operators are used to perform geospatial queries.

$geoIntersects: Selects geometries that intersect with a specified GeoJSON object.
$geoWithin: Selects geometries that are within a specified GeoJSON object.
$near: Returns geospatial objects in proximity to a point.
$nearSphere: Returns geospatial objects in proximity to a point on a sphere.

8. ### Projection Operators ###

These operators are used in projections to modify the output of a query.

$: Projects the first element in an array that matches the query condition.
$elemMatch: Projects the first matching element from an array based on specified criteria.
$meta: Projects metadata (e.g., text score) associated with each document.
$slice: Limits the number of elements projected from an array.


