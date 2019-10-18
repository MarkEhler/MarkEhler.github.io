---
layout: post
title:      "NLP for the Wine Connesuer"
date:       2019-10-18 00:04:24 +0000
permalink:  nlp_for_the_wine_connesuer
---

<p>
The content of your blog post goes here.Opinions start, revolve around, (and ultimately hopes to build upon) word choice. Is it possible to really know what someone is thinking based on the words they use? Can you teach a computer to understand sentiment? What if personal vocabulary labels your speech as an outlier? Is there such a thing? This project hopes to explore some of these questions using wine reviews and seek to answer the question if wine taste can be quantified.
<br>
<br>
<a href="https://imgur.com/pDqPdx1"><img src="https://imgur.com/pDqPdx1.png" title="Close Words" width=70%/></a>
<br>
These reviews are always one sentence in length, describing the subtleties of wine flavors. The vocabulary is such in the dataset, that a word like ‘brimstone’ would be most closely associated with earthy tastes like; ‘peat’, ‘chamomile’, ‘beeswax’, and ‘granite’. These reviews also give some clue as to how the wine tasters will score the wine with words like ‘excellent’ to describe a high scoring wine or universally bad descriptors like ‘abrasive’ for the under performing wines. 
<br>
<br>
<a href="https://imgur.com/7jexrhf"><img src="https://imgur.com/7jexrhf.png" title="Distribution"/> </a>
<br>
Wine critics are an interesting case study to me, because they have to power to polarize people towards or against something using only their words. These critics are of the fairly toothless variety. They might give a scathing review but still offer a score of 80/100 for the wine. Wine Enthusiast is, after all, selling ad space in the magazine.   With this in mind we are justifed in normalizing the target variable and transforming that data to a bivariate outcome split down the mean average.  Scores were either  a 0 "bad" or a 1 "good".
<br>
<br>
<a href="https://imgur.com/VeRzZeE"><img src="https://imgur.com/VeRzZeE.png" title="Word Choice"/> </a>

We can (and do) go several directions with information like this. First will be seeing if we can train a neural network to accurately predict the score our critics give after their description of the wine. It is an interesting side note that breaking the data apart by reviewer leads variations in word choice by reviewer and sparks the imagination for plotting unique word choice and variance in meaning from word to word and person to person.  Below you will see the word choice plotted out in 3d space for two of the wine reviewers.  You’ll notice expected overlap, likely for flavor words and wine names.  You’ll also notice that O’Keef seems to have a larger vocabulary, at least from this view.
<br>
<br>


For a first attempt run with NLP I rearranged the score outcome into a bivariate label, either the wine is good, or it is bad. Common sense baseline for this type of model would be the probability of guessing right; it would be 0.5 MAE or looking at it another way it would be 0.5 ROC/AUC. 
<br>
<br>

The random forest model scored a precision of 65% 
The most successful neural network scored a cross validated accuracy of 83%, it looked like this:
<br>
<a href="https://imgur.com/O9wA2QG"><img src="https://imgur.com/O9wA2QG.png" title="conf matrix"/></a>
<br>
```
model_four = Sequential() model_four.add(Embedding(input_dim=max_words, output_dim=embedding_dim, input_length=maxlen, weights=[embedding_matrix], trainable=False )) 
model_four.add(LSTM(50, return_sequences=True)) 
model_four.add(GlobalMaxPool1D()) 
model_four.add(Dropout(0.5)) 
model_four.add(Dense(32, kernel_regularizer=regularizers.l2(0.005), activation='relu'))))
model_four.add(Dropout(0.50)) 
model_four.add(Dense(1, activation='sigmoid')) 
model_four.summary()
```
<a href="https://imgur.com/5Be5vF5"><img src="https://imgur.com/5Be5vF5.png" title="Accuracy"/></a>
<a href="https://imgur.com/RfwJfIC"><img src="https://imgur.com/RfwJfIC.png" title="Loss"/></a>
</p>
