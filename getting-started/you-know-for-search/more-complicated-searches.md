
elasticsearch: the definitive guide » getting started » you know, for search… » more-complicated searches
«  search with query dsl     full-text search  »
more-complicated searchesedit

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
Let’s make the search a little more complicated. We still want to find all employees with a last name of Smith, but we want only employees who are older than 30. Our query will change a little to accommodate a filter, which allows us to execute structured searches efficiently:

GET /megacorp/employee/_search
{
    "query" : {
        "filtered" : {
            "filter" : {
                "range" : {
                    "age" : { "gt" : 30 } 
                }
            },
            "query" : {
                "match" : {
                    "last_name" : "smith" 
                }
            }
        }
    }
}
VIEW IN SENSE


This portion of the query is a range filter, which will find all ages older than 30—gt stands for greater than.



This portion of the query is the same match query that we used before.

Don’t worry about the syntax too much for now; we will cover it in great detail later. Just recognize that we’ve added a filter that performs a range search, and reused the same match query as before. Now our results show only one employee who happens to be 32 and is named Jane Smith:

{
   ...
   "hits": {
      "total":      1,
      "max_score":  0.30685282,
      "hits": [
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
«  search with query dsl     full-text search  »

