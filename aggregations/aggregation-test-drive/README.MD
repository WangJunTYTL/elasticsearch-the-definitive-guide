aggregation test-driveedit

adding a metric to the mix
buckets inside buckets
one final modification
We could spend the next few pages defining the various aggregations and their syntax, but aggregations are truly best learned by example. Once you learn how to think about aggregations, and how to nest them appropriately, the syntax is fairly trivial.

So let’s just dive in and start with an example. We are going to build some aggregations that might be useful to a car dealer. Our data will be about car transactions: the car model, manufacturer, sale price, when it sold, and more.

First we will bulk-index some data to work with:

POST /cars/transactions/_bulk
{ "index": {}}
{ "price" : 10000, "color" : "red", "make" : "honda", "sold" : "2014-10-28" }
{ "index": {}}
{ "price" : 20000, "color" : "red", "make" : "honda", "sold" : "2014-11-05" }
{ "index": {}}
{ "price" : 30000, "color" : "green", "make" : "ford", "sold" : "2014-05-18" }
{ "index": {}}
{ "price" : 15000, "color" : "blue", "make" : "toyota", "sold" : "2014-07-02" }
{ "index": {}}
{ "price" : 12000, "color" : "green", "make" : "toyota", "sold" : "2014-08-19" }
{ "index": {}}
{ "price" : 20000, "color" : "red", "make" : "honda", "sold" : "2014-11-05" }
{ "index": {}}
{ "price" : 80000, "color" : "red", "make" : "bmw", "sold" : "2014-01-01" }
{ "index": {}}
{ "price" : 25000, "color" : "blue", "make" : "ford", "sold" : "2014-02-12" }
VIEW IN SENSE
Now that we have some data, let’s construct our first aggregation. A car dealer may want to know which color car sells the best. This is easily accomplished using a simple aggregation. We will do this using a terms bucket:

GET /cars/transactions/_search?search_type=count 
{
    "aggs" : { 
        "colors" : { 
            "terms" : {
              "field" : "color" 
            }
        }
    }
}
VIEW IN SENSE


Because we don’t care about search results, we are going to use the count search_type, which will be faster.



Aggregations are placed under the top-level aggs parameter (the longer aggregations will also work if you prefer that).



We then name the aggregation whatever we want: colors, in this example



Finally, we define a single bucket of type terms.

Aggregations are executed in the context of search results, which means it is just another top-level parameter in a search request (for example, using the /_search endpoint). Aggregations can be paired with queries, but we’ll tackle that later in Scoping Aggregations.

Next we define a name for our aggregation. Naming is up to you; the response will be labeled with the name you provide so that your application can parse the results later.

Next we define the aggregation itself. For this example, we are defining a single terms bucket. The terms bucket will dynamically create a new bucket for every unique term it encounters. Since we are telling it to use the color field, the terms bucket will dynamically create a new bucket for each color.

Let’s execute that aggregation and take a look at the results:

{
...
   "hits": {
      "hits": [] 
   },
   "aggregations": {
      "colors": { 
         "buckets": [
            {
               "key": "red", 
               "doc_count": 4 
            },
            {
               "key": "blue",
               "doc_count": 2
            },
            {
               "key": "green",
               "doc_count": 2
            }
         ]
      }
   }
}


No search hits are returned because we used the search_type=count parameter



Our colors aggregation is returned as part of the aggregations field.



The key to each bucket corresponds to a unique term found in the color field. It also always includes doc_count, which tells us the number of docs containing the term.



The count of each bucket represents the number of documents with this color.

The response contains a list of buckets, each corresponding to a unique color (for example, red or green). Each bucket also includes a count of the number of documents that "fell into" that particular bucket. For example, there are four red cars.

The preceding example is operating entirely in real time: if the documents are searchable, they can be aggregated. This means you can take the aggregation results and pipe them straight into a graphing library to generate real-time dashboards. As soon as you sell a silver car, your graphs would dynamically update to include statistics about silver cars.

Voila! Your first aggregation!

«  combining the two     adding a metric to the mix  »
