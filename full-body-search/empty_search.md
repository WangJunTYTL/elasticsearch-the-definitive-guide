
elasticsearch: the definitive guide » getting started » full-body search » empty search
«  full-body search     query dsl  »
empty searchedit

empty search
query dsl
queries and filters
most important queries and filters
combining queries with filters
validating queries
Let’s start with the simplest form of the search API, the empty search, which returns all documents in all indices:

GET /_search
{} 
VIEW IN SENSE


This is an empty request body.

Just as with a query-string search, you can search on one, many, or _all indices, and one, many, or all types:

GET /index_2014*/type1,type2/_search
{}
And you can use the from and size parameters for pagination:

GET /_search
{
  "from": 30,
  "size": 10
}
A GET Request with a Body?

The HTTP libraries of certain languages (notably JavaScript) don’t allow GET requests to have a request body. In fact, some users are suprised that GET requests are ever allowed to have a body.

The truth is that RFC 7231--the RFC that deals with HTTP semantics and content --does not define what should happen to a GET request with a body! As a result, some HTTP servers allow it, and some—especially caching proxies—don’t.

The authors of Elasticsearch prefer using GET for a search request because they feel that it describes the action—retrieving information—better than the POST verb. However, because GET with a request body is not universally supported, the search API also accepts POST requests:

POST /_search
{
  "from": 30,
  "size": 10
}
The same rule applies to any other GET API that requires a request body.

We present aggregations in depth in Aggregations , but for now, we’re going to focus just on the query.

Instead of the cryptic query-string approach, a request body search allows us to write queries by using the query domain-specific language, or query DSL.

«  full-body search     query dsl  
