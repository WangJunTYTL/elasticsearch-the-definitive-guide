
elasticsearch: the definitive guide » aggregations » high-level concepts
«  aggregations     buckets  »
high-level conceptsedit

buckets
metrics
combining the two
Like the query DSL, aggregations have a composable syntax: independent units of functionality can be mixed and matched to provide the custom behavior that you need. This means that there are only a few basic concepts to learn, but nearly limitless combinations of those basic components.

To master aggregations, you need to understand only two main concepts:

Buckets
Collections of documents that meet a criterion
Metrics
Statistics calculated on the documents in a bucket
That’s it! Every aggregation is simply a combination of one or more buckets and zero or more metrics. To translate into rough SQL terms:

SELECT COUNT(color) 
FROM table
GROUP BY color 


COUNT(color) is equivalent to a metric.



GROUP BY color is equivalent to a bucket.

Buckets are conceptually similar to grouping in SQL, while metrics are similar to COUNT(), SUM(), MAX(), and so forth.

Let’s dig into both of these concepts and see what they entail.

«  aggregations     buckets  »
