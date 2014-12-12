
elasticsearch: the definitive guide » aggregations » looking at time
«  building bar charts     returning empty buckets  »
looking at timeedit

returning empty buckets
extended example
the sky’s the limit
If search is the most popular activity in Elasticsearch, building date histograms must be the second most popular. Why would you want to use a date histogram?

Imagine your data has a timestamp. It doesn’t matter what the data is—Apache log events, stock buy/sell transaction dates, baseball game times—anything with a timestamp can benefit from the date histogram. When you have a timestamp, you often want to build metrics that are expressed over time:

How many cars sold each month this year?
What was the price of this stock for the last 12 hours?
What was the average latency of our website every hour in the last week?
While regular histograms are often represented as bar charts, date histograms tend to be converted into line graphs representing time series. Many companies use Elasticsearch solely for analytics over time series data. The date_histogram bucket is their bread and butter.

The date_histogram bucket works similarly to the regular histogram. Rather than building buckets based on a numeric field representing numeric ranges, it builds buckets based on time ranges. Each bucket is therefore defined as a certain calendar size (for example, 1 month or 2.5 days).

Can a Regular Histogram Work with Dates?

Technically, yes. A regular histogram bucket will work with dates. However, it is not calendar-aware. With the date_histogram, you can specify intervals such as 1 month, which knows that February is shorter than December. The date_histogram also has the advantage of being able to work with time zones, which allows you to customize graphs to the time zone of the user, not the server.

The regular histogram will interpret dates as numbers, which means you must specify intervals in terms of milliseconds. And the aggregation doesn’t know about calendar intervals, which makes it largely useless for dates.

Our first example will build a simple line chart to answer this question: how many cars were sold each month?

GET /cars/transactions/_search?search_type=count
{
   "aggs": {
      "sales": {
         "date_histogram": {
            "field": "sold",
            "interval": "month", 
            "format": "yyyy-MM-dd" 
         }
      }
   }
}
VIEW IN SENSE


The interval is requested in calendar terminology (for example, one month per bucket).



We provide a date format so that bucket keys are pretty.

Our query has a single aggregation, which builds a bucket per month. This will give us the number of cars sold in each month. An additional format" parameter is provided so that the buckets have "pretty" keys. Internally, dates are simply represented as a numeric value. This tends to make UI designers grumpy, however, so a prettier format can be specified using common date formatting.

The response is both expected and a little surprising (see if you can spot the "surprise"):

{
   ...
   "aggregations": {
      "sales": {
         "buckets": [
            {
               "key_as_string": "2014-01-01",
               "key": 1388534400000,
               "doc_count": 1
            },
            {
               "key_as_string": "2014-02-01",
               "key": 1391212800000,
               "doc_count": 1
            },
            {
               "key_as_string": "2014-05-01",
               "key": 1398902400000,
               "doc_count": 1
            },
            {
               "key_as_string": "2014-07-01",
               "key": 1404172800000,
               "doc_count": 1
            },
            {
               "key_as_string": "2014-08-01",
               "key": 1406851200000,
               "doc_count": 1
            },
            {
               "key_as_string": "2014-10-01",
               "key": 1412121600000,
               "doc_count": 1
            },
            {
               "key_as_string": "2014-11-01",
               "key": 1414800000000,
               "doc_count": 2
            }
         ]
...
}
The aggregation is represented in full. As you can see, we have buckets which represent months, a count of docs in each month, and our pretty "key_as_string".

«  building bar charts     returning empty buckets  »
