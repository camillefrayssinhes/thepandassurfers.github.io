### Can We Go Deeper?

## What is the influence of the public opinions or emotions about Apple expressed in the media on the stock market ?

Media influences beliefs by providing compelling information about events. In this regard, the media have been identified to play a significant role in shaping the consensus market opinion. Can a series of news articles make a stock rise? On the other hand, can it send a market into turmoil ? We hypothesize that emotions of the public in the media about a company would reflect in its stock price. Indeed, positive news about Apple would encourage people to invest in the stocks of the company. On the other hand, increases in expressions of anxiety, worry and fear would predict downward pressure on the stocks of the company. Understanding the author's opinion from a piece of text is the objective of sentiment analysis. We classify quotes as positive, negative and neutral based on the sentiment present. We want to answer the following questions : are the stock price drops/rises correlated with negative/positive quotes ?

After the basic analysis, the main goal we set out to achieve is training models to predict socio-economic properties of areas based on food consumption habits.
For this, we used the data set on the MSOA Level, as it has more areas with higher representativeness in comparison to the LSOA level, while also providing a higher granularity than the next higher granularity, Wards.
The latter would group multiple diverse areas, smoothing out interesting divergences.

We started with a regression analysis to predict continuous outcome values based on nutritional facts.
To improve the model's performance, we removed outliers.
While the general data quality was near perfect concerning completeness and missing values, we focused on identifying and removing extreme values regarding nutrient and product consumption using [*Local Outlier Fields*](https://www.dbs.ifi.lmu.de/Publikationen/Papers/LOF.pdf).

The resulting regression model was able to predict continuous variables such as median area income or the percentage of *BAME* (***B****lack*, ***A****sian* and ***M****inority* ***E****thnic*) inhabitants in an area with a high coefficient of determination (0.6).
We could also learn that most of the input variables have a significant impact on the prediction result and thus can derive that they have a predictive interplay with the target values.

---

### The *Sherwood* Forest

To better address the differences between the areas in the following, we opted to switch from a regression task to a classification task based on the clustering and labelling we conducted earlier.
This allows us to better capture the differences between the areas than a single continuous feature, which cannot easily represent multiple disjoint ethnic groups, religions, or education levels.

For the next classification task, we chose to train a *Random Forest Model*.
It incorporates an ensemble of deep decision trees trained on bootstrapped samples of the data with adapted feature selection to increase the variety between trees.
Each tree in the model represents a layered application of simple rules, which when applied split each set of areas into two subgroups.
These can then again be split based on another feature in the next layer of the tree.
Before we start the analysis, we need to address the pitfalls of the clustering which led to obvious unequal labelling.
For example, many more areas have a Christian or white majority in comparison to a Hindu majority or an Asian majority.

We address this issue by oversampling the minority labels to improve the model performance, as Random Forests are preferential towards the majority class.
We compared different oversampling techniques, such as random oversampling, which just entails drawing from the minority labels samples with replacement.
The disadvantage with this technique is that it only multiplies minority labels to put more weight behind them, it does not create new synthetic data.
More advanced approaches, such as [SMOTE](https://www.jair.org/index.php/jair/article/view/10302) and [ADASYN](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=4633969&casa_token=lmtht2WRwM4AAAAA:H5Mwc-PPkrfC6jvyhiWFGIX0zL5IVX5rpFgD55AHHGjbJlx5YSAK98Qc7aFttn-UZKGfm0xLBw&tag=1) have been developed, which create new synthetic minority label data samples based on interpolating between existing samples.
This approach helped to boost the performance for multiple models, although in some instance it was not enough to allow for successful model training.

To further handle the imbalance we choose to evaluate the model on how good it performs not by simply looking at how accurate it was overall, but by looking at its *balanced accuracy* for every subgroup.
As an easy example, a model that would need to classify areas according to ethnic composition could in the base case be accurate by simply being good in predicting white areas and could misclassifying all other minority areas.
As there are many white areas it would still predict mostly correct and have high accuracy, while having a far worse balanced accuracy.

#### The Good...

With our methodology, we were indeed able to train some models that were effective in predicting at least some of the socio-economic properties of an area.
Especially the model to predict majority religion in an area did perform well, achieving up to 93% balanced accuracy and over 95% general accuracy.
This high performance could be attributed to the high distinctiveness religion achieves in comparison to other groupings.
In other words, consumption habits between sub-denominations, such as Sunni and Shia Muslims, might still be mainly defined by the main religion, i.e *halal* or following *Vedic* beliefs.
On the other hand, this might not hold to a broader grouping done in the original census.
For example the census under the label Asian groups together very distinct subgroups such as Indians and Pakistani into a larger whole.
In the same vein, the Black census category groups together Jamaican and Central-African communities.

#### ... And the Bad

Sadly, not all of our models performed as well.
Especially the model predicting ethnic make-up and, to a lesser extent, the model predicting area income.
*This is, unfortunately, part of the reality of our approach to this kind of analysis, but we might still learn something from this*.
Reasons for the ill performance could be multi-faceted.
One obvious reason is our grouping and labelling methodology and the underlying data collection in the census.
We already introduced the caveat that the terms used in the census data about ethnicity are especially broad, which might prevent the model from learning distinctions between the major groups as they are themselves made up of multiple smaller and distinct subgroups.
Another facet might be that for wealth, simply addressing household median income cannot easily reflect more complex interplays regarding wealth, especially in a rather expensive city as London.
There, home-ownership can play a leading role in the disparity between people with similar household incomes.
Adding to this that policies such as *[Right to Buy](https://en.wikipedia.org/wiki/Right_to_Buy)* can further tip the scale by granting long-time residents an economic advantage over later arrivals and short-time renters.
Thus the grouping conducted might not capture enough information to form more distinctive groups regarding economics, that can be differentiated by the model.
Something to note about the performance of both of these models is that they generally still achieved a high accuracy, as they were able to distinguish between the majority class and the minority classes.
However, they performed rather badly at distinguishing the minority classes between each other as can be seen in the confusion matrix for the classifier predicting majority ethnicity.
We can see that it is both rather *precise* (middle) and *recall*s the majority of the white majority areas (right).
But it is bad at identifying ethnically diverse areas, which get misclassified across the complete spectrum.


*After addressing the training of the models, we also want find out some more about their inner workings and what are the most impactful facts they used in their predictions.*

### Learning What the Model Says

Interpreting random forest models regarding their decision process is hard, as they present ensembles of tens to hundreds of decision trees learned in a way to increase variety in the decision process of the individual trees.
*Talk about not to see the wood for the trees*.
Consequently, the individual trees are already complex, and over the size of the ensemble, it is nearly incomprehensible.
However, to at least get a feeling for the underlying process we can use *feature importance*, the contribution of individual features, to the final model.
Analysing this for our two most accurate models, predicting religion based on nutrients and based on product consumption, we can make a few interesting observations.
What we need to keep in mind when doing this is that based on the training of the individual trees, the occurrence of a feature is at least to an extent influenced by random chance.
This is due to the fact that only subsets of features are considered for each split.
Thus an important feature might just have bad luck in the lottery and gets chosen a less often than other features.
Hence, feature importance needs to be taken with a grain of salt.


For the model based on nutrients, we can see that especially nutrient diversity, protein, and salt consumption are often chosen splitting features in the classifiers.
Finding a rationale behind the importance of a feature can be difficult without knowledge of the way they split up the groups.
We might find some superficial explanations, such as vegetarianism being more prevalent in Hindu circles and thus leading to a lower protein, i.e meat consumption.
But finding in-depth explanations is very difficult.


Looking into which specific kinds of products have high importance regarding splitting presents a few more insights.
Especially fish, dairy, sweets, and wine consumption are highly important in identifying the religious majority in an area.
Once again, we might emit a hypothesis regarding a possible rationale behind their importances, i.e alcohol consumption is forbidden in conservative Islamic circles, milk products enjoy high approval in some Hindu circles, and so on.
While we can discover that these features are relevant in the final decision process, what we actually want to learn is *how* do they lead to the final decision we observe?
And especially, *In which way do they make groups distinguishable from each other?*

---

<a id='Responsible'></a>