# CREDBANK-data
Data to accompany the ICWSM 2015 paper "CREDBANK: A Large-scale Social Media Corpus With Associated Credibility Annotations"

## Downloading the data
You can use <a href="https://aws.amazon.com/cli/">AWS CLI</a> or the easier <a href="http://s3tools.org/s3cmd">s3cmd</a> to download the data. First configure the tool to use your AWS account credentials and then use the appropriate command to download the s3 links listed below.

To download a data file (e.g., stream_tweets_byTimestamp.data) to the current folder, use the following command:
```
aws s3api get-object --request-payer requester --bucket credbank --key stream_tweets_byTimestamp.data stream_tweets_byTimestamp.data
```
OR
```
s3cmd get --requester-pays s3://credbank/stream_tweets_byTimestamp.data
```

Note that you will be charged a small data transfer fee by AWS, which can be offset by their <a href="https://aws.amazon.com/free/">free tier</a>.

## Data Description

The CREDBANK corpus was collected between mid October 2014 and end of February 2015.
It is a collection of streaming tweets tracked over this period, topics in this tweet stream, topics classified as events or non events, events annotated with credibility ratings. The data is spread across four files. The description of each file along with their location is listed below:

### 1. Streaming Tweet File

**S3 Location:** `s3://credbank/stream_tweets_byTimestamp.data`

This file contains more than 169 million streaming tweets spread across multiple timebins when these were collected. The collection period span from  10-Oct-2014 to 26-Feb-2015. Each line in the file corresponds to a timebin and contains 3 fields:

* ```timespan_key``` : Time span around which tweets were collected. For example, ```time_key = 20150114_044623-20150114_061420``` corresponds to streaming tweets collected around two time windows: 20150114_044623 and 20150114_061420, where
20150114_061420 is in the format: ```%Y%m%d_%H%M%S```. 
* ```tweet_count``` : Number of tweets in that time bin.
* ```ListOf_tweetid_author_createdAt_tuple``` : A list of tuples, where each tuple contains three fields: ```tweet ID``` , ```tweet author``` , ```tweet creation date```  

A snippet:
```
timespan_key		tweet_count		ListOf_tweetid_author_createdAt_tuple 
20150114_044623-20150114_061420	92255	[('555299882668670977', 'ZaE_BeAtZBsM808', 'Wed Jan 14 09:46:23 +0000 2015'), ('555299882698035203', 'gothebroncos', 'Wed Jan 14 09:46:23 +0000 2015'),....]
```

### 2. Topic File (Event/Non-Event)

**S3 Location:** `s3://credbank/eventNonEvent_annotations.data`

This file contains more than 62,000 topics. Each topic is represented by 3 terms, corresponding to the top three terms returned by running topic modeling (LDA) over the streaming tweets. It contains 3 fields:

* ```timespan_key```: same as in (1). Streaming Tweet File
* ```topic_terms``` : A list of 3 terms corresponding to the top 3 terms in each topic.
* ```isEvent```: A boolean value (0/1) to indicate whether the topic was rated as event or not. If a majority of Turkers (at least 6 out of 10) rated the topic as event, then isEvent = 1, else it is = 0.

A snippet:

```
time_key	topic_terms	isEvent
20141024_170629-20141024_181626 louis,ebola,nurse       1
20141024_170629-20141024_181626 retweet,gain,retweets   0
```


### 3. Credibility Annotation File

**S3 Location:** `s3://credbank/cred_event_TurkRatings.data`


This file contains more than 1300 events and their corresponding credibility ratings and reasons behind the ratings entered by Turkers. It contains 4 fields:

* ```topic_key``` : This is a combination of the time_key and topic_terms from the (2). Topic File. The entries in the Topic File which were marked as event (isEvent = 1) have credibility ratings in the credibility annotation file.
* ```topic_terms``` : A list of 3 terms corresponding to the top 3 terms in each topic.
* ```Cred_Ratings```: A list of 30 credibility ratings entered by Turkers. The ratings are based on a 5-point Likert scale ranging from [-2 to +2]:
    - [-2] Certainly Inaccurate
    - [-1] Probably Inaccurate
    -  [0] Uncertain (Doubtful)
    - [+1] Probably Accurate
    - [+2] Certainly Accurate
* ```Reasons```: A list of 30 reasons corresponding to the ratings entered by the Turkers.

A snippet:

```
topic_key	topic_terms	Cred_Ratings	Reasons
louis_ebola_nurse-20141024_170629-20141024_181626       [u'louis', u'ebola', u'nurse']  ['1', '-1', '2', '-2', '0', '2', '0',....]    ['Nurses union describes the procedures taken by nurse who now has Ebola from treating a patient., .....]
```

### 4. Searched Tweet File

**S3 Location:** `s3://credbank/cred_event_SearchTweets.data`

This file contains more than 80 million tweets spread across event topics. The tweets are fetched using the Twitter search API. The search query is constructed by taking a boolean AND over all 3 terms of a topic. Each line in the file contains 4 fields:

* ```topic_key``` : same as in (2). Credbility Annotation File
* ```topic_terms``` : same as in (2). Credbility Annotation File
* ```tweet_count```: Total number of tweets returned by the search API for the topic_terms.
* ```ListOf_tweetid_author_createdAt_tuple``` : A list of tuples, where each tuple contains three fields: ```tweet ID``` , ```tweet author``` , ```tweet creation date```  

A snippet:
```
topic_key	topic_terms	tweet_count	ListOf_tweetid_author_createdAt_tuple
louis_ebola_nurse-20141024_170629-20141024_181626       [u'louis', u'ebola', u'nurse']  13243 [('ID=522760301435830272', 'AUTHOR=iMhartyz', 'CreatedAt=2014-10-16 14:45:42'), ('ID=522760172872413184', 'AUTHOR=Tino_carter_v', 'CreatedAt=2014-10-16 14:45:12'), ......] 
```


### Citation Information
If you use the dataset in your research, please cite the following paper:
> Mitra, Tanushree, and Eric Gilbert. "CREDBANK: A Large-Scale Social Media Corpus with Associated Credibility Annotations." Ninth International AAAI Conference on Web and Social Media. 2015.
