---
layout: doc_page
---
[Next](/docs/KMVsketch2.html)

#The KMV Sketch, Step 1
To explain how a simple sketch works, let us start with the well-known <i>k Minimum Value</i> or <i>KMV</i> sketch. 

Our objectives are as follows:

* Estimate the number of unique identifiers in the entire Big Data in a single pass through all the data.
* Retain no more then <i>k</i> values in the sketch at any one time.

In the diagram on the left we have a source of data, Big Data, that could be stored data or a live stream of data.  We can imagine that this data consists of many millions of events or rows of data, where each event could consist of many columns of information about the events:  dimensional information, various counters or metrics, and identifiers that uniquely identify a device or user that was the source of the event. There are possibly millions of different identifiers and the same identifiers can appear many times throughout the data. For our purposes here, we will only focus on the identifiers.    

In the middle of the diagram we have a hash function whose job is to transform each identifier encountered into a pseudo-random fractional value between zero and one.

On the right we have a cache that maintains on ordered list of the hash values retained by the sketch. To the right of this cache we will list the rules that the sketch must follow to achieve our objectives.

<img class="ds-img" src="/docs/img/KMV1.png" alt="KMV1" />

[Next](/docs/KMVsketch2.html)