
elasticsearch: the definitive guide » aggregations » high-level concepts » metrics
«  buckets     combining the two  »
metricsedit

buckets
metrics
combining the two
Buckets allow us to partition documents into useful subsets, but ultimately what we want is some kind of metric calculated on those documents in each bucket. Bucketing is the means to an end: it provides a way to group documents in a way that you can calculate interesting metrics.

Most metrics are simple mathematical operations (for example, min, mean, max, and sum) that are calculated using the document values. In practical terms, metrics allow you to calculate quantities such as the average salary, or the maximum sale price, or the 95th percentile for query latency.

«  buckets     combining the two  »
