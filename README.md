# CREDBANK-data
Data to accompany the ICWSM 2015 paper "CREDBANK: A Large-scale Social Media Corpus With Associated Credibility Annotations"


There are two main files associated with credibility annotations:

 * [cred_event_TurkRatings.data](https://s3-us-west-2.amazonaws.com/credbank/cred_event_TurkRatings.data) - credibility ratings and reasonings entered by turkers for each topic. Each row has following tab separated fields:
 
 ```
topic_key       topic_terms     Ratings_list    Reasons_list
```
 * [cred_event_SearhTweets.data](https://s3-us-west-2.amazonaws.com/credbank/cred_event_SearchTweets.data) - tweets corresponding to each topic fetched using the search API
 ```
 topic_key       topic_terms     tweet_count     (tweetid,author,createdAt)tuple_list
 ```
 
 Supplementary file:
 * [event_nonEvent annotations](https://s3-us-west-2.amazonaws.com/credbank/event_nonEvent_22Jan_26Feb.data)
