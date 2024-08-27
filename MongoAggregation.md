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

## Array Update Operators in MongoDB ##

### $push: ### Adds an element to an array. If the field is not an array, it creates the field as an array with the value as its element.

Example: Add a single value to an array or use modifiers like $each, $slice, $sort, and $position to customize the behavior.
```
db.collection.updateOne(
    { _id: 1 },
    { $push: { scores: 89 } }
)
```

### $pull: ### Removes all instances of a value or values that match a specified condition from an existing array.

Example: Remove an element from an array.
```
db.collection.updateOne(
    { _id: 1 },
    { $pull: { scores: 89 } }
)
```
### $addToSet: ### Adds a value to an array only if the value is not already in the array. This operator ensures no duplicate values.

Example: Add a unique element to an array.
```
db.collection.updateOne(
    { _id: 1 },
    { $addToSet: { tags: "mongodb" } }
)
```
### $pop: ### Removes the first or last element of an array.

Example: Remove the last element from an array.
```
db.collection.updateOne(
    { _id: 1 },
    { $pop: { scores: 1 } }  // Use -1 to remove the first element
)
```
### $pullAll: ### Removes all matching values from an array. It differs from $pull in that it removes all instances of the specified values from the array.

Example: Remove all specified elements from an array.
```
db.collection.updateOne(
    { _id: 1 },
    { $pullAll: { scores: [89, 92] } }
)
```

### $push with Modifiers: ###
These modifiers are used in combination with $push to provide additional functionality.

* $each: Allows you to push multiple values onto an array.
* $slice: Limits the number of array elements.
* $sort: Sorts array elements.
* $position: Specifies the position in the array to add elements.


## Common Update Array Modifiers in MongoDB ##

### $each: ###

Usage: Used with $push and $addToSet to add multiple elements to an array.
Example: Add multiple projects to a project array field:
```
db.empdb9.updateOne(
  { ename: "Rahul" },
  { $push: { project: { $each: ["p2", "p3"] } } }
)
```

### $slice: ###

Usage: Used with $push to limit the size of an array. It allows you to keep only the last N elements in the array after an update.
Example: Keep only the last 3 elements after pushing a new element:
```
db.empdb9.updateOne(
  { ename: "Rahul" },
  { $push: { project: { $each: ["p4"], $slice: -3 } } }
)
```
Note: $slice: -3 means keeping the last 3 elements.

### $sort: ###

Usage: Used with $push to sort array elements in ascending or descending order when adding new elements.
Example: Sort the project array in ascending order while adding new projects:
```
db.empdb9.updateOne(
  { ename: "Rahul" },
  { $push: { project: { $each: ["p1"], $sort: 1 } } }
)
```
Note: $sort: 1 sorts in ascending order, and $sort: -1 sorts in descending order.

## $position: ##

Usage: Used with $push to specify the position in the array where new elements should be added.
Example: Add a new project at the beginning of the project array:
```
db.empdb9.updateOne(
  { ename: "Rahul" },
  { $push: { project: { $each: ["p0"], $position: 0 } } }
)
```
Note: $position: 0 places the element at the start of the array.

## cursor ##

Cursor is a MongoDB object used to iterate over a set of results returned by a query or aggregation. Instead of returning all results at once, MongoDB uses cursors to optimize the retrieval of large datasets, handling data in manageable batches.

## How aggregation framework works? ##

### Syntax: ###
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

`{ $project: { <filed1>: <1>, <field2>: <0>, <newField1>: <expression> ... } }`
Example: { $project: { title: 1, author: 1, _id: 0 } }

3. ### $group ###

Groups documents by a specified identifier expression and applies an accumulator function (e.g., sum, avg) to each group.
``` { $group: { _id: <expressio>, <field>: { <accumulator> : <expression> }, ... }} ```

   ** Accumulator operators **
   
   `{ $<accumulatorOperator>: <expression> }`
   
   some of the commonly used accumulator operators:
   i. ** $sum **: Calculates the sum of numeric values.
   
   Example: Calculate the total sales for each product.
   ```
   db.sales.aggregate([
     { $group: { _id: "$product", totalSales: { $sum: "$amount" } } }
   ])
   ```
   
   ii. ** $avg **: Calculates the average of numeric values.
   
   Example: Find the average score of each student.
   ```
   db.students.aggregate([
     { $group: { _id: "$student_id", avgScore: { $avg: "$score" } } }
   ])
   ```
   
   iii. ** $min **: Returns the minimum value in a group.
   
   Example: Find the youngest age in each department.
   ```
   db.employees.aggregate([
     { $group: { _id: "$department", youngest: { $min: "$age" } } }
   ])
   ```
   
   iv. ** $max **: Returns the maximum value in a group.
   
   Example: Find the highest score in each subject.
   ```
   db.exams.aggregate([
     { $group: { _id: "$subject", highestScore: { $max: "$score" } } }
   ])
   ```
   
   v. ** $push **: Adds values to an array.
   
   Example: Group all reviews by product.
   ```
   db.reviews.aggregate([
     { $group: { _id: "$product_id", reviews: { $push: "$review" } } }
   ])
   ```
   
   vi. ** $addToSet **: Adds unique values to an array (no duplicates).
   
   Example: List all unique tags used in blog posts.
   ```
   db.posts.aggregate([
     { $group: { _id: null, uniqueTags: { $addToSet: "$tags" } } }
   ])
   ```
   
   vii. ** $first **: Returns the first document from each group.
   
   Example: Get the first order date for each customer.
   ```
   db.orders.aggregate([
     { $sort: { orderDate: 1 } },
     { $group: { _id: "$customerId", firstOrderDate: { $first: "$orderDate" } } }
   ])
   ```
   viii. ** $last **: Returns the last document from each group.
   
   Example: Get the most recent order for each customer.
   ```
   db.orders.aggregate([
     { $sort: { orderDate: 1 } },
     { $group: { _id: "$customerId", lastOrderDate: { $last: "$orderDate" } } }
   ])
   ```
   ix. ** $stdDevSamp **: Returns the sample standard deviation of the input values.
   
   Example: Calculate the standard deviation of scores in each class.
   ```
   db.scores.aggregate([
     { $group: { _id: "$class", stdDev: { $stdDevSamp: "$score" } } }
   ])
   ```
   
   x. ** $stdDevPop **: Returns the population standard deviation of the input values.
   
   Example: Calculate the population standard deviation of salaries in each department.
   ```
   db.employees.aggregate([
     { $group: { _id: "$department", stdDev: { $stdDevPop: "$salary" } } }
   ])
   ```
   
   Accumulator operators are used in aggregation stages, specifically within the $group and $project stages, to perform calculations or data transformations. These operators process multiple documents to return a single value, such as sums, averages, or concatenated strings.

Example: { $group: { _id: "$cust_id", totalAmount: { $sum: "$amount" } } }

4. ### $sort ###

Sorts all input documents and outputs them in sorted order.

{ $sort: {<field1>: <-1 | 1>, <field2>: <-1 | 1> ...}}
Example: { $sort: { age: -1 } } (sort by age in descending order)

5. ### $limit ###

Limits the number of documents passed to the next stage.

Example: { $limit: 5 }

6. ### $skip ###

Skips a specified number of documents before passing the remaining documents to the next stage.

Example: { $skip: 10 }

7. ### $unwind ###

Deconstructs an array field from the input documents to output a document for each element in the array.
{ $unwind: <arrayReferenceExpression> }

Example: { $unwind: "$tags" },
town> db.peoples.aggregate([{$unwind: "$tags"}, {$group: {_id: {tags: "$tags"}}} ])

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

Example: { $out: "filteredResults" },

db.peoples.aggregate([ { $group: { _id: {age: "$age", eyeColor: "$eyeColor"}}}, {$out: "<ANewCollectionName>"} ])

10. ### $count ###

Counts the number of documents that are passed to it and outputs the count as a document.


Example: { $count: "num_docs" }
db.peoples.aggregate([{ $match: {age:{$lte: 25}, gender:'male'}}, {$group: {_id: {name:'$name', isActive:'$isActive', eyeColor:'$eyeColor'}}}, {$count: "allDocumentsCount"}])
[ { allDocumentsCount: 151 } ]

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
```
{
  $bucket: {
    groupBy: "$price",
    boundaries: [0, 100, 200, 300],
    default: "Other"
  }
}
```
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

## Limit of mongoDB ##

* All aggregation stages can use maximum of 100 MB of RAM
* Server will return an error if the ram limit is exceeded
* { allowDiskUse: true } - will enable MongoDB to write data of stages to the temporal files

eg: db.peoples.aggregate([], {allowDiskUse: true})

## Transcation ##

A transaction in the context of databases is a sequence of one or more operations (such as queries, updates, or deletions) that are treated as a single unit of work. The key characteristic of a transaction is that it is atomic; that is, all the operations within the transaction must be fully completed, or none of them should be. This ensures that the database remains in a consistent state, even in the case of errors or failures.

### Key Properties of Transactions: ACID ###

ACID is an acronym that stands for Atomicity, Consistency, Isolation, and Durability. These properties are applied to database transactions to ensure reliable and consistent data management. Let's break down how each ACID property is applied in a typical database transaction and provide an example to illustrate.

### Atomicity: ###

Application: Ensures that all operations within a transaction are completed successfully, or none are. If any operation fails, the entire transaction is rolled back to its initial state, ensuring that partial updates do not occur.

Example: Consider a bank transfer where money is deducted from one account and credited to another. Atomicity ensures that either both actions are completed or neither is, preventing a situation where money is deducted without being credited elsewhere.

### Consistency: ###

Application: Ensures that a transaction brings the database from one valid state to another, maintaining all predefined rules (such as data types, foreign keys, constraints, etc.). The database must remain consistent before and after the transaction.

Example: In the same bank transfer scenario, consistency ensures that the sum of the balances before and after the transaction remains the same, adhering to rules like non-negative account balances.

### Isolation: ###

Application: Ensures that concurrent transactions do not interfere with each other. Each transaction is executed in isolation, so intermediate states are not visible to other transactions. The final state reflects all transactions as if they were executed sequentially.

Example: If two transactions are running simultaneously to transfer funds from the same account, isolation prevents them from affecting each other until one is complete. Thus, no other transaction sees the intermediate results of a transaction.

### Durability: ###

Application: Ensures that once a transaction is committed, it remains permanently recorded in the database, even in the event of a system failure (like a crash or power loss). This is typically achieved through writing changes to persistent storage.

Example: After the bank transfer is successfully completed, the new balances are stored in a durable way so that even if the system crashes immediately afterward, the changes are not lost.

## Sharding in MongoDB ##

Sharding is a method used in MongoDB to distribute data across multiple servers or clusters to achieve horizontal scaling. By dividing large datasets into smaller, more manageable pieces, called shards, MongoDB can handle large amounts of data and high-traffic applications more efficiently.

### Key Concepts in Sharding ###

* Shard Key: A field or set of fields that determines how data is partitioned across shards.
* Chunks: MongoDB divides sharded data into chunks, which are ranges of the shard key values. Each chunk is assigned to a shard.
* Balancing: MongoDB automatically moves chunks across shards to ensure an even distribution of data and load.
* Query Routing: The mongos router handles queries and directs them to the appropriate shard(s) based on the shard key and query parameters.

## Time to live (TTL) ##

TTL (Time to Live) in MongoDB refers to a feature that allows you to automatically delete documents from a collection after a specified period. This feature is useful for applications that require data to be removed after a certain time, such as session data, logs, or temporary data.

### Creating a TTL Index ###

Let's say you have a collection named sessions where each document represents a user session, and you want sessions to expire 24 hours (86,400 seconds) after they are created.

1. ** Insert a Document: **

```
db.sessions.insert({
  userId: "12345",
  createdAt: new Date()  // This field will be used for TTL
});
```

2. ** Create a TTL Index: **

```
db.sessions.createIndex(
  { "createdAt": 1 },
  { expireAfterSeconds: 86400 }
);
```

In this example:

`{ "createdAt": 1 }` specifies the field (`createdAt`) to index and the sort order (1 for ascending).
`{ expireAfterSeconds: 86400 }` sets the TTL to 24 hours.

## MAXIMUM DOCUMENT SIZE IS 16MB ##

## Storage Enginers ##

Storage engines in MongoDB are components that manage how data is stored, retrieved, and updated on disk. They are responsible for the actual data management tasks such as reading and writing data to storage, caching data in memory, handling data compression, and ensuring durability and consistency of the data.

### Key Storage Engines in MongoDB ###

1. ** WiredTiger: **

** Default Engine: ** WiredTiger is the default storage engine starting from MongoDB 3.2.

2. ** MMAPv1: **

Legacy Engine: MMAPv1 was the original storage engine used in early versions of MongoDB but is no longer supported in versions from MongoDB 4.2 onwards.

3. ** In-Memory Storage Engine **

4. ** Encrypted storage engine **

## ObjectId ##


** ObjectId ** in BSON (Binary JSON) is a unique identifier for documents in a MongoDB collection. It is a 12-byte hexadecimal value that MongoDB uses as the default value for the `_id` field in a document if no other value is provided. The `_id` field serves as the primary key for the document, ensuring that every document within a collection has a unique identifier.

### Structure of ObjectId ###

An `ObjectId` consists of 12 bytes, broken down into the following components:

1. ** Timestamp (4 bytes): ** The first 4 bytes represent a Unix timestamp in seconds. This is the number of seconds since the Unix epoch (January 1, 1970). The timestamp provides the date and time when the ObjectId was created.

2. ** Machine Identifier (3 bytes): ** The next 3 bytes are a unique identifier for the machine on which the ObjectId was generated. This usually represents a hash of the machine's hostname.

3. ** Process Identifier (2 bytes): ** The following 2 bytes are derived from the process identifier (PID) of the MongoDB process that generated the ObjectId. This ensures uniqueness across different processes running on the same machine.

4. ** Counter (3 bytes): ** The last 3 bytes are a counter that starts with a random value and is incremented each time a new ObjectId is created by the same process. This counter helps ensure uniqueness even when multiple ObjectIds are generated within the same second on the same machine and process.

Example:

```
{
  "_id": ObjectId("64ac1234f65a123456789012"),
  "name": "John Doe",
  "email": "johndoe@example.com"
}
```
## Cursor methods ##

In MongoDB, a cursor is an object that allows you to iterate over the results of a query. When you perform a find operation in MongoDB, it returns a cursor that you can use to access the documents in the result set. Cursors provide various methods to manipulate and control the data returned by a query. Here are some common cursor methods and their descriptions:

1. ### forEach() ###
Description: Iterates over each document in the cursor and applies a JavaScript function to each document.
Usage:
```
db.collection.find().forEach(function(doc) {
  print(doc.name);
});
```
Example: This example prints the name field of each document in the collection.

2. ### toArray() ###

Description: Converts the documents returned by the cursor into an array. This is useful when you want to work with the entire result set at once.
Usage:
```
const docs = db.collection.find().toArray();
printjson(docs);
```
Example: This example retrieves all documents and prints them in JSON format.
3. ### next() ###

Description: Returns the next document in the cursor. Useful for manual iteration.
Usage:
```
const cursor = db.collection.find();
const doc = cursor.next();
printjson(doc);
```
Example: This example fetches the next document from the cursor and prints it.

4. ### hasNext() ###
Description: Returns true if there is another document in the cursor, and false otherwise.
Usage:
```
const cursor = db.collection.find();
while (cursor.hasNext()) {
  printjson(cursor.next());
}
```
Example: This example iterates over all documents using a while loop.

5. ### count() ###

Description: Returns the total number of documents that match the query criteria, regardless of the limit or skip applied.
Usage:
```
const count = db.collection.find().count();
print(count);
```
Example: This example prints the total number of documents matching the query.

6. ### limit() ###

Description: Limits the number of documents returned by the cursor.
Usage:
```
db.collection.find().limit(5);
```
Example: This example limits the query result to 5 documents.

7. ### skip() ###

Description: Skips a specified number of documents in the result set, which is useful for pagination.
Usage:
```
db.collection.find().skip(10);
```
Example: This example skips the first 10 documents in the result set.

8. ### sort() ###

Description: Sorts the documents in the cursor by a specified field or fields.
Usage:
```
db.collection.find().sort({ age: 1 });
```
Example: This example sorts the documents in ascending order by the age field.

9. ### batchSize() ###

Description: Controls the number of documents returned in each batch of query results.
```
db.collection.find().batchSize(100);
```

Example: This example sets the batch size to 100 documents.

10. ### close() ###

Description: Closes the cursor, freeing up resources on the server. It's typically used in client applications where cursors are opened and left unclosed.
Usage:
```
const cursor = db.collection.find();
cursor.close();
```

Example: This example closes the cursor after use.

11. ### maxTimeMS() ###

Description: Specifies a time limit for processing operations on the cursor, in milliseconds. Useful for avoiding long-running queries.
Usage:
```
db.collection.find().maxTimeMS(5000);
```
Example: This example limits the query processing time to 5000 milliseconds (5 seconds).


## $unwind ##

The $unwind stage in MongoDB's aggregation framework is used to deconstruct an array field from the input documents, creating a separate document for each element of the array. This operation is useful when you need to manipulate or analyze data that is stored in arrays within documents.

### How $unwind Works ###
When you use $unwind, it breaks down an array into multiple documents, each containing one element of the array along with the rest of the document fields. This is especially useful when you want to:

Query on individual elements of an array.
Perform operations like $group, $sort, or $match on the elements of an array.
Basic Syntax
The basic syntax for $unwind is:
```
{
  $unwind: {
    path: "<field path>",
    preserveNullAndEmptyArrays: <boolean> // Optional
    includeArrayIndex: "<string>"         // Optional
  }
}
```
path: The field path of the array you want to unwind. The field name must be prefixed with a $ sign (e.g., $tags).
preserveNullAndEmptyArrays (optional): A boolean that, when set to true, keeps the documents if the array is null, missing, or empty. The default is false.
includeArrayIndex (optional): A string name for a new field that will contain the array index of the unwound item in its original array.

** Example Usage **
Suppose you have a collection named peoples with documents that include a field called tags, which is an array of tags associated with each person:

```
JSON:
{
    "_id": 1,
    "name": "Alice",
    "tags": ["developer", "blogger", "traveler"]
},
{
    "_id": 2,
    "name": "Bob",
    "tags": ["manager", "coach"]
}
```
Example 1: Basic Unwind
To unwind the tags array so that each tag is in a separate document:

```
db.peoples.aggregate([
  {
    $unwind: "$tags"
  }
])
```
Result:

json
```
{ "_id": 1, "name": "Alice", "tags": "developer" }
{ "_id": 1, "name": "Alice", "tags": "blogger" }
{ "_id": 1, "name": "Alice", "tags": "traveler" }
{ "_id": 2, "name": "Bob", "tags": "manager" }
{ "_id": 2, "name": "Bob", "tags": "coach" }
```
Each tag now appears as a separate document.

## $merge ##

The $merge stage in MongoDB's aggregation framework is used to merge the output of an aggregation pipeline into an existing collection. This stage allows you to write the aggregation results directly back to a specified collection, either by inserting new documents, updating existing ones, or replacing documents entirely.


The $merge stage in MongoDB's aggregation framework is used to merge the output of an aggregation pipeline into an existing collection. This stage allows you to write the aggregation results directly back to a specified collection, either by inserting new documents, updating existing ones, or replacing documents entirely.

## Key Features of $merge ##

Writing to Collections: $merge writes the results of the aggregation pipeline to a specified collection. This is useful for updating materialized views or transforming and saving data.
Flexible Update Options: $merge offers various options for how documents are handled when they already exist in the target collection.
Atomic Operations: The operations performed by $merge are atomic for each document.
Syntax
The basic syntax for $merge is as follows:

```
{
  $merge: {
    into: "<collectionName>",
    on: <identifierField>,    // Optional
    whenMatched: <operation>, // Optional
    whenNotMatched: <operation> // Optional
  }
}
```
** into: ** The name of the target collection where the output will be merged.
** on: ** The field or fields used to identify matching documents in the target collection. Defaults to _id if not specified.
** whenMatched: ** Specifies what to do if a document from the aggregation matches an existing document in the target collection.
** whenNotMatched: ** Specifies what to do if a document from the aggregation does not match any existing document in the target collection.


