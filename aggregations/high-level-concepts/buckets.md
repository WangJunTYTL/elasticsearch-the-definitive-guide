bucketsedit

buckets
metrics
combining the two
A bucket is simply a collection of documents that meet a certain criteria:

An employee would land in either the male or female bucket.
The city of Albany would land in the New York state bucket.
The date 2014-10-28 would land within the October bucket.
As aggregations are executed, the values inside each document are evaluated to determine whether they match a bucket’s criteria. If they match, the document is placed inside the bucket and the aggregation continues.

Buckets can also be nested inside other buckets, giving you a hierarchy or conditional partitioning scheme. For example, Cincinnati would be placed inside the Ohio state bucket, and the entire Ohio bucket would be placed inside the USA country bucket.

Elasticsearch has a variety of buckets, which allow you to partition documents in many ways (by hour, by most-popular terms, by age ranges, by geographical location, and more). But fundamentally they all operate on the same principle: partitioning documents based on a criteria.

«  high-level concepts     metrics  »

