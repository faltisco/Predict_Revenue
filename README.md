# Predict a Movie's Revenue

## 

How can past data from The Movie Database (TMDB) help us build a predictive model of a movie's chances for success ? What are the best features helping to know how profitable a movie will be? Is it always true that high budget movies have the greatest chances for yielding the highest revenue?  

In this project, the data used for this analysis is from a past Kaggle competition [here](https://www.kaggle.com/c/tmdb-box-office-prediction/data). Kaggle provides two main data files in csv format, a training and a test set. The target variable is a movie's revenue, which is omitted from the test set. After Exploratory Data Analysis (EDA), modeling will be done using two or more (as time allows) gradient boosting techniques. The end goal is a predictive model that tests well on the given testing data.  

## The Data 

Kaggle's Playground Prediction Competition "TMDB Box Office Prediction" closed five month's ago, but the data is still publicly available. Three additional datasets and modeling techniques were gleaned from [here](https://www.kaggle.com/zero92/tmdb-prediction/data).  The data represents 3000 movies in the training set and 4398 in the test set. In addition to the train and test data, there were additional features and release dates added from [here](https://www.kaggle.com/zero92/tmdb-prediction/data).

## Exploratory Data Analysis(EDA)

#### Domain Knowledge

In some ways, it is common knowledge to know about movies. The media often iterates over "the weekend box office" revenue as if it's a sports score. But what helps predict a movie's success is not fully understood, even by so-called experts. Many times a movie's budget is so high, the movie doesn't make a profit. But what does the data say? Is the budget predictive? Are there other interesting features like a film's shooting location telling? Future goal's should include NLP analysis of several of the features (eg. the movie's tagline, keywords, toverview and even the title), but that is beyond the scope at this point. However, EDA is important to give us a starting point, perhaps a "big picture" will enable the analysis to yield better results.
