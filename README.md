# Predict a Movie's Revenue

## 

How can past data from The Movie Database (TMDB) help us build a predictive model of a movie's chances for success? What are the best features helping to know how profitable a movie will be? Is it always true that high budget movies have the greatest chances for yielding the highest revenue?  

In this project, the data used for this analysis is from a past Kaggle competition [here](https://www.kaggle.com/c/tmdb-box-office-prediction/data). Kaggle provides two main data files in csv format, a training and a test set. The target variable is a movie's revenue, which is omitted from the test set. After Exploratory Data Analysis (EDA), modeling will be done using two or more (as time allows) gradient boosting techniques. The end goal is a predictive model that tests well on the given testing data.  

## The Data 

Kaggle's Playground Prediction Competition "TMDB Box Office Prediction" closed five month's ago, but the data is still publicly available. Three additional datasets and modeling techniques were gleaned from [here](https://www.kaggle.com/zero92/tmdb-prediction/data).  The data represents 3000 movies in the training set and 4398 in the test set. In addition to the train and test data, there were additional features and release dates added from [here](https://www.kaggle.com/zero92/tmdb-prediction/data).

**Data Description**
**id** - Integer unique id of each movie

**belongs_to_collection** - Contains the TMDB Id, Name, Movie Poster and Backdrop URL  of a movie in JSON format. You can see the Poster and Backdrop Image like this: https://image.tmdb.org/t/p/original/<Poster_path_here>. Example: https://image.tmdb.org/t/p/original//iEhb00TGPucF0b4joM1ieyY026U.jpg

**budget**:Budget of a movie in dollars. 0 values mean unknown. 

**genres** : Contains all the Genres Name & TMDB Id in JSON Format

**homepage** - Contains the official homepage URL of a movie. Example: http://sonyclassics.com/whiplash/	, this is the homepage of Whiplash movie.

**imdb_id** - IMDB id of a movie (string). You can visit the IMDB Page like this: https://www.imdb.com/title/<imdb_id_here>

**original_language** - Two digit code of the original language, in which the movie was made. Like: en = English, fr = french. 

**original_title** - The original title of a movie. Title & Original title may differ, if the original title is not in English. 

**overview** - Brief description of the movie.

**popularity** -  Popularity of the movie in float. 

**poster_path** - Poster path of a movie. You can see the full image like this: https://image.tmdb.org/t/p/original/<Poster_path_here>

**production_companies** - All production company name and TMDB id in JSON format of a movie.

**production_countries** - Two digit code and full name of the production company in JSON format.

**release_date** - Release date of a movie in mm/dd/yy format.

**runtime** - Total runtime of a movie in minutes (Integer).

**spoken_languages** - Two digit code and full name of the spoken language. 

**status** - Is the movie released or rumored? 

**tagline** - Tagline of a movie 

**title** - English title of a movie

**Keywords** - TMDB Id and name of all the keywords in JSON format. 

**cast** - All cast TMDB id, name, character name, gender (1 = Female, 2 = Male) in JSON format

**crew** - Name, TMDB id, profile path of various kind of crew members job like Director, Writer, Art, Sound etc. 

**revenue** - Total revenue earned by a movie in dollars. 

## Exploratory Data Analysis(EDA)

#### Domain Knowledge

In some ways, it is common knowledge to know about movies. The media often iterates over "the weekend box office" revenue as if it's a sports score. But what helps predict a movie's success is not fully understood, even by so-called experts. Many times a movie's budget is so high, the movie doesn't make a profit. But what does the data say? Is the budget predictive? Are there other interesting features like a film's shooting location telling? Future goal's should include NLP analysis of several of the features (eg. the movie's tagline, keywords, toverview and even the title), but that is beyond the scope at this point. However, EDA is important to give us a starting point, perhaps a "big picture" will enable the analysis to yield better results.


<img src="./images/lc_time_series.png"/>



Post-IPO, Lending CLub's strongest month was Mar'16 when it originated $949.4M in loans on its platform. Since IPO, Lending Club has posted strong yearly loan origination numbers as seen below:
<p align="center">
  
|Year|Amount|
|:---|:-----|
|2015|$6.4B|
|2016|$6.4B|
|2017|$6.6B|
|2018|$7.9B|
|2019|$4.1B|

</p>

#### Understanding the Loan Mix

The loans on Lending Club are classified in 5 categories: Fully Paid, Current, Charged Off, Late (31-120 days), In Grace Period, Late (16-30 days), Does not meet the credit policy. Status:Fully Paid, Does not meet the credit policy. Status:Charged Off, Default. Below, we have grouped up the loan categories including late, in grace period and in default to show overall, what the Lending Club loan platform has originated.

<p align="left">
<img src="./images/lc_loan_status.png" width = 7500 />
</p>

As we can see here, ~85% of Lending Club loans fall under the Fully Paid or Current category, indicating that a sizable chunk of the loans available to investors are performing. To further break these loans down, we can analyze how Lending Club grades loans. 

Lending Club, at loan origination, gives a loan a grade of A to G and a subgrade of 1 to 5 indicating the financial rating of the borrower based on a proprietary model. The model outputs a number ranging from 1 - 25 which is then translated to a grade and a subgrade. For example, a model rank of 6 would correspond to B1. To dive further into how Lending Club loans perform by grade, we have the below plot:

<img src="./images/lc_grade_pie.png"/>

The exact $ (expressed in millions )values are as follows:
<p align="center">
  
| Grade| Principal Paid Off| Outstanding| Bad Debt|
|:-----|:------------------|:-----------|:--------|
| A    | $5,050.3M         | $2,440.4M  | $146.6M |
| B    | $7,196.7M         | $2,884.8M  | $553.5M |
| C    |$6,896.6M          |$2,801.4M   |$1,047.8M|
| D    |$3,399.7M          |$1,415.8M   |$838.5M  |
| E    |$1,518.0| $338.7M | $561.5M|
| F    |$469.9M|$79.4M|$250.6M|
| G    |$135.3M|$25.8M|$87.1M|

</p>

Speficially, we can see that Lending Club does a good job rating its loans, with each consecutive grade slightly producing more bad debt than the previous grade. 

#### Geographies

To get a sense of how Lending Club's loans are dispersed throughout the U.S., we can view the two below plots:
(Please note that the plots below are .png file representations of an interactive .html file. The html files are saved in the images repository and can be re-created using code found in the src repository)

<img src="./images/state_loan_originations.png"/>
<img src="./images/state_loan_count.png"/>

The two plots confirm what may have already been suspected. The largest loan volume is coming from CA, TX, NY, and FL. However, when we look average loan size, we find the following:
<p align="center">
  
|State| Avg Loan Size|
|-----|--------------|
|CA| $15.5K |
|FL| $14.6K|
|NY| $15.0K|
|TX| $15.9K|

</p>
Which are considerably less than AK, with an avg loan size of $17.4K.

#### Individuals

Now looking at the profile of individual borrowers, we produce the following plots:

<img src="./images/borrower_profile.png"/>

From this, we can conclude the following points:
-  Loan amounts spike around large round numbers and have more density in the < $20,000.00 range
-  The FICO distribution of funded loans has a clear cutoff at ~650 and is right skewed with a large mass less than 700 (considered as the cutoff between a good vs mediocre credit score)
-  A large % of the borrowers have been at their job for a long time, as represented by the large column at the 10 tick. The data lumps in any worker who has worked over 10 years at their current role with those that have been at their current role for exactly 10 years.
-  Almost half of all borrowers have a mortgage with another ~ 40% renting. Only 11.2% own their property, meaning that most borrowers in the data set will have a monthly obligation to rent or a mortgage which will have a large effect on underwriting and credit risk rating.




## Evaluating Returns

grade_returns.png
grade_annualized_returns.png

Because Lending Club asigns grades to their loans, we can evaluate the returns by grade on both an actual return and an annualized return basis. Also, because the loans can only take two different term lengths, 36 or 60 months, we can further divide the grades by term length. We hypothesize that the highest returns should be earned by 60 month rated B-D loans as these will have a greater return compared to their respective 36 month counterparts to compensate for the additional holding time required. Also, we expect these loans to outperform others by grade because A loans will have lower rates of return because they are considered the safest and the loans rated E-G will be expected to have high chargeoff rates, diminishing returns. 

<img src="./images/grade_returns.png"/>

<img src="./images/grade_annualized_returns.png"/>

From the two plots, we can see that the highest performing group is the 36 month term B loans with a return of 5.7% and an annualized return of 1.9%. Most surprisingly, we find that the rate of return of Lending Club loans by grade and term length are extremely low compared to the risk in investing in unsecured peer-to-peer loans. We will benchmark our algorithms against these rates of returns:

|Grade| Term | Return | Annualized Return|
|-----|------|--------|------------------|
|A|36|5.2%|1.7%|
|A|60|3.3%|0.7%|
|B|36|5.7%|1.9%|
|B|60|3.5%|0.7%|
|C|36|4.0%|1.3%|
|C|60|2.5%|0.5%|
|D|36|2.0%|0.7%|
|D|60|1.4%|0.3%|
|E|36|0.0%|0.0%|
|E|60|1.2%|0.2%|
|F|36|-1.2%|-0.4%|
|F|60|-1.7%|-0.3%|
|G|36|-8.8%|-3.0%|
|G|60|-5.3%|-1.1%|

## Data Cleaning / Transformation

## Training Models

## Tuned Model Evaluation
