
elasticsearch: the definitive guide » aggregations » available buckets and metrics
«  one final modification     building bar charts  »
available buckets and metricsedit

high-level concepts
aggregation test-drive
available buckets and metrics
building bar charts
looking at time
scoping aggregations
filtering queries and aggregations
sorting multivalue buckets
approximate aggregations
significant terms
controlling memory use and latency
closing thoughts
Elasticsearch has various buckets and metrics. The reference documentation does a great job describing the parameters and how they affect the component. Instead of re-describing them here, we are simply going to link to the reference docs and provide a brief description. Skim the list so you know what is available, and check the reference docs when you need exact parameters.

bucketsedit

See Global: includes all documents in your index
{See Filter: only includes documents that match the filter
See Missing: all documents which do not have a particular field
See Terms: generates a new bucket for each unique term
See Range: creates arbitrary ranges which documents fall into
See Date Range: similar to Range, but calendar aware
See IPV4 Range: similar to Range, but can handle "IP logic" like CIDR masks, etc
See Geo Distance: similar to Range, but operates on geo points
See Histogram: equal-width, dynamic ranges
See Date Histogram: similar to Histogram, but calendar aware
See Nested: a special bucket for working with nested documents (see Nested aggregations)
See Geohash Grid: partitions documents according to what geohash grid they fall into (see geohash_grid aggregation)
See TopHits: Return the top search results grouped by the value of a field (see Field collapsing)
metricsedit

Individual statistics: See Min, See Max, See Avg, See Sum
{See Stats: calculates min/mean/max/sum/count of documents in bucket
See Extended Stats: Same as stats, except it also includes variance, std deviation, sum of squares
See Value Count: calculates the number of values, which may be different from the number of documents (e.g. multi-valued fields)
See Cardinality: calculates number of distinct/unique values (see Finding Distinct Counts)
See Percentiles: calculates percentiles/quantiles for numeric values in a bucket (see Calculating Percentiles)
See Significant Terms: finds "uncommonly common" terms (see Significant Terms)
«  one final modification     building bar charts  »
