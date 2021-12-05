```python
# Useful starting lines
import numpy as np
import seaborn as sns
from dataloader import *
from finance import *
from plots import *
from finance import stock, compare
from quotebankexploration import *
from wikipedia import *
import vaderSentiment
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
%load_ext autoreload
%autoreload 2
```


```python
from task1 import *
from task2 import *
from task3 import *
```

***
## Quotebank Data Exploration


```python
quotes = load_quotes(2019, 'processed quotes')
quotebank_exploration(quotes)
```

    Let's see what the dataset looks like and what is it's shape.



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>quoteID</th>
      <th>quotation</th>
      <th>speaker</th>
      <th>qids</th>
      <th>date</th>
      <th>numOccurrences</th>
      <th>probas</th>
      <th>urls</th>
      <th>phase</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2019-01-17-000302</td>
      <td>[ O ] ne of the biggest challenges in protecti...</td>
      <td>Tim Cook</td>
      <td>[Q1404825, Q265852, Q7803347, Q7803348]</td>
      <td>2019-01-17 11:46:57</td>
      <td>1</td>
      <td>[[Tim Cook, 0.8848], [None, 0.0949], [Steve Jo...</td>
      <td>[https://www.cultofmac.com/601276/tim-cook-tak...</td>
      <td>E</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2019-02-28-002734</td>
      <td>A sudden shortfall in iPhone revenue is causin...</td>
      <td>None</td>
      <td>[]</td>
      <td>2019-02-28 14:29:32</td>
      <td>1</td>
      <td>[[None, 0.581], [Tim Cook, 0.419]]</td>
      <td>[http://www.ibtimes.com/apple-layoffs-190-cali...</td>
      <td>E</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2019-09-07-003380</td>
      <td>Apple tends to perform well when it changes th...</td>
      <td>None</td>
      <td>[]</td>
      <td>2019-09-07 17:19:15</td>
      <td>19</td>
      <td>[[None, 0.9333], [Chris Wiltz, 0.0667]]</td>
      <td>[http://www.wfmz.com/business/national-and-wor...</td>
      <td>E</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2019-04-02-008471</td>
      <td>As Hon Hai shares had lagged behind the broade...</td>
      <td>Alex Huang</td>
      <td>[Q4717204]</td>
      <td>2019-04-02 09:36:23</td>
      <td>1</td>
      <td>[[Alex Huang, 0.9346], [None, 0.0654]]</td>
      <td>[http://www.thestandard.com.hk/breaking-news.p...</td>
      <td>E</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2019-05-03-013085</td>
      <td>But we believe that our Mac revenue would have...</td>
      <td>Tim Cook</td>
      <td>[Q1404825, Q265852, Q7803347, Q7803348]</td>
      <td>2019-05-03 08:09:00</td>
      <td>2</td>
      <td>[[Tim Cook, 0.8447], [None, 0.1426], [Luca Mae...</td>
      <td>[https://www.laptopmag.com/articles/tim-cook-i...</td>
      <td>E</td>
    </tr>
  </tbody>
</table>
</div>



    (22694, 9)


The dataset comprises the following columns : 
 * `quoteID`: Primary key of the quotation (format: "YYYY-MM-DD-{increasing int:06d}").
 * `quotation`: Text of the longest encountered original form of the quotation.
 * `speaker`: Selected most likely speaker. This matches the first speaker entry in `probas`. If none of the speakers is selected, the speaker is defined as "None".
 * `qids`: Wikidata IDs of all aliases that match the selected speaker. If no Wikidata IDs is found, the value is '[]'.
 * `date`: Earliest occurrence date of any version of the quotation.
 * `numOccurrences`: Number of time this quotation occurs in the articles.
 * `probas`: Array representing the probabilities of each speaker having uttered the quotation.
 * `urls`: List of links to the original articles containing the quotation. 
 * `phase`: Corresponding phase of the data in which the quotation first occurred (A-E).

***
## Quotebank Data Pre-processing
We filter and transform the data according to our needs. The first step is to filter out any quotes that are not related to the company, the products or its direction board. To do so, we use a combination of white-listed words (Apple, iPhone, iPad, Macbook etc.), black-listed words that are not related to the Apple company (Big Apple, apple pie, big mac etc.) and relevant speakers (Tim Cook, Steve Jobs etc.). The filtered dataset is then saved as a pickle file.


```python
# Key-words for filtering

# One word key-words
keywords_1_word = ["iphone", "ipad", "imac", "ipod", "macbook", "mac", "airpods",
        "lightening", "magsafe", "aapl", "iwatch", "itunes", "homepod", "macos", "ios", "ipados",
        "watchos", "tvos", "wwdc", "siri", "facetime", "appstore", "icloud", "iphones"]

# Two words key-words
keywords_2_words = ["apple watch", "steve jobs", "tim cook", "face id",
        "pro display xdr", "katherine adams", "eddy cue", "craig federighi"]

# Keep only the 'Apple' that corresponds to the company
keywords_capital = ["Apple"]

# Remove all the words that can be confused with the words we are looking for
black_list = ["Big Apple"]

# Create a dictionnary of key-words
keywords = {"One word": keywords_1_word, "Two words": keywords_2_words, "Capital words": keywords_capital, "Black list": black_list}

# Speakers we want to look for quotes
speakers = ["steve jobs", "tim cook", "katherine adams", "eddy cue", "craig federighi", "john giannandrea", "greg joswiak",
    "sabih khan", "luca maestri", "deirdre o'brien", "johny srouji", "john ternus", "jeff williams", "lisa jakson",
    "stella low", "isabel ge mahe", "tor myhren", "adrian perica", "phil schiller", "arthur levinson", "james bell",
    "albert gore", "andrea jung", "monica lozano", "ronald sugar", "susan wagner"]
```


```python
# The idea of this section is to now apply the filter with the key-words
# we define before.

# Initialize the lists for the final plots
frequency_all = []
frequency_apple = []
years = np.arange(2008, 2021)

# Loop over all the years from 2008 to 2020
for year in years:
    # Filtering the quotes
    filt_quotes = filter_quotes(f"data/quotes-{str(year)}.json.bz2", \
        speakers = speakers, keywords = keywords, save = f"filtered_quotes_{str(year)}")
    
    # Update the list for the final plots
    frequency_all.append(filt_quotes['total'])
    frequency_apple.append(len(filt_quotes['dataframe']))
```

#### Distribution of the unprocessed and processed quotes among the years

Now we have filtered all the quotes in relation to Apple company, we want to know the distribution of the quotes among the years and how many quotes we deal with. The idea is also to see if there is a particular year where we have significantly more or less quotes than the others. Then, in the following figure we have plotted the total number of quotes and the number of quotes we have after filtering for every year from `2008` to `2020`.

If we look at the graph, we see that all years seem to have about the same number of citations (filtered or not), except for the two years `2008` and `2020`, where fewer citations are recorded. We could look at the percentage of Apple citations among all citations over the years to see if there is a noticeable change. [not done yet]


```python
# Plot the results
bar_plots_quotes(frequency_all, frequency_apple, years)
```

### EDA after filtering

This new filtered dataset provides a great preview of Apple mentions in the media. Let's look more precisely in this dataset :


```python
year = 2019
quote_df = load_quotes(year,category= 'processed quotes')
quotebank_exploration(quote_df)
```

We noticed that they were some quotes that were still irrelevant due to the keyword "mac" so we decided to filter again a little our dataset by removing couple of words as "big mac", "freddie mac" or "johnny mac".


```python
quote_df = refilter(quote_df)
```


```python
print("Total number of occurences from every quotes of each speaker for year %i" %year)
df_table = plot_table_numOcc(quote_df)
df_table
```


```python
plot_pie_numquote(quote_df, year, head_ = 5)
```


```python
plot_quotes_per_day(quote_df, year)
```


```python
plot_numOcc_per_day(quote_df, year)
```

***

# Wikipedia

For now, we have our filtered dataset in relation to Apple company and the speakers corresponding to each quote. Now, we want to know what is the "impact" of the speaker. We want to know how much this person is famous or/and has an impact on the others. For that, we will use Wikipedia that has almost a page for every famous person. So the idea now is to get the data set coming from the Wikipedia API, study it and see how we can use it for our problem.

For now, we will use the `.parquet` already given from the Wikipedia API and we will see later if we need more information than the already given ones. The first idea will be to group together all `.parquet` files in one single dataframe.


```python
# Get all the parquet files in a single dataframe
df_wiki = concat_wiki_files()
display(df_wiki.head(10))
```

This being done, we can see multiple features for each speaker. An interesting one for us is the `id` that will corresponds to the `qids` of the QuoteBank data set. The idea will be to keep only the speakers in the Wikipedia dataframe that are in our filtered quotes data set. Then, we will get all the speakers' ID that have spoken in our filtered quotes data set.


```python
# Here we group together all the filtered quotes in a single dataframe
df_filtered = get_filtered_quotes()
display(df_filtered.head(10))
```


```python
# Now we keep only the 'qids' and we remove duplicates
quotes_ID = get_wiki_ids(df_filtered)
```


```python
pd.DataFrame(quotes_ID)
```

Now, we have to find a way to get a correspondance between the `qids` of the quotes and the `id` of the Wikipedia data set. This will informs us if there is some speaker that are not in our Wikipedia data set, and then if we must find another way to obtain the informations we want.

[in progress...]

After that, we want to get all the snumber of views on the Wikipedia pages of the different speakers

***

# Yahoo Finance Data Exploration

The yfinance library provides financial metrics on most stocks and indices. It will allow us to quantitatively compare the stock price and volume of Apple to a major equity index, such that the S&P500. Later on we will use this new dataset to find and compare days of high volatility, and find any correlation with potential speakers and quotations.

A stock dataframe consists of the following features: 
 * `Date`: Primary key of the quotation (format: "YYYY-MM-DD").
 * `Open`: Stock price at the daily opening of the market.
 * `High`: Highest stock price reached during the day.
 * `Low`: Lowest stock price reached during the day.
 * `Adj Close`:  Adjusted stock price at the daily close of the market.
 * `Volume`: Amount of stocks that changed hands during the day.

 We will focus our study on the `Adjusted Open`, `Adjusted Close` and `Volume`.

## A first preview of the performance of $AAPL
As stated previously we will focus on the Apple stock performance, using the daily opening, daily close and volume metrics from the yfinance API. We load the stock metrics for the year 2019 and perform some first observation on this data.


```python
apple_ticker = "AAPL"
spy_ticker = "SPY"
year = 2019
apple_stock = yf.download(apple_ticker, start=f'{year}-01-01', end=f'{year}-12-31', progress = False)
apple_stock.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Adj Close</th>
      <th>Volume</th>
    </tr>
    <tr>
      <th>Date</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2018-12-31</th>
      <td>39.632500</td>
      <td>39.840000</td>
      <td>39.119999</td>
      <td>39.435001</td>
      <td>38.282604</td>
      <td>140014000</td>
    </tr>
    <tr>
      <th>2019-01-02</th>
      <td>38.722500</td>
      <td>39.712502</td>
      <td>38.557499</td>
      <td>39.480000</td>
      <td>38.326294</td>
      <td>148158800</td>
    </tr>
    <tr>
      <th>2019-01-03</th>
      <td>35.994999</td>
      <td>36.430000</td>
      <td>35.500000</td>
      <td>35.547501</td>
      <td>34.508709</td>
      <td>365248800</td>
    </tr>
    <tr>
      <th>2019-01-04</th>
      <td>36.132500</td>
      <td>37.137501</td>
      <td>35.950001</td>
      <td>37.064999</td>
      <td>35.981869</td>
      <td>234428400</td>
    </tr>
    <tr>
      <th>2019-01-07</th>
      <td>37.174999</td>
      <td>37.207500</td>
      <td>36.474998</td>
      <td>36.982498</td>
      <td>35.901768</td>
      <td>219111200</td>
    </tr>
  </tbody>
</table>
</div>



### Price and Volume
As stated previously, we can first look at the performance of the $AAPL stock in term of its stock price and daily volume. Empirically we observe that the days of sharp decreases or increase are often linked to day of high volume volatility. Later on we may look at the QuoteBank dataset if the days of high volatility also see many Apple related quotes.


```python
stock("AAPL", year = year, fig = "price_volume")
```


    
![png](output_files/output_30_0.png)
    


We can also observe with histograms the distribution of the daily volume of exchange of the Apple stock. From our observations it may be fitted appropriately as a left skewed distribution, and the average daily volume sits around 112 millions stocks trade daily. Later on we will focus on the days of high volatility, i.e with higher volume than usual.


```python
stock("AAPL", year = year, fig = "daily_diff")
```

### Daily price difference
We pursue this idea of studying the volatility as an indicator of meaningful trading days, by introducing the daily price indicator. By taking the difference between the daily closing and opening price, and dividing by the opening price, we obtain the percentage of increase or decrease of the stock during that day. We observe a gaussian-like distribution of the daily percentage change of the stock price, with a mean slightly above 0 at 0.1799%. We will later on compare this histogram of the daily price change with other major stocks and we will look more closely at the high change days.


```python
stock("AAPL", year = year, fig = "volume")
```

### Comparison with the overall market
The S&P 500 is regarded as one of the best gauge of large-cap U.S. equities. It is a weighted average of some of the 500 best and largest american companies, 6.2% of which is the Apple stock. It is often taken as a general indicator of the overall health of the US stock market, and individual stock may be compared to the S&P. In the following figure, we observe than during the 2019 trading period, the Apple stock generally underperformed the S&P but still ended up relatively with the same performance. Both are highly correlated.


```python
compare(spy_ticker, apple_ticker, year = year, fig = "price")
```

Looking at the volume of exchange of both SPY and AAPL, we observe again a strong correlation between the two. One of the reasons is that the day of high volume exchange are often close or during the week of the quarterly results. Most companies in the S&P 500 provide their quarterly results in the same time-frame.


```python
compare(spy_ticker, apple_ticker, year = year, fig = "volume")
```

Finally we can again take a closer look at the distribution of the daily price difference. Interestingly enough we observe that both stocks performs *in average* with a mean daily difference close to 0 with an normal distribution. However the Apple stock has a greater variance than the S&P, in other words the Apple stock is more suggest to volatility than the S&P. In a financial context, this is seen as being a riskier security with the same yielding. 


```python
compare(spy_ticker, apple_ticker, year = year, fig = "daily_diff")
```

***
### Subtask 1 : 
What is the role of the media coverage in explaining stock market fluctuations ? How does the media coverage of Apple evolve over time ? Are there any specific patterns we can observe across the years, for instance bigger coverage when there is the keynote ? Thousands of events occur around the world every day, and humans notice a small subset of these events. Yet, the media attract attention to specific events. In this regard, a company’s media visibility will definitely affect its stock price. We investigate the reciprocal relationships between the fluctuations of the stock market of Apple and media coverage related to this company for a period of 12 years (2008–2020). 


```python
task1(quotes)
```


    
![png](output_files/output_42_0.png)
    


    p-value : 3.448043392337313e-19
    98 1.2885458593230257e-20



    
![png](output_files/output_42_2.png)
    



    
![png](output_files/output_42_3.png)
    



```python
  stock_name = "AAPL"
  year = 2019
  # Find the days of high volatility 
  stock = yf.download(stock_name, start=f'{year}-01-01', end=f'{year}-12-31', progress = False)

  q1 = 0.98
  q2 = 0.98

  stock['Volatile'] = stock['Volume'] > np.quantile(stock['Volume'], q = q1)
  stock.reset_index(inplace=True)
```


```python

  ax1 = sns.set_style(style="white", rc=None )
  fig, ax1 = plt.subplots(figsize=(30,10))
#
  #sns.lineplot(data = stock['Open'], sort = False, ax=ax1)
 # ax2 = ax1.twinx()

  sns.barplot(y = stock['Volume'], x = stock['Date'],  alpha=0.3, ax=ax1, color="Green")
  sns.barplot(y = stock[stock.Volatile]['Volume'], x= stock['Date'], ax = ax1, color = 'Red')
  plt.title(f"Daily Volume for the ${stock_name} stock in {year}")

  ax1.xaxis.set_major_locator(matplotlib.dates.AutoDateLocator())
  x_dates = stock['Date'].dt.strftime('%Y-%m').sort_values().unique()
  ax1.set_xticklabels(labels = x_dates, rotation=45, ha='right')

  plt.show()
```


    
![png](output_files/output_44_0.png)
    



```python

daily_quotes = pd.DataFrame(quotes.groupby(quotes.date.dt.date).quotation.count())
daily_quotes['HighCount'] = daily_quotes['quotation'] > np.quantile(daily_quotes['quotation'], q = q2)
daily_quotes.index.rename('Date')
daily_quotes.reset_index(inplace=True)

ax1 = sns.set_style(style="white", rc=None )
fig, ax1 = plt.subplots(figsize=(12,6))
daily_quotes.date = pd.to_datetime(daily_quotes.date, format='%Y-%m-%d')
sns.barplot(x= daily_quotes['date'], y= daily_quotes['quotation'], ax = ax1, color='green')
sns.barplot(x= daily_quotes['date'], y= daily_quotes[daily_quotes.HighCount]['quotation'], ax = ax1, color='red')
ax1.xaxis.set_major_locator(matplotlib.dates.AutoDateLocator())
x_dates = daily_quotes['date'].dt.strftime('%Y-%m').sort_values().unique()
ax1.set_xticklabels(labels = x_dates, rotation=45, ha='right')

```




    [Text(0.0, 0, '2019-01'),
     Text(59.0, 0, '2019-02'),
     Text(120.0, 0, '2019-03'),
     Text(181.0, 0, '2019-04'),
     Text(243.0, 0, '2019-05'),
     Text(304.0, 0, '2019-06')]




    
![png](output_files/output_45_1.png)
    



```python
from statsmodels.tsa.seasonal import seasonal_decompose
from statsmodels.tsa.stattools import adfuller

analysis = stock.copy()
analysis.set_index('Date',inplace=True)
analysis = analysis['Open']
decompose_result_mult = seasonal_decompose(analysis, model="additive",period=4)

trend = decompose_result_mult.trend
seasonal = decompose_result_mult.seasonal
residual = decompose_result_mult.resid

p_value = adfuller(residual.dropna())[1]
print(f"p-value : {p_value}")

decompose_result_mult.plot();
```

    p-value : 3.320446266943836e-10



    
![png](output_files/output_46_1.png)
    



```python
best_value = 1
for period in range(1,125):
  analysis = stock.copy()
  analysis.set_index('Date',inplace=True)
  analysis = analysis['Open']
  decompose_result_mult = seasonal_decompose(analysis, model="additive",period=period)

  trend = decompose_result_mult.trend
  seasonal = decompose_result_mult.seasonal
  residual = decompose_result_mult.resid

  p_value = adfuller(residual.dropna())[1]

  if p_value < best_value:
    best_period = period
    best_value = p_value

print(best_period,best_value)
```

    3 4.337447869570744e-20


***
### Subtask 2 
Who are the individuals who have influence over potential customers, and do these influencers have an impact on the Apple company image and eventually, on the stock market ? Personality  plays  a huge  role  in  consumer  buying  behavior. Indeed, the high level of public attention and the positive emotional responses that define celebrity increase the economic opportunities available to a firm. We hypothesize that quotes from celebrities significantly impact the stock market, whereas quotes from ordinary people have no significant predictive power. One defining characteristic of a celebrity is that it is a social actor who attracts large-scale public attention : the greater the number of people who know of and pay attention to the actor, the greater the extent and value of that celebrity.


```python
task2()
```

***

### Subtask 3 :
What is the influence of the public opinions or emotions about Apple expressed in the media on the stock market ? Media influences beliefs by providing compelling information about events. In this regard, the media have been identified to play a significant role in shaping the consensus market opinion. Can a series of news articles make a stock rise? On the other hand, can it send a market into turmoil ? We hypothesize that emotions of the public in the media about a company would reflect in its stock price. Indeed, positive news about Apple would encourage people to invest in the stocks of the company. On the other hand, increases in expressions of anxiety, worry and fear would predict downward pressure on the stocks of the company. Understanding the author's opinion from a piece of text is the objective of sentiment analysis. We classify quotes as positive, negative and neutral based on the sentiment present. We want to answer the following questions : are the stock price drops/rises correlated with negative/positive quotes ? 


```python
task3()
```
