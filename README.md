Anserini
========

Build using Maven:

```
mvn clean package appassembler:assemble
```

### Experiments on Gov2

Indexing:

```
sh target/appassembler/bin/IndexGov2 -input /path/to/gov2/ \
 -index lucene-index.gov2.cnt -threads 32 -optimize
```

The directory `/path/to/gov2/` should be the root directory of Gov2 collection, i.e., `ls /path/to/gov2/` should bring up a bunch of subdirectories, `GX000` to `GX272`.

After indexing is done, you should be able to peform a retrieval run:

```
sh target/appassembler/bin/SearchGov2 src/main/resources/topics-and-qrels/topics.701-750.txt \
 src/main/resources/topics-and-qrels/qrels.701-750.txt run.701-750.txt lucene-index.gov2.cnt/
```

A copy of `trec_eval` is included in `eval/`. Unpack and compile it. Then you can evaluate the runs:

```
eval/trec_eval.9.0/trec_eval src/main/resources/topics-and-qrels/qrels.701-750.txt run.701-750.txt
```


### Experiments on ClueWeb09 (Category B)

Indexing:

```
sh target/appassembler/bin/IndexClueWeb09b -input /path/to/cw09b/ \
  -index lucene-index.cw09b.cnt -threads 32 -optimize
```

The directory `/path/to/cw09b/` should be the root directory of ClueWeb09B collection, i.e., `ls /path/to/cw09b/` should bring up a bunch of subdirectories, `en0000` to `enwp03`.

After indexing is done, you should be able to perform a retrieval run:

```
sh target/appassembler/bin/SearchClueWeb09b src/main/resources/topics-and-qrels/topics.web.151-200.txt \
  run.web.151-200.txt lucene-index.cw09b.cnt
```

Then you can evaluate the runs:

```
trec_eval src/main/resources/topics-and-qrels/qrels.web.151-200.txt run.web.151-200.txt
```

To record search/running times:

```
sh target/appassembler/bin/Time lucene-index.cw09b.cnt
```

### Experiments on Tweets2011

Indexing:

```
sh target/appassembler/bin/IndexTweets -collection /path/to/tweets2011-collection/ \
  -index tweets2011-index -optimize -store
```

Running topics from TREC 2011 and 2012:

```
sh target/appassembler/bin/SearchTweets -index tweets2011-index/ -num_results 1000 \
 -queries src/main/resources/topics-and-qrels/topics.microblog2011.txt > run.ql.2011.txt

sh target/appassembler/bin/SearchTweets -index tweets2011-index/ -num_results 1000 \
 -queries src/main/resources/topics-and-qrels/topics.microblog2012.txt > run.ql.2012.txt
```

Add `-rm3` option for query expansion with relevance models (RM3).

For evaluation:

```
eval/trec_eval.9.0/trec_eval src/main/resources/topics-and-qrels/qrels.microblog2011.txt run.ql.2011.txt
eval/trec_eval.9.0/trec_eval src/main/resources/topics-and-qrels/qrels.microblog2012.txt run.ql.2012.txt
```

### Twitter (Near) Real-Time Search

To get access to the Twitter public stream, you need a developer account to obtain OAuth credentials. After creating an account on the Twitter developer site, you can obtain these credentials by [creating an "application"](https://dev.twitter.com/apps/new). After you've created an application, create an access token by clicking on the button "Create my access token".

To to run the Twitter (near) real-time search demo, you must save your Twitter API OAuth credentials in a file named `twitter4j.properties` in your current working directory. See [this page](http://twitter4j.org/en/configuration.html) for more information about Twitter4j configurations. The file should contain the following (replace the `**********` instances with your information):

```
oauth.consumerKey=**********
oauth.consumerSecret=**********
oauth.accessToken=**********
oauth.accessTokenSecret=**********
```

Once you've done that, fire up the demo with:

```
sh target/appassembler/bin/TweetSearcher -index twitter-index
```

The demo starts up an HTTP server on port `8080`, but this can be changed with the `-port` option. Query via a web browser at `http://localhost:8080/search?query=query`. Try `birthday`, as there are always birthdays being celebrated.
