# Reading-Racial-Discrimination-in-Slavic-Studies-Scholarship-through-a-Digital-Lens

This is a repository for notebooks, data, extra details, and visualizations related to the article "Reading Racial Discrimination in Slavic Studies Scholarship through a Digital Lens" in *Slavic Review*. In this README, we describe all of the process behind building the datasets, the models we ran, and our preliminary results in further detail than permitted by the space limitations of the article published in the journal. The README also directs to the relevant datasets and models for each section of the associated *Slavic Review* article. 

# Corpus Building and Data Sources

### Hatebase 

Our initial experiments worked from terms contained in hatebase.  Using the [HateBase](https://github.com/hatebase/Hatebase-API-Docs), we dowloaded all of the terms in the database and created a single csv file (in the repository above).  For a searchable version of the terms [click here](https://flatgithub.com/Russian-NLP/Reading-Racial-Discrimination-in-Slavic-Studies-Scholarship/0_preliminary_approaches?filename=0_preliminary_approaches%2Fhatebase.csv&filters=&sha=d2b4f6e4ff3d94816b13071483d7e52b8fccc457&sort=&stickyColumnName=).  

### JSTOR Data for Research

The vast majority of our data comes from JSTOR Data for Research, which is now known as [Constellate](https://constellate.org/browse/jstor-subjects).  We used the JSTOR area studies filters to select texts from three distinct disciplines:
- Slavic Studies, 1991-2020, 41,251 titles
- African American Studies, 1985-2020, 57,958 titles  
- Gender Studies, 1992-2020, 24,999 titles 

For each text, JSTOR provides a list of all the words in the text and their frequency. While data was available for single terms (1-grams), two-term spans (2-grams), and three-term spans (3-grams), we only made use of the 1-gram data. Additionaly, JSTOR provides an XML file for each text with metadata.  The code to read these files and join them with the ngram data can be found in the notebook `join_JSTOR_metadata_with_project_data.ipynb`. 


JSTOR makes files available that provide data on the lexical features of the text without making it possible to reconstruct the article or study the co-occurrence of words in context. Given copyright restrictions, this is an understandable choice on the part of JSTOR. However, most text analysis methods and tools require raw text as input and are not able to process the data provided by Data for Research. The creation of pseudo-texts to solve this problem simply involved the addition of each term in the text the same number of times recorded in the data. For example, if the data tells us that the  term “sushki” appears three times in an article, we would create the pseudo-text “sushki sushki sushki” and combine this pseudo-text with others to form a pseudo-text of the entire article at hand. This choice isn’t ideal but allowed us to use topic modeling tools and to train classification models using the pseudo-texts.  Further research with the full text of the articles and book chapters would provide better results, but the pseudo-texts were sufficient for our experiments and rapid response research.

The texts of *Logos* (2010-2020) and *Veche* (1994-2018) were downloaded from their respective publishers, meaning that we had access to the texts of these journals in their original form. Our *Logos* dataset can be downloaded in full from [this Google Drive folder](https://drive.google.com/drive/folders/1XkDAaBmx2GEUTRNvbawYNX4KHmtn6U9P). 

## 1. Topic Modeling

Once we had a corpus, we were able to begin our next phase of digital research analysis by way of topic modeling. Specifically, we conducted our analysis using Bidirectional Encoder Representations from Transformers (BERT), a state-of-the-art method for natural language processing and was created by Google in 2018. We used an existing machine learning model with a method called term frequency-inverse document frequency (TF-IDF) to identify topics in our corpus of scholarly articles. TF-IDF is a method to quantify the significance of a specific term to a larger document or topic. Our program used TF-IDF to return the 200 most frequent terms in each of the topics it found. For more information, see Maarten Grootendorst, [“Topic Modeling with BERT. Leveraging BERT and TF-IDF to create easily interpretable topics” Medium, October 5, 2020](https://github.com/MaartenGr/BERTopic).  


## Sentiment Analysis

In addition to the topic modeling that formed that bulk of our engagement with the full English- and Russian-language corpus, our access to the full texts of *Logos* and *Veche* allowed us to run sentiment analysis on those journals using [Dostoevsky](https://pypi.org/project/dostoevsky/), a library that asks whether Russian-language texts exhibit a predominantly positive, neutral, or negative emotional tone. This [Google Collaboratory Notebook](https://colab.research.google.com/drive/14fyxLfmQy6C2kZnZKZlOfbyTEwNpe-i9?usp=sharing) has the code we used to run the sentiment analysis. The results show that the majority (97.6%) of articles from the Russian subset of our data are neutral in terms of sentiment. While neutral sentiment is to be expected in most scholarly work, articles about identity-based injustice and inequality would likely exhibit negative sentiment markers as well. The neutrality of Russian-language scholarship in high-profile journals may be another indicator that scholarship in the transnational Russosphere does not include active discussions of racial and bodily inequity. However, these results may also indicate that existing sentiment analysis libraries for the Russian language are not well-equipped to analyze tonal nuances in scholarly writing. Manual annotation of scholarly articles may be required to better prepare tools like Dostoevsky for detecting points of discussion and controversy in the Russophone sector of our field.


## 2. Perspectival Modeling

**introduce spaCy, how it was used to train the models
**how did we make/get the Slavic Studies/Gender Studies/African American Studies models? sentence or two on how the model was actually trained
**published example of how else to do this, check out figure XXXX in Distant Horizons (https://raw.githubusercontent.com/tedunderwood/horizon/master/chapter2/images/C2Fig2allSF.jpg)
**clarify relationship of this work to Underwood if the final version of the article is not sufficient, clarify here if necessary


## 3. Additional Images--> put in respective sections with a tiny amount of explination
**term frequency images? 
**color convergence for Slavic Studies/Gender Studies/African American Studies? Decreasing ability for model to predict. Purple and yellow?

## 4. Future Work with Russian Sources

Willing to expand our Russian corpora we have decided to analyse the content of [*Zhurnal'nyi Zal*](https://magazines.gorky.media/), which is a literature-based Internet catalog representing the activities of Russian liberal arts thick magazines published in Russia and abroad. The article contents include a title, body, author name and publication date. The data scraped was stripped out of HTML tags, XML comments, CSS information, and finally stored in JSON format. We were assisted in this work by Alexander Kudryashov and Michael Gelperin - 1st year Data, Culture and Visualization Master students at ITMO University, St. Petersburg.

We created a corpus of 24,295 articles from *Zhurnal’nyi Zal*. Of this corpus, the most articles came from the magazine *Noviy Mir*, which has the highest number of issues published in the entire digital collection. The archive contains issues of that publication since 1993.

Data scraping tech stack used consisted of pure Python 3 with its libraries: one for making network calls, another to analyze texts retrieved with regular expressions and one for converting data to JSON format and store it on the disk.

After collecting the data, the data was prepared for further study. Articles were split into metadata CSVs and raw texts. Apart from necessary steps such as cleansing, lemmatization, and tokenization, we focused on stopwords removal to improve the quality of the topics. 

Next, we loaded the texts into the (Latent Dirichlet Allocation (LDA) model)[https://towardsdatascience.com/topic-modeling-and-latent-dirichlet-allocation-in-python-9bf156893c24]. It output a set of topics and the most probable topic for each article. We joined the output with metadata and created interactive graphs using the (plot.ly Python library)[https://plotly.com/python/].

After those steps, it is possible to see articles per topic distribution for the whole journal or its subset with the ability to filter articles by publication date, genre, author, and other metadata. Another visualization is placing topic distributions over a timescale, allowing to observe topic dynamics. As an example, please see (this Colab notebook)[https://colab.research.google.com/drive/1h2Mmij4x7A_ujuwa5TgrZz0W96kpmkVH#scrollTo=SezSKgn8XPhK] which shows how topics change over time for *Studia* magazine.

In addition, we have translated the most common hate words found in prior steps into Russian language and calculated term frequencies for them for our corpus. The preliminary results could be found (here)[https://colab.research.google.com/drive/1h2Mmij4x7A_ujuwa5TgrZz0W96kpmkVH#scrollTo=SezSKgn8XPhK].

It is worth mentioning that the preliminary results of *Zhurnal'nyi Zal* corpus expand the discussion of Slavic Studies field and allow to raise more specific research questions on demand. We anticipate continuing to work on the topic modeling analysis of the *Zhurnal'nyi Zal* corpus. 

