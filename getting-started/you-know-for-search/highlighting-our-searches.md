highlighting our searchesedit

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
Many applications like to highlight snippets of text from each search result so the user can see why the document matched the query. Retrieving highlighted fragments is easy in Elasticsearch.

Let’s rerun our previous query, but add a new highlight parameter:

GET /megacorp/employee/_search
{
    "query" : {
        "match_phrase" : {
            "about" : "rock climbing"
        }
    },
    "highlight": {
        "fields" : {
            "about" : {}
        }
    }
}
VIEW IN SENSE
When we run this query, the same hit is returned as before, but now we get a new section in the response called highlight. This contains a snippet of text from the about field with the matching words wrapped in <em></em> HTML tags:

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
            },
            "highlight": {
               "about": [
                  "I love to go <em>rock</em> <em>climbing</em>" 
               ]
            }
         }
      ]
   }
}


The highlighted fragment from the original text

You can read more about the highlighting of search snippets in the highlighting reference documentation.

«  phrase search     analytics  »

