sticsearch: the definitive guide » getting started » sorting and relevance » string sorting and multifields
«  sorting     what is relevance?  »
string sorting and multifieldsedit

sorting
string sorting and multifields
what is relevance?
fielddata
Analyzed string fields are also multivalue fields, but sorting on them seldom gives you the results you want. If you analyze a string like fine old art, it results in three terms. We probably want to sort alphabetically on the first term, then the second term, and so forth, but Elasticsearch doesn’t have this information at its disposal at sort time.

You could use the min and max sort modes (it uses min by default), but that will result in sorting on either art or old, neither of which was the intent.

In order to sort on a string field, that field should contain one term only: the whole not_analyzed string. But of course we still need the field to be analyzed in order to be able to query it as full text.

The naive approach to indexing the same string in two ways would be to include two separate fields in the document: one that is analyzed for searching, and one that is not_analyzed for sorting.

But storing the same string twice in the _source field is waste of space. What we really want to do is to pass in a single field but to index it in two different ways. All of the core field types (strings, numbers, Booleans, dates) accept a fields parameter that allows you to transform a simple mapping like

"tweet": {
    "type":     "string",
    "analyzer": "english"
}
into a multifield mapping like this:

"tweet": { 
    "type":     "string",
    "analyzer": "english",
    "fields": {
        "raw": { 
            "type":  "string",
            "index": "not_analyzed"
        }
    }
}
VIEW IN SENSE


The main tweet field is just the same as before: an analyzed full-text field.



The new tweet.raw subfield is not_analyzed.

Now, or at least as soon as we have reindexed our data, we can use the tweet field for search and the tweet.raw field for sorting:

GET /_search
{
    "query": {
        "match": {
            "tweet": "elasticsearch"
        }
    },
    "sort": "tweet.raw"
}
VIEW IN SENSE
Warning
Sorting on a full-text analyzed field can use a lot of memory. See Fielddata for more information.

«  sorting     what is relevance?  »
