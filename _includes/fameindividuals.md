<a id='fame'></a>

## Who are the individuals who have influence over potential customers, and do these influencers have an impact on the Apple company image and eventually, on the stock market ?


In the precedent section, we have looked at the influence of the author’s opinion from the different quotes of our data set on the stock market. But yet, we still miss an important piece of information. Indeed, if the previous president of the United States Donald Trump shares his opinion about Apple and if a random postman shares his opinion, it won't likely have the same effect. Thus: what is the impact of the speaker on the stock market? The response is relatively simple, it depends on how well known the speaker was at the time he or she was quoted in the media. And a quite simple way to have that indicators is the number of pageviews on then speakers' Wikipedia page (if there is one !).

The reason why we look at the fame of the speaker is that **personality plays a huge role in consumer buying behavior**. Indeed, the high level of public attention and the positive emotional responses that define celebrity increase the economic opportunities available to a firm. We hypothesize that quotes from celebrities significantly impact the stock market, whereas quotes from ordinary people have no significant predictive power. One defining characteristic of a celebrity is that it is a social actor who attracts large-scale public attention : the greater the number of people who know of and pay attention to the actor, the greater the extent and value of that celebrity.

As an important next step to explore the impact of the speaker fame on the stock market, we need to find a metric to identify how much a speaker is notorious. Are the different speakers poorly influential, moderately influential or highly influential? 

To achieve this, we use Wikipedia that has almost one page for every famous person. This allows us to give a **fame score** to the speakers based on their Wikipedia page view statistics. The fame score corresponds to the number of the speaker's Wikipedia page views at the year where the quote was published, normalized between 0 and 1.

Considering that we have only access to the Wikipedia page views since 2015, for this part we focus our analysis on the quotations between 2015 and 2020. 

### What is the impact of the quotations according to the speaker's notoriety on the stock market? 

Now, we have a fame score for each single quote. Let's see what is the impact of this fame score on the stock market? 

{% include stock_price_against_quotes_score.html %}