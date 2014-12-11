
elasticsearch: the definitive guide » getting started » full-body search » validating queries
«  combining queries with filters     sorting and relevance  »
validating queriesedit

empty search
query dsl
queries and filters
most important queries and filters
combining queries with filters
validating queries
Queries can become quite complex and, especially when combined with different analyzers and field mappings, can become a bit difficult to follow. The validate-query API can be used to check whether a query is valid.

GET /gb/tweet/_validate/query
{
   "query": {
      "tweet" : {
         "match" : "really powerful"
      }
   }
}
VIEW IN SENSE
The response to the preceding validate request tells us that the query is invalid:

{
  "valid" :         false,
  "_shards" : {
    "total" :       1,
    "successful" :  1,
    "failed" :      0
  }
}
understanding errorsedit

To find out why it is invalid, add the explain parameter to the query string:

GET /gb/tweet/_validate/query?explain 
{
   "query": {
      "tweet" : {
         "match" : "really powerful"
      }
   }
}
VIEW IN SENSE


The explain flag provides more information about why a query is invalid.

Apparently, we’ve mixed up the type of query (match) with the name of the field (tweet):

{
  "valid" :     false,
  "_shards" :   { ... },
  "explanations" : [ {
    "index" :   "gb",
    "valid" :   false,
    "error" :   "org.elasticsearch.index.query.QueryParsingException:
                 [gb] No query registered for [tweet]"
  } ]
}
understanding queriesedit

Using the explain parameter has the added advantage of returning a human-readable description of the (valid) query, which can be useful for understanding exactly how your query has been interpreted by Elasticsearch:

GET /_validate/query?explain
{
   "query": {
      "match" : {
         "tweet" : "really powerful"
      }
   }
}
VIEW IN SENSE
An explanation is returned for each index that we query, because each index can have different mappings and analyzers:

{
  "valid" :         true,
  "_shards" :       { ... },
  "explanations" : [ {
    "index" :       "us",
    "valid" :       true,
    "explanation" : "tweet:really tweet:powerful"
  }, {
    "index" :       "gb",
    "valid" :       true,
    "explanation" : "tweet:realli tweet:power"
  } ]
}
From the explanation, you can see how the match query for the query string really powerful has been rewritten as two single-term queries against the tweet field, one for each term.

Also, for the us index, the two terms are really and powerful, while for the gb index, the terms are realli and power. The reason for this is that we changed the tweet field in the gb index to use the english analyzer.

«  combining queries with filters     sorting and relevance  »
