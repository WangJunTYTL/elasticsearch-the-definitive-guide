
elasticsearch: the definitive guide » getting started » you know, for search… » search with query dsl
«  search lite     more-complicated searches  »
search with query dsledit

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
Query-string search is handy for ad hoc searches from the command line, but it has its limitations (see Search Lite). Elasticsearch provides a rich, flexible, query language called the query DSL, which allows us to build much more complicated, robust queries.

The domain-specific language (DSL) is specified using a JSON request body. We can represent the previous search for all Smiths like so:

GET /megacorp/employee/_search
{
    "query" : {
        "match" : {
            "last_name" : "Smith"
        }
    }
}
VIEW IN SENSE
This will return the same results as the previous query. You can see that a number of things have changed. For one, we are no longer using query-string parameters, but instead a request body. This request body is built with JSON, and uses a match query (one of several types of queries, which we will learn about later).

«  search lite     more-complicated searches  »
