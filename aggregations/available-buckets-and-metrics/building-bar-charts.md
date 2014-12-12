building bar chartsedit

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
One of the exciting aspects of aggregations are how easily they are converted into charts and graphs. In this chapter, we are focusing on various analytics that we can wring out of our example dataset. We will also demonstrate the types of charts aggregations can power.

The histogram bucket is particularly useful. Histograms are essentially bar charts, and if you’ve ever built a report or analytics dashboard, you undoubtedly had a few bar charts in it.

The histogram works by specifying an interval. If we were histogramming sale prices, you might specify an interval of 20,000. This would create a new bucket every $20,000. Documents are then sorted into buckets.

For our dashboard, we want a bar chart of car sale prices, but we also want to know the top-selling make per price range. This is easily accomplished using a terms bucket nested inside the histogram:

GET /cars/transactions/_search?search_type=count
{
   "aggs":{
      "price":{
         "histogram":{
            "field":"price",    
            "interval":20000    
         },
         "aggs":{
            "make":{
               "terms":{
                  "field":"make",   
                  "size":1
               }
            }
         }
      }
   }
}
VIEW IN SENSE
 

The histogram bucket requires two parameters: a numeric field, and an interval that defines the bucket size.



A terms bucket is nested inside each price range, which will show us the top make per price range.

As you can see, our query is built around the price aggregation, which contains a histogram bucket. This bucket requires a numeric field to calculate buckets on, and an interval size. The interval defines how "wide" each bucket is. An interval of 20000 means we will have the ranges [0-20000, 20000-40000, ...].

Next, we define a nested bucket inside the histogram. This is a terms bucket over the make field. There is also a new size parameter, which defines the number of terms we want to generate. A size of 1 means we want only the top make for each price range (the make that has the highest doc count).

And here is the response (truncated):

{
...
   "aggregations": {
      "price": {
         "buckets": [
            {
               "key": 0,
               "doc_count": 3,
               "make": {
                  "buckets": [
                     {
                        "key": "honda",
                        "doc_count": 1
                     }
                  ]
               }
            },
            {
               "key": 20000,
               "doc_count": 4,
               "make": {
                  "buckets": [
                     {
                        "key": "ford",
                        "doc_count": 2
                     }
                  ]
               }
            },
...
}
The response is fairly self-explanatory, but it should be noted that the histogram keys correspond to the lower boundary of the interval. The key 0 means 0-20,000, the key 20000 means 20,000-40,000, and so forth.

Graphically, you could represent the preceding data in the histogram shown in Figure 35, “Histogram of top makes per price range”:

Figure 35. Histogram of top makes per price range

Histogram of top makes per price range

Of course, you can build bar charts with any aggregation that emits categories and statistics, not just the histogram bucket. Let’s build a bar chart of popular makes, and their average price, and then calculate the standard error to add error bars on our chart. This will use the terms bucket and an extended_stats metric:

GET /cars/transactions/_search?search_type=count
{
  "aggs": {
    "makes": {
      "terms": {
        "field": "make",
        "size": 10
      },
      "aggs": {
        "stats": {
          "extended_stats": {
            "field": "price"
          }
        }
      }
    }
  }
}
This will return a list of makes (sorted by popularity) and a variety of statistics about each. In particular, we are interested in stats.avg, stats.count, and stats.std_deviation. Using this information, we can calculate the standard error:

std_err = std_deviation / count
This will allow us to build a chart like Figure 36, “Barchart of average price per make, with error bars”:

Figure 36. Barchart of average price per make, with error bars

Barchart of average price per make, with error bars

«  available buckets and metrics     looking at time  »
