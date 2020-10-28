# **INFO5731 Assignment Three**

In this assignment, you are required to conduct information extraction, semantic analysis based on **the dataset you collected from assignment two**. You may use scipy and numpy package in this assignment.

# **Question 1: Understand N-gram**

(45 points). Write a python program to conduct N-gram analysis based on the dataset in your assignment two:

(1) Count the frequency of all the N-grams (N=3).

(2) Calculate the probabilities for all the bigrams in the dataset by using the fomular count(w2 w1) / count(w2). For example, count(really like) / count(really) = 1 / 3 = 0.33.

(3) Extract all the **noun phrases** and calculate the relative probabilities of each review in terms of other reviews (abstracts, or tweets) by using the fomular frequency (noun phrase) / max frequency (noun phrase) on the whole dataset. Print out the result in a table with column name the all the noun phrases and row name as all the 100 reviews (abstracts, or tweets). 



```python
# codes
import nltk, re, string, collections
from nltk.util import ngrams # function for making ngrams

# upload csv file
with open("tweets.csv", "r", encoding='latin-1') as file:
    text = file.read()

# print out the first 1000 characters
text[0:1000]
```




    'ï»¿2020-10-27 06:16:59,"b\'I\\xe2\\x80\\x99m helping.. 10days since my first vaccine, No side effects...feeling good.  #BePartOfResearch #CovidVaccine #NHS\\xe2\\x80\\xa6 https://t.co/jYlDRppuXy\'"\n2020-10-27 06:02:16,"b\'#Covid vaccine (unapproved) is out in China, being sold at yuan 400 (US$60) and people are getting it\\xe2\\x80\\xa6 https://t.co/N2ennlLjnI\'"\n2020-10-27 06:01:00,b\'The Trump administration this week will announce a plan to cover the out-of-pocket costs of #COVID19 vaccines for m\\xe2\\x80\\xa6 https://t.co/9HRf6tOd9l\'\n2020-10-27 05:30:11,"b\'Egypt reaches deal with Russia over #Covidvaccine, amid Russian soft power push as concerns remain over the drugs c\\xe2\\x80\\xa6 https://t.co/3EGdOcFEb8\'"\n2020-10-27 04:41:45,b\'But we are supposed to trust these folks to give us a #COVID19 \\n#COVIDVaccine ? They knowingly had asbestos in baby\\xe2\\x80\\xa6 https://t.co/tpOwHxvRnc\'\n2020-10-27 04:35:19,b\'#CoronaVirusUpdates :\\n\\n#Australian state of #Victoria  records zero new #coronavirus cases in 4'




```python
# preprocessing 
# or punctuation marks other than periods
# also don't want to consider capitalization. 

# get rid of all the XML markup
text = re.sub('<.*>','',text)

# get rid of the "ENDOFARTICLE." text
text = re.sub('ENDOFARTICLE.','',text)

# get rid of punctuation (except periods)
punctuationNoPeriod = "[" + re.sub("\.","",string.punctuation) + "]"
text = re.sub(punctuationNoPeriod, "", text)

# make sure it looks ok
text[0:1000]
```




    'ï»¿20201027 061659bI\\xe2\\x80\\x99m helping 10days since my first vaccine No side effectsfeeling good  BePartOfResearch CovidVaccine NHS\\xe2\\x80\\xa6 httpstcojYlDRppuXy\n20201027 060216bCovid vaccine unapproved is out in China being sold at yuan 400 US60 and people are getting it\\xe2\\x80\\xa6 httpstcoN2ennlLjnI\n20201027 060100bThe Trump administration this week will announce a plan to cover the outofpocket costs of COVID19 vaccines for m\\xe2\\x80\\xa6 httpstco9HRf6tOd9l\n20201027 053011bEgypt reaches deal with Russia over Covidvaccine amid Russian soft power push as concerns remain over the drugs c\\xe2\\x80\\xa6 httpstco3EGdOcFEb8\n20201027 044145bBut we are supposed to trust these folks to give us a COVID19 \\nCOVIDVaccine  They knowingly had asbestos in baby\\xe2\\x80\\xa6 httpstcotpOwHxvRnc\n20201027 043519bCoronaVirusUpdates \\n\\nAustralian state of Victoria  records zero new coronavirus cases in 48 hours\\n\\nIndia to s\\xe2\\x80\\xa6 httpstcocjXs5IfUKv\n20201027 042146bCadila one of Indias two Covid va'




```python
tokenized = text.split()

# and get a list of all the bi-grams
esBigrams = ngrams(tokenized, 2)
```


```python
# get the frequency of each bigram in our corpus
esBigramFreq = collections.Counter(esBigrams)

# what are the ten most popular ngrams 
esBigramFreq.most_common(10)
```




    [(('up', 'to'), 15),
     (('have', 'a'), 14),
     (('to', 'the'), 14),
     (('the', 'NHS'), 14),
     (('NHS', 'Covid19'), 14),
     (('Covid19', 'Vaccine'), 14),
     (('Vaccine', 'Research'), 14),
     (('Research', 'Registry'), 13),
     (('you', 'have'), 12),
     (('a', 'spare'), 12)]



# **Question 2: Undersand TF-IDF and Document representation**

(40 points). Starting from the documents (all the reviews, or abstracts, or tweets) collected for assignment two, write a python program: 

(1) To build the **documents-terms weights (tf*idf) matrix bold text**.

(2) To rank the documents with respect to query (design a query by yourself, for example, "An Outstanding movie with a haunting performance and best character development") by using **cosine similarity**.


```python
import numpy as np 
import pandas as pd 
import re  
import nltk 
nltk.download('stopwords')  
from nltk.corpus import stopwords 
```

    [nltk_data] Downloading package stopwords to
    [nltk_data]     /Users/melissavang/nltk_data...
    [nltk_data]   Package stopwords is already up-to-date!



```python
tweets = pd.read_csv("tweets.csv")
```


```python
tweets.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>2020-10-27 06:16:59</th>
      <th>b'I\xe2\x80\x99m helping.. 10days since my first vaccine, No side effects...feeling good.  #BePartOfResearch #CovidVaccine #NHS\xe2\x80\xa6 https://t.co/jYlDRppuXy'</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020-10-27 06:02:16</td>
      <td>b'#Covid vaccine (unapproved) is out in China,...</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2020-10-27 06:01:00</td>
      <td>b'The Trump administration this week will anno...</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2020-10-27 05:30:11</td>
      <td>b'Egypt reaches deal with Russia over #Covidva...</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2020-10-27 04:41:45</td>
      <td>b'But we are supposed to trust these folks to ...</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2020-10-27 04:35:19</td>
      <td>b'#CoronaVirusUpdates :\n\n#Australian state o...</td>
    </tr>
  </tbody>
</table>
</div>




```python
tweets.shape
```




    (94, 2)



# **Question 3: Create your own training and evaluation data for sentiment analysis**

(15 points). **You dodn't need to write program for this question!** Read each review (abstract or tweet) you collected in detail, and annotate each review with a sentiment (positive, negative, or neutral). Save the annotated dataset into a csv file with three columns (first column: document_id, clean_text, sentiment), upload the csv file to GitHub and submit the file link blew. This datset will be used for assignment four: sentiment analysis and text classification. 



```python
# The GitHub link of your final csv file
https://github.com/vamel19/INFO5731_FALL2020/blob/master/Annotated%20dataset.csv
```
