full-text searchedit

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
The searches so far have been simple: single names, filtered by age. Let’s try a more advanced, full-text search—a task that traditional databases would really struggle with.

We are going to search for all employees who enjoy rock climbing:

GET /megacorp/employee/_search
{
    "query" : {
        "match" : {
            "about" : "rock climbing"
        }
    }
}
VIEW IN SENSE
You can see that we use the same match query as before to search the about field for “rock climbing.” We get back two matching documents:

{
   ...
   "hits": {
      "total":      2,
      "max_score":  0.16273327,
      "hits": [
         {
            ...
            "_score":         0.16273327, 
            "_source": {
               "first_name":  "John",
               "last_name":   "Smith",
               "age":         25,
               "about":       "I love to go rock climbing",
               "interests": [ "sports", "music" ]
            }
         },
         {
            ...
            "_score":         0.016878016, 
            "_source": {
               "first_name":  "Jane",
               "last_name":   "Smith",
               "age":         32,
               "about":       "I like to collect rock albums",
               "interests": [ "music" ]
            }
         }
      ]
   }
}
 

The relevance scores

By default, Elasticsearch sorts matching results by their relevance score, that is, by how well each document matches the query. The first and highest-scoring result is obvious: John Smith’s about field clearly says “rock climbing” in it.

But why did Jane Smith come back as a result? The reason her document was returned is because the word “rock” was mentioned in her about field. Because only “rock” was mentioned, and not “climbing,” her _score is lower than John’s.

This is a good example of how Elasticsearch can search within full-text fields and return the most relevant results first. This concept of relevance is important to Elasticsearch, and is a concept that is completely foreign to traditional relational databases, in which a record either matches or it doesn’t.

«  more-complicated searches     phrase search  »

