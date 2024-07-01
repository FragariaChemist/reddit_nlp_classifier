# Web APIs & NLP -- A Potential Market Research and Sentiment Analysis Tool For the Goulet Pen Company
## Background

[The Goulet Pen Copany](https://www.gouletpens.com/) is an online only retail business centered on foutain pens, paper, and accessories.  Their company values emphasize personal customer service, product education, and establishing trust with their customer base. While they are a small business, they are well recognized in the fountain pen community.  


## Problem Statement

Goulet Pens is an online only retail store, so it's crucial that they maintain a positive relationship with online fountain pen communities.  Reddit's '[fountainpens](https://www.reddit.com/r/fountainpens/)' subreddit community has 292K members alone.  There are dozens of other fountain pen related communities on Reddit and platforms like Discord.  For this project, I will also be looking at the '[pens](https://www.reddit.com/r/pens/)' subreddit.  Fountain pens are discussed on both subreddit forums.

I will be looking at both subreddit communities and developing classification models to determine if posts originate from 'fountainpens' or 'stationery'.  Using these models can help Goulet Pens identify trends and gain insight into what customers want and their opinions about Goulet Pens.

---
## Dataset Dictionary

|Feature|Type|Source|Description|
|---|---|---|---|
|**cleaned_corpus**|*int/str*|Reddit|Combined title and post data.  Subreddits have been binarized.| 
|**corpus**|*str*|Reddit|Title and post text data from r/fountainpens and r/pens subreddit communities.
|**data_collection_log**|*various*|Generated|Data and time log of when Reddit posts were scraped.
|**text_processed_corpus**|*float*|Generated|cleaned_corpus.csv that has been processed with SpaCy.
|
## Dataset Sources
- [r/fountainpens](https://www.reddit.com/r/fountainpens/)
- [r/pens](https://www.reddit.com/r/pens/)
---
## Analysis

A total of 2800 posts were collected and the text inside post titles and bodies were analyzed.  1557 posts (56%) originated from r/fountains and 1243 (44%) from r/pens.  Four posts were dropped due to no text (emojis and photos only) or if they just contained stop words.

SpaCy was used as the natural language processor before modeling the data.
* [Text Classification using Python SpaCy](https://machinelearninggeek.com/text-classification-using-python-spacy/)
 * [SpaCy Tokens](https://spacy.io/api/token)
 * [Natural Language Processing With SpaCy in Python](https://realpython.com/natural-language-processing-spacy-python/#lemmatization)


The following table outlines which models were implemented and their results.  The goal was to achieve a high level of specificity, therefore the Bernoulli Naive Bayes & CountVectorizer model was chosen for text analysis. Ideally, the model should never classify a r/pens post as a r/fountainpens post as it is a more specialized group.

r/fountainpens posts is considered the positive target. The baseline model is 56%.
|Model Type|Train Data Accuracy|Test Data Accuracy|Prediction Accuracy|Sensitivity|Specificity|Precision|
|---|---|---|---|---|---|---|
|**Logistic Regression & CountVectorizer**|98%|89%|89%|92%|87%|90%|
|**Logistic Regression & TD IDF**|89%|87%|87%|91%|82%|86%|
|**Bernoulli Naive Bayes & CountVectorizer**|90%|90%|90%|88%|91%|93%|
|**Multinomial Naive Bayes & Countvectorizer**|91%|91%|91%|93%|88%|91%

#### Parameters used per model

*Logistic Regression &  CountVectorizer*<br>
    cvec__max_features 10000<br>
    cvec__min_df: 0.001<br>
    cvec__max_df: 0.7<br>
    cvec__ngram_range: (1,2)<br>
    logreg__C: 1.0<br>
    logreg__penalty: l2 (Ridge)<br>
    logreg__solver: liblinear'<br>

*Logistic Regression & TFI DF*<br>
    tvec__max_features: 300<br>
    tvec__min_df: 0.001<br>
    tvec__max_df: 0.7<br>
    tvec__ngram_range: (1, 2)<br>
    logreg__C: 1.0<br>
    logreg__penalty: l2 (Ridge)<br>
    logreg__solver: liblinear

*Bernoulli Naive Bayes & CountVectorizer*<br>
    bb__alpha: 0.75,<br>
    cvec__max_features: 2000<br>
    cvec__min_df: 3<br>
    cvec__max_df: 0.7<br>
    cvec__ngram_range: (1,3)

*Multinomial Naive Bayes & CountVectorizer*<br>
    bb__alpha: 0.95,<br>
    cvec__max_features: 2000<br>
    cvec__min_df: 3<br>
    cvec__max_df: 0.7<br>
    cvec__ngram_range: (1,3)
    
### Confusion Matrices
* [Logistic Regression & CountVectorizer](plot_images/logreg_cvec_confusion_matrix.png)
* [Logistic Regression & TF IDF](plot_images/logreg_tfidf_confusion_matrix.png)
* [Bernoulli Naive Bayes & CountVectorizer](plot_images/bernoulli_nb_cvec_confusion_matrix.png)
* [Multinomial Naive Bayes & CountVectorizer](plot_images/multinomial_nb_cvec_confusion_matrix.png)


---

# Conclusion

I was able to successfully create a model using SpaCy natural language processing, Bernoullin Naive Bayes, and CountVectorizer to categorize r/fountainpens and r/pens subreddit posts.  The model has a 90% prediction accuracy which is an improvement over the baseline model of 56%.  Furthermore, the model has a 91% specificity score which was a major goal of this project.  Only fountain pen related posts should be categorized as part of r/fountainpens.

I was able to gather insight into what community members were talking about after processing the text through Spacy and vectorizing the data.  Obvious words like 'fountain pen', 'pen ink', and 'gel pen' appeared, but I was able to see other trends.  There are many posts about showing off a new pen purchase or a specific brand of pen.  

Taking this one step further, I collected a list of brands sold by Goulet Pens and generated a chart showing which of them were talked about the most on the Reddit subforums.  I was also able to pull out the word 'Goulet' and compare its counts to other brands.

A model similar to this could be modified and used for market research and sentiment analysis.  If I were to spend more time on this project, I would like to learn more about SpaCy as I think it has more potential than this project shows.  I would also integrate post comments into the dataset.
