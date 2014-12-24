phrase searchedit

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
Finding individual words in a field is all well and good, but sometimes you want to match exact sequences of words or phrases. For instance, we could perform a query that will match only employee records that contain both “rock” and “climbing” and that display the words are next to each other in the phrase “rock climbing.”

To do this, we use a slight variation of the match query called the match_phrase query:

GET /megacorp/employee/_search
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    }
}
VIEW IN SENSE
This, to no surprise, returns only John Smith’s document:

{
   ...
   "hits": {
      "total":      1,
      "max_score":  0.23013961,
      "hits": [
         {
            ...
            "_score":         0.23013961,
            "_source": {
               "first_name":  "John",
               "last_name":   "Smith",
               "age":         25,
               "about":       "I love to go rock climbing",
               "interests": [ "sports", "music" ]
            }
         }
      ]
   }
}
«  full-text search     highlighting our searches  »

