search liteedit

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
A GET is fairly simple—you get back the document that you ask for. Let’s try something a little more advanced, like a simple search!

The first search we will try is the simplest search possible. We will search for all employees, with this request:

GET /megacorp/employee/_search
VIEW IN SENSE
You can see that we’re still using index megacorp and type employee, but instead of specifying a document ID, we now use the _search endpoint. The response includes all three of our documents in the hits array. By default, a search will return the top 10 results.

{
   "took":      6,
   "timed_out": false,
   "_shards": { ... },
   "hits": {
      "total":      3,
      "max_score":  1,
      "hits": [
         {
            "_index":         "megacorp",
            "_type":          "employee",
            "_id":            "3",
            "_score":         1,
            "_source": {
               "first_name":  "Douglas",
               "last_name":   "Fir",
               "age":         35,
               "about":       "I like to build cabinets",
               "interests": [ "forestry" ]
            }
         },
         {
            "_index":         "megacorp",
            "_type":          "employee",
            "_id":            "1",
            "_score":         1,
            "_source": {
               "first_name":  "John",
               "last_name":   "Smith",
               "age":         25,
               "about":       "I love to go rock climbing",
               "interests": [ "sports", "music" ]
            }
         },
         {
            "_index":         "megacorp",
            "_type":          "employee",
            "_id":            "2",
            "_score":         1,
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
Note
The response not only tells us which documents matched, but also includes the whole document itself: all the information that we need in order to display the search results to the user.

Next, let’s try searching for employees who have “Smith” in their last name. To do this, we’ll use a lightweight search method that is easy to use from the command line. This method is often referred to as a query-string search, since we pass the search as a URL query-string parameter:

GET /megacorp/employee/_search?q=last_name:Smith
VIEW IN SENSE
We use the same _search endpoint in the path, and we add the query itself in the q= parameter. The results that come back show all Smiths:

{
   ...
   "hits": {
      "total":      2,
      "max_score":  0.30685282,
      "hits": [
         {
            ...
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
«  retrieving a document     search with query dsl  »

