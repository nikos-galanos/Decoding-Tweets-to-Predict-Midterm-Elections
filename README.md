# Decoding-Tweets-to-Predict-Midterm-Elections

This project was completed by Nikos Galanos, Arushi Jain and Anant Vashistha, three Master of Business Analytics candidates from MIT, for the course 15.072 - Advanced Analytics Edge with Dr. Bart Van Paris.

## Motivation and Objective

Social channels are the primary source of spreading information and understanding public opinion. Social media has impacted our society, political discourse, and media landscape through hashtag activism on recent affairs such as the Overturning of Roe vs.Wade, Covid-19, and Black Lives Matter. Many parties also routinely use these channels for political campaigns and marketing activities for maximum public outreach. Twitter data shows that the election candidates and the general public were pretty active on Twitter before and during the US mid-term elections. We decided to leverage the tweets of the candidates and the public's reaction to those tweets as proxies for forecasting the mid-term election outcomes.

We developed a model to predict the outcome of US mid-term elections by identifying the critical topics (based on candidates’ tweet data) and analyzing public sentiment from the replies to those tweets. The model used the actual election outcome as the dependent variable predicted using a combination of sentiments and topics as well as additional Twitter metadata such as likes, retweets, and followers as features for each candidate. 

The model can be expanded to any election, given that the required tweets of the involved candidates, their public replies, and additional metrics are collected and analyzed. The report discusses a high-level view of the dataset (scraping, cleaning, and EDA), methodology (topic modeling, sentiment analysis, and feature engineering), model results, conclusion, and challenges.


## DATASET

### Data Scraping

● We collected candidate state and party information from Ballotpedia.
● For each candidate, we searched their Twitter handles and used Tweepy’s User Lookup API and Tweet Lookup API to get the user information and tweet data from 15th Sep - 15th Oct and their reply data from 8th Oct - 15th Oct. The number of replies we could fetch was limited to a week due to the limited number of tweets we could scrape (25000) as a part of Twitter’s Developer API’s Elevated Access. Nevertheless, we fetched up to 200 tweets per user and up to 1000 replies for each user. However, due to these limitations, we couldn’t fetch reply data for all the scraped tweets.
● We collected the election results information (number of votes, vote share, winner) from npr.org


## METHODOLOGY

### Topic Modeling

We first represent our tweets as a TF-IDF Vector. We then use Latent Dirichlet Allocation (LDA) to classify tweets (documents) into multiple topics and topics into multiple words using Dirichlet distributions.

#### Examples of Extracted Topics

Topic 1 Top Keywords: social, cost, medicare, security, senior, insurance, prescription, insulin
Example: @bittergertrude You are so right. No matter what your health conditions are, you deserve high quality healthcare with no out of pocket costs! I’m going to the Senate to do all I can to make that happen
(Topic Contribution: ~80%)

Topic 2 Top Keywords: abortion, woman, choice, libertarian, freedom, health, care, right, access
Example: RT @PattyMurray: It's heartbreaking &amp; wrong that a patient from Texas had to fly to WA state just to get abortion care
(Topic Contribution: ~73%)

Topic 3 Top Keywords: corporation, profit, resolution, location, inflation, energy
Example: Food and energy prices keep rising as inflation breaks expectations again 
(Topic Contribution: ~69%)


Since we have only 93 candidates and we plan to use topics as our features we combined similar-looking topics by manually looking at the top topic keywords, the top 30 most relevant terms from the topic graph, and sample representative tweets to arrive at 7 topics: 

- Election Campaigns
- Inflation/ Economy
- Healthcare
- Abortion
- Crime/ Race
- Education
- Current Events (HurricaneIan / Ukraine / Iran

### Sentiment Analysis

We conducted sentiment analysis on the replies dataset containing 36K replies to gauge the public sentiment on the candidate’s
tweets using three different models: Textblob, Flair, and Distil Bert.

### Feature Engineering

We arrive at candidate-level data in the following steps:

- Compute the average reply sentiment per candidate
- Multiply the average sentiment per candidate with the number of replies on each tweet to get the weighted tweet sentiment
- Multiply the weighted tweet sentiment with the topic contributions for each tweet to get topic sentiments for each tweet
- Average the topic sentiments per candidate on their tweets for each of the 7 topics

We repeat this exercise for the 3 sentiment models to get 3 datasets. We also use average likes per candidate, average retweets per candidate, and the number of candidates' followers as additional metadata information along with party information and information about whether the candidate is currently serving as a candidate (boolean). Hence, we arrive at 12 features for 93 candidates.

Since we need winners at the state level, instead of predicting a binary variable of win v/s loss, we predict the continuous variable 'vote share' for each candidate


## Results

The BERT model with Random Forest correctly classified the winning party in 31 out of 33 states and winning candidates in 30 out of 33 states. According to media reports, the incorrectly predicted states (party-level) were Nevada and Pennsylvania, out of which Nevada had a very close call.

## CONCLUSION
The results from our models show that social media can be a powerful tool in predicting important political and social outcomes. Through the limited data on tweets, replies, and Twitter metadata information of the contesting candidates, we could correctly predict the election results for 87 out of 93 candidates (61/65 in-sample and 26/28 out-of-sample). We also found that apart from the party information and the information about if the candidate is currently serving, the topics that the contesting candidates talk about are the most important indicators for the election results - Inflation, Abortion, and Healthcare, in our case. The significance of these topics also makes intuitive sense considering the fears of recession, the controversy around the overturning of Roe vs. Wade, and high healthcare costs in the USA.


## Final Note

For a detailed overview of our methods and findings, please see the report found in the repo entitled Report.pdf or the slide deck entitled Presentation.pdf. Thanks!
