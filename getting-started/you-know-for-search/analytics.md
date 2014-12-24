analyticsedit

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
Finally, we come to our last business requirement: allow managers to run analytics over the employee directory. Elasticsearch has functionality called aggregations, which allow you to generate sophisticated analytics over your data. It is similar to GROUP BY in SQL, but much more powerful.

For example, let’s find the most popular interests enjoyed by our employees:

GET /megacorp/employee/_search
{
  "aggs": {
    "all_interests": {
      "terms": { "field": "interests" }
    }
  }
}
VIEW IN SENSE
Ignore the syntax for now and just look at the results:

{
   ...
   "hits": { ... },
   "aggregations": {
      "all_interests": {
         "buckets": [
            {
               "key":       "music",
               "doc_count": 2
            },
            {
               "key":       "forestry",
               "doc_count": 1
            },
            {
               "key":       "sports",
               "doc_count": 1
            }
         ]
      }
   }
}
We can see that two employees are interested in music, one in forestry, and one in sports. These aggregations are not precalculated; they are generated on the fly from the documents that match the current query. If we want to know the popular interests of people called Smith, we can just add the appropriate query into the mix:

GET /megacorp/employee/_search
{
  "query": {
    "match": {
      "last_name": "smith"
    }
  },
  "aggs": {
    "all_interests": {
      "terms": {
        "field": "interests"
      }
    }
  }
}
VIEW IN SENSE
The all_interests aggregation has changed to include only documents matching our query:

  ...
  "all_interests": {
     "buckets": [
        {
           "key": "music",
           "doc_count": 2
        },
        {
           "key": "sports",
           "doc_count": 1
        }
     ]
  }
Aggregations allow hierarchical rollups too. For example, let’s find the average age of employees who share a particular interest:

GET /megacorp/employee/_search
{
    "aggs" : {
        "all_interests" : {
            "terms" : { "field" : "interests" },
            "aggs" : {
                "avg_age" : {
                    "avg" : { "field" : "age" }
                }
            }
        }
    }
}
VIEW IN SENSE
The aggregations that we get back are a bit more complicated, but still fairly easy to understand:

  ...
  "all_interests": {
     "buckets": [
        {
           "key": "music",
           "doc_count": 2,
           "avg_age": {
              "value": 28.5
           }
        },
        {
           "key": "forestry",
           "doc_count": 1,
           "avg_age": {
              "value": 35
           }
        },
        {
           "key": "sports",
           "doc_count": 1,
           "avg_age": {
              "value": 25
           }
        }
     ]
  }
The output is basically an enriched version of the first aggregation we ran. We still have a list of interests and their counts, but now each interest has an additional avg_age, which shows the average age for all employees having that interest.

Even if you don’t understand the syntax yet, you can easily see how complex aggregations and groupings can be accomplished using this feature. The sky is the limit as to what kind of data you can extract!

«  highlighting our searches     tutorial conclusion  »
