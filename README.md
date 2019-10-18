
# Predict a Movie's Revenue

How can past data from The Movie Database (TMDB) help us build a predictive model of a movie's chances for success? What are the best features helping to know how profitable a movie will be? Is it always true that high budget movies have the greatest chances for yielding the highest revenue?  

In this project, the data used for this analysis is from a past Kaggle competition [here](https://www.kaggle.com/c/tmdb-box-office-prediction/data). Kaggle provides two main data files in csv format, a training and a test set. The target variable is a movie's revenue, which is omitted from the test set. After Exploratory Data Analysis (EDA), modeling will be done using two or more (as time allows) gradient boosting techniques. The end goal is a predictive model that tests well on the given testing data.  

## The Data 

Kaggle's Playground Prediction Competition "TMDB Box Office Prediction" closed five month's ago, but the data is still publicly available. Three additional datasets and modeling techniques were gleaned from [here](https://www.kaggle.com/zero92/tmdb-prediction/data).  The data represents 3000 movies in the training set and 4398 in the test set. In addition to the train and test data, there were additional features and release dates added .

## Exploratory Data Analysis(EDA)
Many samples had a budget of under $1000. Other sources with more accurate data ere sought and imputed. 

## An Early Model

As a "quick look," an OLS from statsmodels was used using 'budget' and 'runtime' features, and as expected, it showed that 'budget' had greater predictivity, but the R-squared was only 0.546. The conclusion was that further feature engineering was necessary in order to build a more predictive model.

##Feature Additions & Engineering

Three new data files were concatenated to the training set, and feature engineering was helpful in producing more features, including: genres, production companies, production countries, languages, keywords, cast and films. 

## Light Gradient Boosting Model

The LGBM was chosen. Comparisons on this data were made during the Kaggle competition with CatBoost and XGBModel and LGBM had similar but slightly better results. Should more features be added, LGBM has the advantage of being faster than the aforementioned when the number of parameters grows in size, at least theoretically. In the tests, the speed was similar for all three techniques.

KFold Validation showed an average RMSE score of 1.884. In other words the model did very well on the test data. Prominent feature importance was, in descending order: budget, vote_count, popularity, release year. Additionally, films made in Australia made better money than others. Although it had a relatively small feature importance, comedy genre films made more money than other genre films.

## Conclusions

Early and careful data analysis was really important. Finding such a high percentage of films with wrong data in the budget feature was an important catch. That is, simply looking for null values wouldn't have sufficed, but a bit of domain common knowledge that a movie is never free to make led to seeking and imputing correct budget information. An iterative approach, including doing a "quick look" model was a good way of looking at the big picture. Even all the feature engineering led to finding that budget was the most important predictive factor.

Further work can/should include continued efforts to include data, particularly text based studies like NLP/ML on pre-release press briefings and maybe commercials and their audience demographics. Perhaps other modeling techniques will be studied, too.

##Acknowledgements

Much thanks to Kaggle competitor Baula Hanna, of the Université Denis Diderot in Paris who's feature engineering this project would have been much smaller in scope and accomplishment. His Kaggle site is [here](https://www.kaggle.com/zero92/tmdb-prediction/data). Please "upvote" him as much as possible, he deserves it.

