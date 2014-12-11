
elasticsearch: the definitive guide » getting started » full-body search » most important queries and filters
«  queries and filters     combining queries with filters  »
most important queries and filtersedit

empty search
query dsl
queries and filters
most important queries and filters
combining queries with filters
validating queries
While Elasticsearch comes with many queries and filters, you will use just a few frequently. We discuss them in much greater detail in Search in Depth but next we give you a quick introduction to the most important queries and filters.

term filteredit

The term filter is used to filter by exact values, be they numbers, dates, Booleans, or not_analyzed exact-value string fields:

{ "term": { "age":    26           }}
{ "term": { "date":   "2014-09-01" }}
{ "term": { "public": true         }}
{ "term": { "tag":    "full_text"  }}
VIEW IN SENSE
terms filteredit

The terms filter is the same as the term filter, but allows you to specify multiple values to match. If the field contains any of the specified values, the document matches:

{ "terms": { "tag": [ "search", "full_text", "nosql" ] }}
VIEW IN SENSE
range filteredit

The range filter allows you to find numbers or dates that fall into a specified range:

{
    "range": {
        "age": {
            "gte":  20,
            "lt":   30
        }
    }
}
VIEW IN SENSE
The operators that it accepts are as follows:

gt
Greater than
gte
Greater than or equal to
lt
Less than
lte
Less than or equal to
exists and missing filtersedit

The exists and missing filters are used to find documents in which the specified field either has one or more values (exists) or doesn’t have any values (missing). It is similar in nature to IS_NULL (missing) and NOT IS_NULL (exists)in SQL:

{
    "exists":   {
        "field":    "title"
    }
}
VIEW IN SENSE
These filters are frequently used to apply a condition only if a field is present, and to apply a different condition if it is missing.

bool filteredit

The bool filter is used to combine multiple filter clauses using Boolean logic. It accepts three parameters:

must
These clauses must match, like and.
must_not
These clauses must not match, like not.
should
At least one of these clauses must match, like or.
Each of these parameters can accept a single filter clause or an array of filter clauses:

{
    "bool": {
        "must":     { "term": { "folder": "inbox" }},
        "must_not": { "term": { "tag":    "spam"  }},
        "should": [
                    { "term": { "starred": true   }},
                    { "term": { "unread":  true   }}
        ]
    }
}
VIEW IN SENSE
match_all queryedit

The match_all query simply matches all documents. It is the default query that is used if no query has been specified:

{ "match_all": {}}
VIEW IN SENSE
This query is frequently used in combination with a filter—for instance, to retrieve all emails in the inbox folder. All documents are considered to be equally relevant, so they all receive a neutral _score of 1.

match queryedit

The match query should be the standard query that you reach for whenever you want to query for a full-text or exact value in almost any field.

If you run a match query against a full-text field, it will analyze the query string by using the correct analyzer for that field before executing the search:

{ "match": { "tweet": "About Search" }}
VIEW IN SENSE
If you use it on a field containing an exact value, such as a number, a date, a Boolean, or a not_analyzed string field, then it will search for that exact value:

{ "match": { "age":    26           }}
{ "match": { "date":   "2014-09-01" }}
{ "match": { "public": true         }}
{ "match": { "tag":    "full_text"  }}
VIEW IN SENSE
Tip
For exact-value searches, you probably want to use a filter instead of a query, as a filter will be cached.

Unlike the query-string search that we showed in Search Lite, the match query does not use a query syntax like +user_id:2 +tweet:search. It just looks for the words that are specified. This means that it is safe to expose to your users via a search field; you control what fields they can query, and it is not prone to throwing syntax errors.

multi_match queryedit

The multi_match query allows to run the same match query on multiple fields:

{
    "multi_match": {
        "query":    "full text search",
        "fields":   [ "title", "body" ]
    }
}
VIEW IN SENSE
bool queryedit

The bool query, like the bool filter, is used to combine multiple query clauses. However, there are some differences. Remember that while filters give binary yes/no answers, queries calculate a relevance score instead. The bool query combines the _score from each must or should clause that matches. This query accepts the following parameters:

must
Clauses that must match for the document to be included.
must_not
Clauses that must not match for the document to be included.
should
If these clauses match, they increase the _score; otherwise, they have no effect. They are simply used to refine the relevance score for each document.
The following query finds documents whose title field matches the query string how to make millions and that are not marked as spam. If any documents are starred or are from 2014 onward, they will rank higher than they would have otherwise. Documents that match both conditions will rank even higher:

{
    "bool": {
        "must":     { "match": { "title": "how to make millions" }},
        "must_not": { "match": { "tag":   "spam" }},
        "should": [
            { "match": { "tag": "starred" }},
            { "range": { "date": { "gte": "2014-01-01" }}}
        ]
    }
}
VIEW IN SENSE
Tip
If there are no must clauses, at least one should clause has to match. However, if there is at least one must clause, no should clauses are required to match.

«  queries and filters     combining queries with filters  »
