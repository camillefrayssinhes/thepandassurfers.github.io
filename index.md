---
layout: home
---

<a href="https://en.wikipedia.org/wiki/Apple_Inc.">Apple</a> has been a market leader in the world of technology ever since the launch of its first product. Furthermore, media are more and more being used to study their impact on stock market movements. We aim to show that the rises and falls in stock prices of Apple correlate to the extent that people are talking about Apple in the media, the fame of the speakers and the way they are talking about the company.

In the following, we want to investigate the reciprocal relationships between the fluctuations of the Apple stock market and media coverage related to this company. We explore the Quotebank data set and filter out any quotes that are not related to the company, the products or its direction board. We then combine it with informations on Apple stock market, collected with the Yahoo Finance API.

What is the role of the media coverage in explaining stock market fluctuations ? Firstly, we will ask what is the influence of the people's opinions about Apple expressed in the media on the stock market ? Then, we will examine who are the individuals who have influence over potential customers. Do these influencers have an impact on the company image and eventually, on the stock market ? The aim is to obtain insights on the interplay between the media coverage and the Apple stock market fluctuations.

### Why Apple ?
{% include world_map_apple_stores.html %}

Apple is an American multinational technology company that specializes in consumer electronics, computer software and online services. Apple is the largest information technology company by revenue (totaling $274.5 billion in 2020) and, since January 2021, the world's most valuable company. It is one of the Big Five American information technology companies, alongside Amazon, Google, Facebook, and Microsoft. Today, it is widely spread all over the world, and Apple possesses stores around the world as can be seen below. If you zoom in you might even find the original Apple store at Tysons Corner in McLean, Virginia, USA ! 


### Where Are We Heading ?

In order to provide the reader with a rough outline, we begin the story with a <a href='#PreCurs'>precursory look</a> into the data we have collected.
This is accompanied by a look at the question <a href='#coverage'>"How does the media coverage of Apple evolve over time and how does it influence the Apple stock?"</a>.
Following the initial overview, we develop our understanding of the data by analyzing the correlation between the stock market movements and the <a href='#sentiments'>sentiments expressed</a> in the quotes.
Then we will go deeper by <a href='#fame'>giving a weight to the speakers</a> according to the Wikipedia page view statistics per year. This allows us to classify them as having a significant influence or a low influence over potential customers.


### Why Is This Important ? 

Accurately predicting the stock markets is a complex task as there are millions of events and pre-conditions for a particular stock to move in a particular direction. 
Like most other industries, the financial industry communicates by sharing information and data through the media. People speaking in the media express their emotions and opinions. They also consume media, and in the process are influenced by the sentiments, feelings, and opinions expressed by others. Scientific studies show that people are often influenced by the data they consume, and that their decisions or actions are partly aligned with it. 
Hence, what if the media would have an impact on the stock market ? For instance, what if a rise in the number of pessimistic words in the market column of a journal would foreshadow a <a href="https://archive.canadianbusiness.com/blogs-and-comment/medias-influence-on-stock-market/market">market downturn</a> the next day ? 


### What the Data Says ?

The original <a href="https://dl.acm.org/doi/10.1145/3437963.3441760"> Quotebank data set </a> consists of 178 million speaker-attributed quotations that were extracted from 196 million English news articles crawled from over 377 thousand web domains between August 2008 and April 2020. This **extensive** data set does not enable us to directly investigate the reciprocal relationships between the fluctuations of the Apple stock market and media coverage related to this company. Instead, in order to have a great preview of **Apple mentions** in the media, we filter out any quotes that are not related to the company, the products or its direction board. We will then analyse to what extent Apple is mentioned according to time in the media, who are the speakers talking about this company and the way they are talking about it.
 
The finance data we process is provided by the <a href="https://www.yahoofinanceapi.com/"> Yahoo Finance API </a> and allows us to recover informations about the Apple stock market since 2008. This API provides quick and easy access to finance metrics for any stock or index. Among the many financial metrics available we decided to focus on the daily stock price and volume. The former will be an indicator of the long term health of the stock, and the latter of the daily volatility it may experience.

We study the liquidity traded for the Apple stock (*$AAPL*) between 2015 and 2019. Market liquidity depends on how much interest the public has in the particular asset in question. For example, Apple’s stock is much more liquid than the average tech company’s shares. The reason is that the business is well-known, with solid fundamentals, so investors often have a greater interest in it. In the graph below, the liquidity traded is displayed in blue (regular), and the top 2% of days per year for which the liquidity traded is maximal is displayed in red (volatile). We observe the same trend for the top 2% of days for maximal liquidity traded over the years. It seems that 4 to 6 times a year, a higher liquidity is traded and this is likely to correspond to Apple events in which the company announces release and information on each new product before it hits the market. This allows the consumer to build a demand for the product before it ever hits the shelf. 

{% include liquidity.html %}

We then study the daily stock price for the Apple stock between 2015 and 2019. The stock price is steadily increasing since 2016. However, we notice some drops, particularly in January 2019 and in July 2019. We will try to provide interesting explanations for these drops using the media coverage of Apple in later analyses. (REF À METTRE)

{% include stock_price.html %}

<a id='PreCurs'></a>

## How does the media coverage of Apple evolve over time ?

Thousands of events occur around the world every day, and humans notice only a small subset of these events. Yet, the media attract attention to specific events. In this regard, a company’s media visibility will definitely affect its stock price. Are there any specific patterns we can observe across the years, for instance bigger media coverage when there is the keynote ? 


Let's investigate the relationships between the fluctuations of the stock market of Apple and the media coverage related to this company for a period of 5 years (2015–2019).

We have a look at the daily number of quotes related to Apple between 2015 and 2019. Here again, the daily number of quotes is displayed in blue, and the top 2% of the days for which the number of quotes related to Apple is maximal is displayed in red. In the same idea, similar trends are observed top 2% of days for maximal quotations of Apple over the years.
We can already make a few interesting observations.
For instance, at the beginning of 2016, we observe a significant peak of quotations. What happened at this date ? Apple has become embroiled in a very <a href="https://www.nasdaq.com/articles/will-apple-inc.-aapl-stock-feel-a-bite-from-the-fbi-2016-02-23"> public battle with the FBI </a> over whether or not the tech firm should unlock an iPhone used by one of the shooters in the San Bernardino terror attacks. The media had a field day ! This debate could have gone either way for the Apple stock. On the one hand, the media could have highlighted Apple's refusal to design a program to unlock iPhones underscores the company's hardline against violating customer privacy, and this likely gave customers another reason to choose Apple products. On the other hand, the media could have shifted the narrative and painted Apple in a negative light for obstructing an investigation into a terror plot, and this could have severely affected the Apple stock.



<a id='coverage'></a>

### What Can We Learn From the Data ? 

A question sprang to our attention during the analysis of this above plot and of the one displaying the liquidity traded for the Apple stock. Is the liquidity traded correlated to the quotations related to Apple ? We start with some precursory data visualization and statistical analysis to provide a feeling for the data we are working with and to present some first insights. Combining the former observations, we can look into the evolution of the Apple stock price in a joint relation of the number of quotations related to Apple in the media.
For instance, we observe a stock price drop at the beginning of 2016 corresponding to the battle with the FBI, illustrating that Apple has more suffered from this debate rather than gained confidence from its customers.
...

{% include daily_quotes_related_Apple_stock.html %}

We can conjoin this information with further analyses regarding the correlation between the number of quotations related to Apple and the liquidity traded for the Apple stock. We apply <a href="https://en.wikipedia.org/wiki/Pearson_correlation_coefficient">Pearson correlation </a> to identify if the correlation is statistically significant. The <b>Pearson correlation coefficient is approximately 0.3</b> and the p-value is very small (p < 0.05). Hence, there is a <b>non-negligible positive correlation</b>. In order to go deeper, we can divide the quotations into different categories according to the sentiments expressed (e.g. positive and negative). We would expect to see a stronger correlation coefficient between the number of positive quotations related to Apple and the liquidity traded for the Apple stock, and conversely a weaker correlation coefficient between the number of negative quotations related to Apple and the liquidity traded. 




