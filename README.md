# Predicting_customers_ratings_on_Yelp_using_text_reviews

## Data Extraction from YELP
##### 1. Using YELP Fusion API
YELP has given developers access to download datasets using their API where-in:
- Business Search results using keywords like category, location and prices are available
- Business Details like name, address, phone number, photos, Yelp rating, price levels and hours of operation are available
- Upto 3 Customer Reviews for a business are available
- Detailed documentation which can be used to get data is [here](https://docs.developer.yelp.com/docs/fusion-intro)

##### 2. Using Existing Yelp Dataset
- For this project, I am using the already available dataset provided by [YELP](https://www.yelp.com/dataset/download)
- This 8.65GB datafile contains yelp_academic_dataset_business.json(113MB) and yelp_academic_dataset_review.json(4.97GB) which we will be using in this analysis

##### 3. Processing a HTML File
- We can also use Regular Expressions to extract data from HTML pages of the website. [Python Documentation](https://docs.python.org/3/howto/regex.html) provides more details to use 're' package

##### Python Notebook consists of a quick demo to extract "Top 10 Restaurants Name, Stars Received, Number of Reviews and $ Price Ratings" from a HTML webpage hosted [here](https://cse6040.gatech.edu/datasets/yelp-example)

## Model Training Notes

**1. Points to note while using Naïve Bayes:**
- Naïve Bayes assumes conditional independence between features given Y and therefore correlated attributes degrades its performance. 
- It can handle missing values in both training and test insances by using only the non-missing attributes while computing posterior probabilities
- It is robust to isolated noise points because such points are not able to significantly impact the conditional probability estimates
- It is robust to irrelevant attributes as if Xi is an irrelevant attribute, then P(Xi|Y) becomes almost uniformly distributed for every class y

**"Bayesian Networks"** can be used if we need to relax the rigid conditional independence assumption

**2. Points to note while using Logistic Regression:**
- Logistic Regression directly computes posterior probabilities without computing class conditional probabilities
- It can learn only linear decision boundaries, but can be extended to multiclass classification by training multiple models to learn each of the class labels individually
- It cannot handle missing values as posterior probabilites are calculated by taking a weighted sum of all attributes. Training instance with missing value have to be discarded, however if there are missing value in test data then logistic regression fails to predict its class label 
- It can handle irrelevant attributes by learning weight parameters close to zero for attributes that do not provide any gain in performance during model training. It can also handle interacting attributes since the learning of model parameters is achieved in a joint fashion by considering the effects of all attributes together.

**Assumptions:**
- There is a linear relationship between explanatory variables and the Logit of the Response Variable
- Observations in the dataset are independent of each other
- There is no multicollinearity among explanatory variables
- There are no extreme outliers

**3. Points to note while using Random Forest:**
- Random Forest is a bagging algorithm where-in different subsets of size N (same as entire training data) generated by random sampling with replacement of the training data, result in different trees
- One variation from a standard bagging algorithm is that random forest also uses feature randomness wherein each tree in a random forest can pick only from a random subset of features
- Bagging and Feature Randomness are applied when building each individual tree (which are not pruned), which create an uncorrelated forest of trees whose prediction by committee is more accurate than any individual tree

**Decision Tree**
- Decision Trees uses a greedy approach to grow by making locally optimal decisions about which attribute to use when partitioning the training data
- Entropy or Gini Index or Gain Ratio which takes into account the number of branches into which tree splits is used for splitting the data
- Decision Trees are a non-parametric approach which does not require any prior assumptions about the probability distribution of the attributes and also doesnt require any data transformations
- It cannot handle interacting variables(if they are able to distinguish between classes when used together, but individually they provide little or no information) and performs poorly due to the greedy nature of splitting criteria as such variables might be passed over 
- It can handle missing values, irrelevant attributes (which are not useful for the classification task) by passing over them as they will not provide gain in purity and redundant attributes (if it is strongly correlated with another attribute in the data) as only one of them will be selected as they provide similar gains in purity
- It can only learn linear decision boundaries - typically parallel to axis(x1<10 or x2>5), it can learn oblique decision boundaries(x1+x2<10) but they are computationally more expensive


**4. Points to note while using Support Vector Machine:**
- SVM learning is formulated as a convex optimization problem, wherein we can find the global minima whereas greedy algorithms like tree and neural networks tend to find locally optimal solutions
- SVM provides an effective way of regularizing the model parameters by maximizing the margin of the decision boundary. It is able to create a balance between model complexity and training errors by using a hyper-parameter 'alpha' in sk-learn
- It can handle irrelevant attributes by learning zero weights for such attributes. It can also handle redundant attributes by learning similar weights for the duplicate attributes.
- It cant handle missing data and data imputation is to be performed

## Comparison of Model Performance

