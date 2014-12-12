
elasticsearch: the definitive guide » aggregations » high-level concepts » combining the two
«  metrics     aggregation test-drive  »
combining the twoedit

buckets
metrics
combining the two
An aggregation is a combination of buckets and metrics. An aggregation may have a single bucket, or a single metric, or one of each. It may even have multiple buckets nested inside other buckets. For example, we can partition documents by which country they belong to (a bucket), and then calculate the average salary per country (a metric).

Because buckets can be nested, we can derive a much more complex aggregation:

Partition documents by country (bucket).
Then partition each country bucket by gender (bucket).
Then partition each gender bucket by age ranges (bucket).
Finally, calculate the average salary for each age range (metric)
This will give you the average salary per <country, gender, age> combination. All in one request and with one pass over the data!

«  metrics     aggregation test-drive  »
