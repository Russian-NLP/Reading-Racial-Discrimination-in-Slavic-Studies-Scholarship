# Reading-Racial-Discrimination-in-Slavic-Studies-Scholarship-through-a-Digital-Lens

This is a repository for notebooks, data, extra details, and visualizations related to the article "Reading Racial Discrimination in Slavic Studies Scholarship through a Digital Lens" in *Slavic Review*. In this README, we describe all of the process behind building the datasets, the models we ran, and our preliminary results in further detail than permitted by the space limitations of the article published in the journal. The README also directs to the relevant datasets and models for each section of the associated *Slavic Review* article. 

## 0. Preliminary Approaches: Corpus Building and Methodology

Terms of use of electronic repositories such as JSTOR prevent us from sharing our full dataset in this GitHub repository. This section explains how we built our corpus so that others can recreate the dataset for themselves. The means by which we downloaded parts of our corpus resulted in severe limitations to our dataset, which we had to overcome.  The texts of *Logos* (2010-2020) and *Veche* (1994-2018) were downloaded from their respective publishers, meaning that we had access to the texts of these journals in their original form. However, because we obtained our datasets for *Slavic Review* (1991-2016), *Russian Review* (1991-2014), and *SEEJ* (1991-2016) through [JSTOR Data for Research](https://about.jstor.org/whats-in-jstor/text-mining-support/), that data does not contain the full text of the articles and book chapters it includes. For each text, the service offers a metadata file with essential bibliographic information as well as files with the frequencies in the text of single terms (1-grams), two-term spans (2-grams), and three-term spans (3-grams). 

To overcome this restriction, we created pseudo-texts from metadata and lists of most frequent terms and phrases from each article or chapter. In other words, JSTOR makes files available that provide data on the lexical features of the text without making it possible to reconstruct the article or study the co-occurrence of words in context. Given copyright restrictions, this is an understandable choice on the part of JSTOR. However, most text analysis methods and tools require raw text as input and are not able to process the data provided by Data for Research. The creation of pseudo-texts to solve this problem simply involved the addition of each term in the text the same number of times recorded in the data. For example, if the data tells us that the  term “sushki” appears three times in an article, we would create the pseudo-text “sushki sushki sushki” and combine this pseudo-text with others to form a pseudo-text of the entire article at hand. This choice isn’t ideal but allowed us to use topic modeling tools and to train classification models using the pseudo-texts.  Further research with the full text of the articles and book chapters would provide better results, but the pseudo-texts were sufficient for our experiments and rapid response research.

**can we share our pseudo texts? Or not?

## 1. Topic Modeling

Once we had a corpus, we were able to begin our next phase of digital research analysis by way of topic modeling. Specifically, we conducted our analysis using Bidirectional Encoder Representations from Transformers (BERT), a state-of-the-art method for natural language processing and was created by Google in 2018. We used an existing machine learning model with a method called term frequency-inverse document frequency (TF-IDF) to identify topics in our corpus of scholarly articles. TF-IDF is a method to quantify the significance of a specific term to a larger document or topic. Our program used TF-IDF to return the 200 most frequent terms in each of the topics it found. For more information, see Maarten Grootendorst, [“Topic Modeling with BERT. Leveraging BERT and TF-IDF to create easily interpretable topics” Medium, October 5, 2020](https://github.com/MaartenGr/BERTopic).  


## Sentiment Analysis

In addition to the topic modeling that formed that bulk of our engagement with the full English- and Russian-language corpus, our access to the full texts of *Logos* and *Veche* allowed us to run sentiment analysis on those journals using [Dostoevsky](https://pypi.org/project/dostoevsky/), a library that asks whether Russian-language texts exhibit a predominantly positive, neutral, or negative emotional tone. This [Google Collaboratory Notebook](https://colab.research.google.com/drive/14fyxLfmQy6C2kZnZKZlOfbyTEwNpe-i9?usp=sharing) has the code we used to run the sentiment analysis. The results show that the majority (97.6%) of articles from the Russian subset of our data are neutral in terms of sentiment. While neutral sentiment is to be expected in most scholarly work, articles about identity-based injustice and inequality would likely exhibit negative sentiment markers as well. The neutrality of Russian-language scholarship in high-profile journals may be another indicator that scholarship in the transnational Russosphere does not include active discussions of racial and bodily inequity. However, these results may also indicate that existing sentiment analysis libraries for the Russian language are not well-equipped to analyze tonal nuances in scholarly writing. Manual annotation of scholarly articles may be required to better prepare tools like Dostoevsky for detecting points of discussion and controversy in the Russophone sector of our field.

**Add link to Google folder for Logos


## 2. Perspectival Modeling

**introduce spaCy, how it was used to train the models
**how did we make/get the Slavic Studies/Gender Studies/African American Studies models? sentence or two on how the model was actually trained
**published example of how else to do this, check out figure XXXX in Distant Horizons (https://raw.githubusercontent.com/tedunderwood/horizon/master/chapter2/images/C2Fig2allSF.jpg)
**clarify relationship of this work to Underwood if the final version of the article is not sufficient, clarify here if necessary


## 3. Additional Images--> put in respective sections with a tiny amount of explination
**term frequency images? 
**color convergence for Slavic Studies/Gender Studies/African American Studies? Decreasing ability for model to predict. Purple and yellow?

## 4. Future work/Discussion
Russian term frequency/Zhurnal'nyi zal, topic modeling over time
