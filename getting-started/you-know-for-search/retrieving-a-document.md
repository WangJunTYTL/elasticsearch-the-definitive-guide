
elasticsearch: the definitive guide » getting started » you know, for search… » retrieving a document
«  indexing employee documents     search lite  »
retrieving a documentedit

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
Now that we have some data stored in Elasticsearch, we can get to work on the business requirements for this application. The first requirement is the ability to retrieve individual employee data.

This is easy in Elasticsearch. We simply execute an HTTP GET request and specify the address of the document—the index, type, and ID. Using those three pieces of information, we can return the original JSON document:

GET /megacorp/employee/1
VIEW IN SENSE
And the response contains some metadata about the document, and John Smith’s original JSON document as the _source field:

{
  "_index" :   "megacorp",
  "_type" :    "employee",
  "_id" :      "1",
  "_version" : 1,
  "found" :    true,
  "_source" :  {
      "first_name" :  "John",
      "last_name" :   "Smith",
      "age" :         25,
      "about" :       "I love to go rock climbing",
      "interests":  [ "sports", "music" ]
  }
}
Tip
In the same way that we changed the HTTP verb from PUT to GET in order to retrieve the document, we could use the DELETE verb to delete the document, and the HEAD verb to check whether the document exists. To replace an existing document with an updated version, we just PUT it again.

«  indexing employee documents     search lite  »
