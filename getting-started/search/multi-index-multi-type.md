
elasticsearch: the definitive guide » getting started » searching—the basic tools » multi-index, multitype
«  the empty search     pagination  »
multi-index, multitypeedit

the empty search
multi-index, multitype
pagination
search lite
Did you notice that the results from the preceding empty search contained documents of different types--user and tweet--from two different indices--us and gb?

By not limiting our search to a particular index or type, we have searched across all documents in the cluster. Elasticsearch forwarded the search request in parallel to a primary or replica of every shard in the cluster, gathered the results to select the overall top 10, and returned them to us.

Usually, however, you will want to search within one or more specific indices, and probably one or more specific types. We can do this by specifying the index and type in the URL, as follows:

/_search
Search all types in all indices
/gb/_search
Search all types in the gb index
/gb,us/_search
Search all types in the gb and us indices
/g*,u*/_search
Search all types in any indices beginning with g or beginning with u
/gb/user/_search
Search type user in the gb index
/gb,us/user,tweet/_search
Search types user and tweet in the gb and us indices
/_all/user,tweet/_search
Search types user and tweet in all indices
When you search within a single index, Elasticsearch forwards the search request to a primary or replica of every shard in that index, and then gathers the results from each shard. Searching within multiple indices works in exactly the same way—there are just more shards involved.

Tip
Searching one index that has five primary shards is exactly equivalent to searching five indices that have one primary shard each.

Later, you will see how this simple fact makes it easy to scale flexibly as your requirements change.

«  the empty search     pagination  »
