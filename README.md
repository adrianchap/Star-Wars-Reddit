# Project 3: Web APIs & NLP

## Star Wars Reddit Classification
by Adrian Chapman

### Problem Statement

Reddit is a community driven message board with many subreddits that users can subscribe to.  Each subreddit is highly moderated and has its own rules, community, and feel.  If a user post does not meet the expectations of the subreddit community, moderators may suggest posting in a different subreddit, may remove the post, or may even ban the user.  Each subreddit is monitored by moderators to ensure the rules of the community are upheld.  

This study aims to determine if advanced classification techniques would be helpful to Reddit moderators in discovering posts that are not fit for their subreddit and perhaps belong in another subreddit.  Two subreddits with opposite views on the same subject matter were chosen to build a corpus of text data to train several classification models on.  These models could help Reddit moderators by auto flagging posts that are predicted to be more similar to the views expressed in the opposing subreddit.

Specifically, 1,000 posts were pulled from the [r/saltierthancrait](https://www.reddit.com/r/saltierthancrait/) and [r/starwarscantina](https://www.reddit.com/r/saltierthancrait/) subreddits.  Both of these subreddits have similar content in that discussion is focused on the recent Star Wars sequel trilogy released by Disney.  However, r/starwarscantina is explicit about being a positive community, and r/saltierthancrait is known for their negative views about the Star Wars franchise since Disney took it over in 2012.

By building a classifier that can accuratly predict when a post belongs in r/saltierthancrait as opposed to r/starwarscantina, we could help moderators of r/starwarscantina find posts that are overly negative and more similar to the content found in r/saltierthancrait to help ensure a positive experience for this and other subreddit communities.


### Data:

The [PRAW](https://praw.readthedocs.io/en/latest/) Reddit API wrapper was used to pull 1,000 posts from both r/saltierthancrait and r/starwarscantina on 10/13/2020.  For the purposes of this study, only the text from the Title and the Post were used.  

The raw data used in this study can be found in 'rawdata.csv' in the *data* directory, and the script for the PRAW data pull can be found in 01_PRAW_pull.ipynb in the *code* directory.
  
  
### Methodology:

Before modeling, the text was cleaned by removing hyperlinks and punctuation.  Missing data for the text fields were replaced with an empty string.  The post title and post body text were concatenated into a single field. While stemming and lemmatizing were both considered, ultimately the text was mostly preserved in it's original form before vectorizing.

### EDA:

Word count for the two subreddits was explored using CountVectorizer.  The most popular words used in each subreddit returned fairly similar results.  On average, r/saltierthancrait was more verbose than r/starwarscantina, having an average post length of almost 120 words compared to just 70. This discovery proved fruitful as word count was kept as an additional feature for modeling.

The VADER sentiment analysis tool was also applied to determine differences in sentiment between the two subreddits.  On average, r/saltierthancrait was much more negative while r/starwarscantina had an overall positive valence.  The VADER negative, neutral, and positive scores were also retained as features for modeling. 

### Data Preprocessing

In early stages of modleing, tests were run to see if TfidfVectorizer or CountVectorizer would be more useful for vectorizing the text data.  Ultimately, TFIDF proved to be more successful for the models built.  For the training and test data sets, TfidfVectorizer was used with to create 3,000 features of words and bigrams.  The default english stop-words were removed.  Words in over 90% of documents were not considered, nor were words in less than 3 documents.
   
   
### Modeling:

In addition to the vectorized text described above, a variable containing the word count of the post, and 4 variables from the VADER sentiment analysis were used for all models.

Many classification models were tested including Multinomial Naive Bayes, Decision Trees, Random Forest, Extra Trees, Bagging Classifier, Ada Boost, and Gradient Boosted Trees.  A grid search over several hyperparameters was established for each model.  Ultimately, the top 3 best performing models (Extra Trees, Ada Boost, Gradient Boost) were fed into a voting classifier.  This model did not significantly outperform the Gradient Boosted Tree model, however.  
  
### Results:

Gradient Boosted Trees was the single best performing model.  While the model was somewhat overfit, having a train accuracy of 91.7%, the test accuracy of 78.3 was significantly better than the baseline model score of 51%.  Specificity was also looked at in detail as we did not want to classify posts that do indeed belong to r/starwarscantina erroneously to be flagged for removal.

The most important features for modeling were explored in the Ada Boost model, revealing the single most important feature to be the compound sentiment score from VADER.  Sentiment proved to be an extremely valuable feature to add to the modeling data in this case as the text features between the two subreddits were otherwise quite similar.

### Conclusions:

Ultimately, this study showed that classifying reddit posts between subreddits with opposing views on the same subject can be improved well over baseline especially with the aid of sentiment analysis.  Further refining these models could provide utility for moderators of r/starwarscantina and other subreddits who wish to automate the discovery of posts that are not fitting for their subreddit community.
  
### Next Steps:

Further model testing on larget quantities of data or other subreddits could help establish best practices for the classification techniques explored in this study.  Gaining access to posts that have already been deleted or removed by moderators would provide an even better training data set for future studies in this field. 
    
  


  



