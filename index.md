
## What is the media’s influence on stock market? 

<a href="https://en.wikipedia.org/wiki/Apple_Inc.">Apple</a> has been a market leader in the world of technology ever since the launch of its first product. Furthermore, media are more and more being used to study their impact on stock market movements. We aim to show that the rises and falls in stock prices of Apple correlate to the extent that people are talking about Apple in the media, the fame of the speakers and the way they are talking about the company.

In the following, we want to investigate the reciprocal relationships between the fluctuations of the Apple stock market and media coverage related to this company. We explore the Quotebank data set and filter out any quotes that are not related to the company, the products or its direction board. We then combine it with informations on Apple stock market, collected with the Yahoo Finance API.

What is the role of the media coverage in explaining stock market fluctuations ? [Firstly, we will examine who are the individuals who have influence over potential customers. Do these influencers have an impact on the company image and eventually, on the stock market ?] À MODIFIER. Then, we will ask what is the influence of the people's opinions about Apple expressed in the media on the stock market ? The aim is to obtain insights on the interplay between the media coverage and the Apple stock market fluctuations.

### Why Apple ?

<div class="h_iframe" align="center" >
    <iframe src="includes/world_map_apple_stores.html" frameborder="0" allowfullscreen style="width:100%; height:400px;overflow:auto;" ></iframe>
</div>

Apple is an American multinational technology company that specializes in consumer electronics, computer software and online services. Apple is the largest information technology company by revenue (totaling $274.5 billion in 2020) and, since January 2021, the world's most valuable company. It is one of the Big Five American information technology companies, alongside Amazon, Google, Facebook, and Microsoft. Today, it is widely spread all over the world, and Apple possesses stores around the world as can be seen below. If you zoom in you might even find the original Apple store at Tysons Corner in McLean, Virginia, USA ! 


### Where Are We Heading ?

In order to provide the reader with a rough outline, we begin the story with a <a href='#PreCurs'>precursory look</a> into the data we have collected.
This is accompanied by a look at the question <a href='#WhoIsTesco'>"How does the media coverage of Apple evolve over time ?"</a>.
Following the initial overview, we develop our understanding of the data by <a href='#Londoners'>clustering the speakers</a> we consider according to the Wikipedia page view statistics per year. This allows us to classify them as having a significant influence or a low influence over potential customers, which we use in later analyses. 
Then we will go deeper by analyzing the correlation between the stock market movements and the <a href='#Londoners'>sentiments expressed</a> in the quotes. 


### Why Is This Important ? 

Accurately predicting the stock markets is a complex task as there are millions of events and pre-conditions for a particular stock to move in a particular direction. 
What if the media have an impact on the stock market ? For instance, what if a rise in the number of pessimistic words in the market column of a journal would foreshadow a <a href="https://archive.canadianbusiness.com/blogs-and-comment/medias-influence-on-stock-market/market">market downturn</a> the next day ? 
Like most other industries, the financial industry communicates by sharing information and data through the media. People speaking in the media express their emotions and opinions. They also consume media, and in the process are influenced by the sentiments, feelings, and opinions expressed by others. Scientific studies show that people are often influenced by the data they consume, and that their decisions or actions are partly aligned with it. 

### What the Data Says ?

The original <a href="https://dl.acm.org/doi/10.1145/3437963.3441760"> Quotebank data set </a> consists of 178 million speaker-attributed quotations that were extracted from 196 million English news articles crawled from over 377 thousand web domains between August 2008 and April 2020. This **extensive** data set does not enable us to directly investigate the reciprocal relationships between the fluctuations of the Apple stock market and media coverage related to this company. Instead, in order to have a great preview of **Apple mentions** in the media, we filter out any quotes that are not related to the company, the products or its direction board. We will then analyse to what extent Apple is mentioned according to time in the media, who are the speakers talking about this company and the way they are talking about it.

The finance data we process is provided by the <a href="https://www.yahoofinanceapi.com/"> Yahoo Finance API </a> and allows us to recover informations about the Apple stock market since 2008. This API provides quick and easy access to finance metrics for any stock or index. Among the many financial metrics available we decided to focus on the daily stock price and volume. The former will be an indicator of the long term health of the stock, and the latter of the daily volatility it may experience.

We study the liquidity traded for the Apple stock (*$AAPL*) between 2015 and 2019. Market liquidity depends on how much interest the public has in the particular asset in question. For example, Apple’s stock is much more liquid than the average tech company’s shares. The reason is that the business is well-known, with solid fundamentals, so investors often have a greater interest in it. In the graph below, the liquidity traded is displayed in blue (regular), and the top 2% of days per year for which the liquidity traded is maximal is displayed in red (volatile). We observe the same trend for the top 2% of days for maximal liquidity traded over the years. It seems that 4 to 6 times a year, a higher liquidity is traded and this is likely to correspond to Apple events in which the company announces release and information on each new product before it hits the market. This allows the consumer to build a demand for the product before it ever hits the shelf. 

<div class="h_iframe" align="center" >
    <iframe src="includes/liquidity.html" frameborder="0" allowfullscreen style="width:100%; height:500px;overflow:auto;" ></iframe>
</div>

We then study the daily stock price for the Apple stock between 2015 and 2019. The stock price is steadily increasing since 2016. However, we notice some drops, particularly in January 2019 and in July 2019. We will try to provide interesting explanations for these drops using the media coverage of Apple in later analyses. (REF À METTRE)

<div class="h_iframe" align="center" >
    <iframe src="includes/stock_price.html" frameborder="0" allowfullscreen style="width:100%; height:500px;overflow:auto;" ></iframe>
</div>

Finally, we have a look at the daily number of quotes related to Apple between 2015 and 2019. Here again, the daily number of quotes is displayed in blue, and the top 2% of the days for which the number of quotes related to Apple is maximal is displayed in red. In the same idea, similar trends are observed top 2% of days for maximal quotations of Apple over the years.

<div class="h_iframe" align="center" >
    <iframe src="includes/daily_quotes.html" frameborder="0" allowfullscreen style="width:100%; height:500px;overflow:auto;" ></iframe>
</div>

A question sprang to our attention during the analysis of this above plot and of the one displaying the liquidity traded for the Apple stock. Are the top 2% of the days for which the liquidity traded is the highest correlated to the top 2% of days for which the quotations related to Apple are the highest ? 


<a id='PreCurs'></a>

### What We Can Learn From the Data ? 

We start with some precursory data visualization and statistical analysis to provide a feeling for the data we are working with and to present some first insights and explore the data a little.

### xxxxxxx

To answer this question we looked at an important piece of information that the researchers provided with the dataset: the relative representativeness that each area achieved.
Here, the *representativeness* of an area is defined as the ratio between the number of loyalty card-holding customers residing in it and its total number of inhabitants.
An area with low representativeness has few Tesco customers and thus prevents extrapolation, as the few Tesco customers would then speak for the majority of residents in an area.
*The similarity to the UKs first-past-the post election system is left uncommented by the Authors*.
The ratio was then normalised using a min-max-scaling to provide normalised representativeness, with the most representative area having a representativeness of 1 and the least representative area having a representativeness of 0.


Representativeness gives us a measure to analyse why an area has a high or low number of Tesco customers, or rather, loyalty card-holding inhabitants.
Looking at the Spearman correlation, a statistically significant correlation between representativeness and socio-economic factors can be observed for multiple groups, both positively and negatively:


Most of the correlations are rather weak.
Nevertheless, they can provide interesting explanations for the results initially obtained by the authors of the original paper.
There, they discovered a decreasing representativeness and number of transactions in areas of south of London.
An interesting tidbit to add to this is that this seems to be the case especially in areas closer to the Home Counties.
These areas are made up mostly of the old white middle class leaving the urban core of London in the wake of suburbanisation during the post-war years, which was later followed by middle-class immigrant communities.
A further hypothesis to be consider in this regard is the direction of this effect.
Put more plainly, do the inhabitants of these areas choose Tesco as their store, or does Tesco choose these inhabitants as their customers.
Sadly, without further information on Tescos modus operandi concerning store openings and their target customer base, we cannot easily answer this question.

Lastly, a fact that might at least bias the result is skewed data collection.
In the original data set, a higher number of Tesco stores from which the data was collected was situated north of the Thames.
This introduces a bias towards the more ethnically diverse areas of northern London and its urban core. However here the question might still stand, if this is a cause or an effect based on Tesco Modi operandi, or might even have another causes.

### xxxxxx


Today is the day the snake catches its tail. During the [1854 Cholera epidemic](https://en.wikipedia.org/wiki/1854_Broad_Street_cholera_outbreak) in London, the researcher John Snow applied techniques of data collection, [visualisation](https://upload.wikimedia.org/wikipedia/commons/thumb/2/27/Snow-cholera-map-1.jpg/1280px-Snow-cholera-map-1.jpg) and analysis to halt the spread of the virus.
Thanks to his research, he became both one of the precursors of modern epidemiologists and the avant-garde of data science.
Today, we feel honoured in that we can say that we are back at the beginning, *analysing water consumption in London during a pandemic*.


A question sprang to our attention during our analysis of the correlation between different socio-economic factors and product consumption.
Why does water consumption strongly correlate with area median income and different ethnicities?
Especially as in this case we are considering water purchased in supermarkets, not tap water.
This result is somehow counter-intuitive: One would expect people with low income to drink less bottled water because it is more expensive.
Early on, this led us to explore the interrelations of social factors and economic factors of an area.


A few working theories we developed during the analysis were:

Water purchased in supermarkets can be carbonated, tap water is not, a liking for the former could fall along different cultural and culinary lines.

For our investigation, we are missing a few crucial pieces of the puzzle.
One crucial flaw is that we only have records on consumption habits from supermarket purchases using loyalty cards.
Consequently, information about gastronomy consumption is missing and even small purchases might be missing.
This is also reflected in the consumption habits of Londoners.
The store locations from which the data was sourced are unevenly distributed over the Greater London area.
This could imply that heavy products such as water are more likely to be purchased by people living near a store.
This is especially possible in a dense city where many people rely on public transportation.
As the areas where many stores located are the northern parts and the urban core, this could skew the data towards ethnically diverse communities living in the area closer to a store.

Another idea is that water quality might differ between low and high-income areas, based on housing age and maintenance quality.
Sadly enough, water infrastructure, and by extension water quality, does not easily conform to census area boundaries.
*A [fact](https://www.ph.ucla.edu/epi/snow/watercompanies.html) known by John Snow*.
While digging deeper into this topic, we found that consumer's drinking water habits are a yet unsolved mystery.
A US [study](https://iwaponline.com/wp/article-abstract/19/1/1/20521/Mistrust-at-the-tap-Factors-contributing-to-public?redirectedFrom=fulltext) states that Hispanic, Black, and foreign-born inhabitants are more likely to think that tap water was unsafe.
Also, low-income households experienced more water insecurity and ethnic minorities were more likely to have negative previous experiences with tap water.
Another [study](https://onlinelibrary.wiley.com/doi/full/10.1111/coep.12088) finds that bottled water is often attributed to greater safety and better taste.
We must keep in mind that these studies are from the US, so who cares!?
London still has the [best water of them all](https://www.standard.co.uk/news/official-london-tap-water-is-the-best-in-britain-6835465.html) and there is no need to carry heavy packs of water around.
*What would John Snow say to this?*

While this is an interesting mystery, we are not able to find a definitive answer just now.
Nevertheless, we are encouraged to solve this rather enticing mystery.
*Stay hydrated*.

---

<a id='Londoners'></a>

### Giving the Child a Name

This approach only provides information about the similarity between areas, not direct labels for each grouping.
Thus we needed to conduct further analyses to identify good labels, representative of the population of the area.

As the weather in London is rather _Covidy_ around this time of the year and thus we can't validate our findings in person, we conducted our study of the inhabitants in the subgroup areas, based on information present in the data.
We then verified our conclusion through wider research on the areas.
For the grouped areas we identified labels based on the shared attributes we used for clustering the groups in the first place.

One example case was the groupings based on the education level of the population of the area, which resulted in the model reaching a high silhouette score for 3 clusters.
For these three clusters, we then analysed the area's median number of inhabitants with a certain education level tracked in the census.
For the first grouping, we discovered an especially high number of inhabitants having a Level 4 or higher education level, which in the UK represents people with at least a certificate of higher education.
For the second grouping, we can see that these include the areas with an exceptionally high student population.
This is further supported by the fact that these areas are those located directly around the major university campuses in London.
Analysing our ethnicity clustering, we could compare it with work conducted for the Wikimedia Foundation, and were indeed able to discover similar population distribution structures as in their [population plots](https://en.wikipedia.org/wiki/Ethnic_groups_in_London).

As a side-note, a clustering and labelling conducted for policy-makers in the Greater London area is available, which provides groupings based on a wider range of *joint*-attributes and deeper analysis of socio-economic trends in the UK.
For us it is sadly of little interest, as it only provides this information on an LSOA / OA level. However, it still provides some [interesting reading](https://data.london.gov.uk/dataset/london-area-classification) and further considerations for more refined clustering approaches on socio-economic data.

To address some pitfalls of our approach, the clustering is mainly conducted on distinct subsets of data, like ethnicity, religion, or household income, thus it neither addresses certain interplay.
For example, it fails to distinguish between a rich or poor minority community.
Another flaw is that Indian, Pakistani and Bangladeshi minorities, are grouped under a single joint *Asian* label, which additionally includes any South-East Asian and Central Asian minorities.
*The authors pride themselves in this approach, easily outclassing the traditional, tried and tested British method for clustering socio-economic groups, the pen and ruler*.

<a id='Distribution'></a>

An additional pitfall is that the approach leads to unbalanced label distribution, as certain distinct groups in society are less common and in some instances prevent extraction of subgroups altogether as they are far too dispersed over the whole set of areas and overshadowed by other groups.
For example, we are entirely missing areas with Middle Eastern, Sikh, or Jewish majorities.
The effects of these pitfalls can be seen later in the predictive models and will be addressed during the model learning stage.
To explore our clustering results on a cross-sectional basis for each area, you can use the interactive map below:

---

<a id='Ensemble'></a>



## Can We Go Back?

To further improve on the performance of our classifier, we might now go with an even more complex model or refine our clustering approach.
Nevertheless, in this instance we might not opt for this option, and instead ask ourselves if we can go back.

A major concern with complex models such as random forests and boosting trees is that, while they perform well and are robust, they present a high level of obfuscation to the derivation of their final result.
For a human, they appear as a near-to-*black-box* that transforms an input into an output, without granting the individual understanding of its decision mechanism.
Addressing this concern as *Responsible Data Science* has become an increasingly important issue.
This is especially relevant when working with sensitive data concerning individuals and when performing societal analyses.
To do our part, additionally to our previous analysis based on random forests, we tried to train models that provide a better explainability to their result and thus get at least a cautious look into the black box.

<a id='Trees'></a>

### But I still like Trees

We start with *decision trees*, starting from the top root node the data is split based on different features into smaller groups that then again can be split further.
This allows for great explainability of their decision process.
Unfortunately, this also necessitates the constraint that the decision tree does not increase its depth or general complexity too much.
In our model training, we again used balanced accuracy scoring, grid search, oversampling and class-weights to boost model performance and inter-class fairness.
On the other hand, this also leads to getting far more complex and nested trees in comparison to a tree trained on accuracy.
To address the issue of complexity and overfitting we used a pruning method to remove smaller and indecisive splits of the tree classifier.
The resulting decision trees were unsurprisingly still rather complex.
But they provide better insights regarding their decision structure, with special interest put on the most decisive cuts between labels.
Many of the trees we trained performed worse than the random forest models for the same features and targets, however in many instances not to an extensive amount.
For example, in a more extreme case, the decision tree pendant to our best performing random forest model achieves a balanced accuracy of 0.927 on the test set, while the random forest model achieves a balanced accuracy of 0.93, hence constituting only a marginal improvement.
The random forest however also achieves a higher base accuracy and a better F1-score.
*Let's look into the decision tree model*.

First off, we can see that the first and most defined split is based on sweets consumption, with many majority Christian areas having a lower fraction of consumption.
For the next split, something interesting can be observed.
Splitting based on fish consumption separates the Muslim and Hindu majority communities almost perfectly.
As already hypothesised earlier, we can observe that alcohol consumption indeed is a splitting feature used to divide between Christian and Muslim majority communities.
Furthermore, our hypothesis about higher dairy consumption finds at least some confirmation based on a split.
Overall, using a decision tree model allows for a better exploration of the data, as we can now see the impact of certain splitting features on which we might now base further analysis and exploration.

---

<a id='AssociationRules'></a>

### *Rule Britannia*

An alternative when looking for explainable models is the use of *association rules*.
These derive rules in the form **antecedents ➞ consequents**, based on observations of common item sets.
In this instance, we used the different labels of the groupings as items. Additionally, we added especially high and low consumption of certain products or proportions of a nutrient as further items.

We used different measures used in association rule mining to evaluate the rules that we derived through the former item set design. However, given the way that item set and association rules are designed, we reach certain conflicts in using metrics, as they are also once again susceptible to the unbalanced distribution of labels.

In our analysis, we focus on rules of the form **nutrients/products ➞ socio-economic descriptors**. This means that we sought out rules that let us derive the socio-economic implications of nutrient distribution.

Before looking at some of the rules we found, it is important to note a few things. Firstly, we found *a lot* of rules, more than 7,000 actually with confidence above 0.6, distributed across 21 consequent sets. This means that there were 21 groups, i.e. distinct subsets of socio-economic descriptors for which we found antecedents.

Instead of going through these rules by hand, which wouldn't have led to anything anyway, we tried to find sensible ways of reducing the number of rules we consider. Looking at the groups for which we had obtained rules, we noticed that some of them seemed to be very similar. Thus, we computed the set of all antecedents for every subgroup and then used *Jaccard similarity* to determine how close the set of antecedents were between groups. We then kept, for every subset of groups with pairwise Jaccard similarity greater than 0.8 the best representative, that is, the rules for the group with the highest lift.

For example, instead of having the groups {Working Class, Secondary School} and {Working Class}, we only kept the first one as their respective antecedent sets had a Jaccard similarity of 0.94. To further reduce the selection of rules, we required every rule to have a confidence of at least 0.65 and a lift greater than 1.8.

This had some unintended consequences. We found after filtering that we had, in the process, lost all rules with support higher than 0.1. We then further observed there was a pretty strong dichotomy between rules. As a rule of thumb, a rule either had high support (>0.1) and on the other hand a low lift (< 1.2), or it had a high lift (> 2) and low support (<0.07). This comes down to the distinction between making general, rather weak statements about large populations and making very precise statements about small groups. In the end, we opted for the second approach, as rules with low lift simply don't make very strong statements.

From the remaining rules, we selected a few by hand, as putting them all here would probably triple the length of the page at the very least.

These rules let us reinforce observations made beforehand.
For example, we can see that the working class tends to have increased spirits consumption while generally avoiding wine, or that the working class has a high sweets consumption.

Two very prominent rules on the Asian working class with extraordinarily high lift reinvigorate the hypothesis of lower meat consumption in that social group.

Sadly, the nature of the data did not allow us to obtain rules for other groups discussed above. For example, we did not find any rules about Hindu or Muslim minorities or concerning students, as these constitute small minorities compared to the other groups.
More surprisingly we did not find any rules concerning social strata other than the working class.
It is not surprising that we did not find rules for the upper class, as it makes up only a small percentage of society.
Nevertheless, we expected to find at least some rules for the two groups corresponding to the middle class.
A possible reason for why we didn't might be because these groups do not actually distinguish themselves that much through nutrient and product consumption.


### *Land's End*

Today we explored the interrelation of socio-economics and food consumption in the Greater London area.
While there were some obstacles in our way, we found some interesting tidbits and could make some interesting observations.
Sadly, not always did our approaches work and thus we tried to learn why they did not work out as planned.
Thus, this is a time for self-reflection and to learn what we could improve in future workd in exploring the interplay between socio-economics and food consumption.

#### To Improve the Recipe

After rounding up the analysis, we want to take a final look back and consider what can be done better and where we could extend further from the foundation we have built here.

As a next step, What we might consider is looking deeper into the data to find conjoint clustering and groupings based on a wider range of socio-economic attributes.
The most obvious example would be the differentiation between the major minorities in the UK, Indian, Pakistani, and Bangladeshi, which in our approach got clustered together based on the skewed singular ethnicity attribute.
An example to improve this would be based on adding majority religion to the clustering as further attributes.
Thus we could better differentiate between both groups.
However, this might also introduce further side-effects, which is why we kept it simpler for our initial analysis.
Additionally, we could take geo-distance into account, following the assumption that communities with a similar make-up are centralized into one or few clusters in a city.

Further efforts could be spent looking into data on a deeper granularity.
In our analysis, we focused on studying MSOA, as they provide high representativeness in the original Tesco data set, while also being more numerous than wards.
LSOA falls faster regarding representativeness, however could introduce new avenues to groupings, as smaller communities are not overshadowed by a larger area population.
Thus, this could allow to extend the original groupings and to address more socio-economic categories, single vs. [*DINK*](https://en.wikipedia.org/wiki/DINK) vs. family households, population density and dwelling type, social welfare recipients, age structure, which were sadly less differentiable on the MSOA level.
Concerning the models that we trained, more training data would especially help the random forest classifier.
While not solving the label imbalance, as it is to be expected with human populations, it could at least create more variety inside each category, which might be better than our approach of using synthetic data.

Last, but not least, concerning the model training we conducted, further refinement to the methodology we used could be applied to improve the model's performance.
For example, some consideration could be spent to use feature engineering, such as computing ratios between carbohydrates and sugars or fats and saturated fats.
However, training the best model possible was not our main goal, instead they helped us to better understand the data and to discover dividing lines with regard to food consumption.

<a id='EnglandFacts'></a>

### Cool England (and *Christmas*) Facts and Trivia:

The Royal Family of *Windsor*, because of its German ancestry and its pre-WW1 *von Sachsen-Coburg und Gotha* name, celebrate Christmas on Christmas Eve, the 24th of December, not on the morning of the 25th as proper Englishmen would do.
On a related note, the royal connection to the *House of Hannover* brought over from Germany the tradition of Christmas trees and introduced it to the British Isles.

Until the Betting and Gaming Act of 1960, the only sport allowed to be played on Christmas Day was archery.
This was due to the Unlawful Games Act of 1541 which aimed to promote the use of the longbow rather than less publicly useful sports, such as football... Pardon, *soccer*.

After the 31st of December, Queen Elizabeth II will be the only person in the UK not to need a passport to enter the EU legally, as all UK passports are issued in her name.
This is the same reason why she does not require a driver's license to drive on British roads, even though she earned one for driving [a lorry during the Blitz](https://i.insider.com/5e8646eb1378e3116b2372a4?width=1100&format=jpeg&auto=webp).

*On Metatrivia*.
The word trivia is derived from the Latin words *tri* and *via*, with the former meaning *three* and the later *road*.
The Romans, big into public infrastructure, built many roads. Places, where three - or more - of these roads met, were convenient places to make public announcements and thus making the announcement common, i.e *trivial*, knowledge.

<a id='Conclusion'></a>

### Anything else?

Although it might date the creation of this data story a little bit, we wish you all Happy Holidays and a hopefully better 2021.
Take an example from the Londoners and have a nice increase in your fat, sugar and alcohol consumption while you are at it, and see you in 2021.
*If you read this in 2021, don't forget to cancel the New Years resolution GYM membership before it's too late.*


