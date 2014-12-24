
elasticsearch: the definitive guide » getting started » you know, for search… » indexing employee documents
«  finding your feet     retrieving a document  »
indexing employee documentsedit

installing elasticsearch
running elasticsearch
talking to elasticsearch
document oriented
finding your feet
indexing employee documents
retrieving a document
search lite
search with query dsl
more-complicated searches
full-text search
phrase search
highlighting our searches
analytics
tutorial conclusion
distributed nature
next steps
The first order of business is storing employee data. This will take the form of an employee document': a single document represents a single employee. The act of storing data in Elasticsearch is called indexing, but before we can index a document, we need to decide where to store it.

In Elasticsearch, a document belongs to a type, and those types live inside an index. You can draw some (rough) parallels to a traditional relational database:

Relational DB  ⇒ Databases ⇒ Tables ⇒ Rows      ⇒ Columns
Elasticsearch  ⇒ Indices   ⇒ Types  ⇒ Documents ⇒ Fields
An Elasticsearch cluster can contain multiple indices (databases), which in turn contain multiple types (tables). These types hold multiple documents (rows), and each document has multiple fields (columns).

Index Versus Index Versus Index

You may already have noticed that the word index is overloaded with several meanings in the context of Elasticsearch. A little clarification is necessary:

Index (noun)
As explained previously, an index is like a database in a traditional relational database. It is the place to store related documents. The plural of index is indices or indexes.
Index (verb)
To index a document is to store a document in an index (noun) so that it can be retrieved and queried. It is much like the INSERT keyword in SQL except that, if the document already exists, the new document would replace the old.
Inverted index
Relational databases add an index, such as a B-tree index, to specific columns in order to improve the speed of data retrieval. Elasticsearch and Lucene use a structure called an inverted index for exactly the same purpose.

By default, every field in a document is indexed (has an inverted index) and thus is searchable. A field without an inverted index is not searchable. We discuss inverted indexes in more detail in Inverted Index.

So for our employee directory, we are going to do the following:

Index a document per employee, which contains all the details of a single employee.
Each document will be of type employee.
That type will live in the megacorp index.
That index will reside within our Elasticsearch cluster.
In practice, this is easy (even though it looks like a lot of steps). We can perform all of those actions in a single command:

PUT /megacorp/employee/1
{
    "first_name" : "John",
    "last_name" :  "Smith",
    "age" :        25,
    "about" :      "I love to go rock climbing",
    "interests": [ "sports", "music" ]
}
VIEW IN SENSE
Notice that the path /megacorp/employee/1 contains three pieces of information:

megacorp
The index name
employee
The type name
1
The ID of this particular employee
The request body—the JSON document—contains all the information about this employee. His name is John Smith, he’s 25, and enjoys rock climbing.

Simple! There was no need to perform any administrative tasks first, like creating an index or specifying the type of data that each field contains. We could just index a document directly. Elasticsearch ships with defaults for everything, so all the necessary administration tasks were taken care of in the background, using default values.

Before moving on, let’s add a few more employees to the directory:

PUT /megacorp/employee/2
{
    "first_name" :  "Jane",
    "last_name" :   "Smith",
    "age" :         32,
    "about" :       "I like to collect rock albums",
    "interests":  [ "music" ]
}

PUT /megacorp/employee/3
{
    "first_name" :  "Douglas",
    "last_name" :   "Fir",
    "age" :         35,
    "about":        "I like to build cabinets",
    "interests":  [ "forestry" ]
}
VIEW IN SENSE
«  finding your feet     retrieving a document  »
