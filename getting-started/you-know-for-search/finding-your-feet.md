
elasticsearch: the definitive guide » getting started » you know, for search… » finding your feet
«  document oriented     indexing employee documents  »
finding your feetedit

installing elasticsearch
running elasticsearch
talking to elasticsearch
document oriented
finding your feet
indexing employee documents
retrieving a document
search lite
search with query dsl
more-complicated searches
full-text search
phrase search
highlighting our searches
analytics
tutorial conclusion
distributed nature
next steps
To give you a feel for what is possible in Elasticsearch and how easy it is to use, let’s start by walking through a simple tutorial that covers basic concepts such as indexing, search, and aggregations.

We’ll introduce some new terminology and basic concepts along the way, but it is OK if you don’t understand everything immediately. We’ll cover all the concepts introduced here in much greater depth throughout the rest of the book.

So, sit back and enjoy a whirlwind tour of what Elasticsearch is capable of.

let’s build an employee directoryedit

We happen to work for Megacorp, and as part of HR’s new "We love our drones!" initiative, we have been tasked with creating an employee directory. The directory is supposed to foster employer empathy and real-time, synergistic, dynamic collaboration, so it has a few business requirements:

Enable data to contain multi value tags, numbers, and full text.
Retrieve the full details of any employee.
Allow structured search, such as finding employees over the age of 30.
Allow simple full-text search and more-complex phrase searches.
Return highlighted search snippets from the text in the matching documents.
Enable management to build analytic dashboards over the data.
«  document oriented     indexing employee documents  »

