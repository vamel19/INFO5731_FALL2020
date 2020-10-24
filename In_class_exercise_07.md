# **The seventh in-class-exercise (20 points in total, 10/21/2020)**

Question description: In the last in-class-exercise (exercise-06), you collected the titles of 100 articles about data science, natural language processing, and machine learning. The 100 article titles will be used as the text corpus of this exercise. Perform the following tasks:

## (1) (8 points) Generate K topics by using LDA, the number of topics K should be decided by the coherence score, then summarize what are the topics. You may refer the code here: 

https://www.machinelearningplus.com/nlp/topic-modeling-gensim-python/


```python
!pip install gensim
```

    Requirement already satisfied: gensim in /opt/anaconda3/lib/python3.8/site-packages (3.8.3)
    Requirement already satisfied: scipy>=0.18.1 in /opt/anaconda3/lib/python3.8/site-packages (from gensim) (1.5.0)
    Requirement already satisfied: smart-open>=1.8.1 in /opt/anaconda3/lib/python3.8/site-packages (from gensim) (3.0.0)
    Requirement already satisfied: six>=1.5.0 in /opt/anaconda3/lib/python3.8/site-packages (from gensim) (1.15.0)
    Requirement already satisfied: numpy>=1.11.3 in /opt/anaconda3/lib/python3.8/site-packages (from gensim) (1.18.5)
    Requirement already satisfied: requests in /opt/anaconda3/lib/python3.8/site-packages (from smart-open>=1.8.1->gensim) (2.24.0)
    Requirement already satisfied: chardet<4,>=3.0.2 in /opt/anaconda3/lib/python3.8/site-packages (from requests->smart-open>=1.8.1->gensim) (3.0.4)
    Requirement already satisfied: certifi>=2017.4.17 in /opt/anaconda3/lib/python3.8/site-packages (from requests->smart-open>=1.8.1->gensim) (2020.6.20)
    Requirement already satisfied: urllib3!=1.25.0,!=1.25.1,<1.26,>=1.21.1 in /opt/anaconda3/lib/python3.8/site-packages (from requests->smart-open>=1.8.1->gensim) (1.25.9)
    Requirement already satisfied: idna<3,>=2.5 in /opt/anaconda3/lib/python3.8/site-packages (from requests->smart-open>=1.8.1->gensim) (2.10)



```python
# Write your code here
import pandas as pd
```


```python
from nltk.tokenize import RegexpTokenizer
from stop_words import get_stop_words
from nltk.stem.porter import PorterStemmer
from gensim import corpora, models
import gensim

tokenizer = RegexpTokenizer(r'\w+')

# create English stop words list
en_stop = get_stop_words('en')

# Create p_stemmer of class PorterStemmer
p_stemmer = PorterStemmer()
    
# create sample list
df = pd.read_csv("Titles.csv")
data_list = df.values.tolist()
new_list = []
for items in data_list:
    for item in items:
        new_list.append(item)

# list for tokenized documents in loop
texts = []

# loop through document list
for i in new_list:
    
    # clean and tokenize document string
    raw = i.lower()
    tokens = tokenizer.tokenize(raw)

    # remove stop words from tokens
    stopped_tokens = [i for i in tokens if not i in en_stop]
    
    # stem tokens
    stemmed_tokens = [p_stemmer.stem(i) for i in stopped_tokens]
    
    # add tokens to list
    texts.append(stemmed_tokens)

# turn our tokenized documents into a id <-> term dictionary
dictionary = corpora.Dictionary(texts)
    
# convert tokenized documents into a document-term matrix
corpus = [dictionary.doc2bow(text) for text in texts]

for topic_number in range(1,10,2):
    for number_word in range (5, 10, 5):
# generate LDA model
        ldamodel = gensim.models.LdaModel(corpus, num_topics= topic_number, id2word = dictionary)
        print(ldamodel.print_topics(num_topics=topic_number, num_words=number_word))
        # Compute Perplexity
        
        # Compute Coherence Score
        coherence_model_lda = CoherenceModel(model=ldamodel, texts=texts, dictionary=dictionary, coherence='c_v')
        coherence_lda = coherence_model_lda.get_coherence()
        print('\nCoherence Score: ', coherence_lda)
```

    [(0, '0.110*"data" + 0.098*"analyt" + 0.087*"big" + 0.010*"survey" + 0.010*"smart"')]
    
    Coherence Score:  0.42262699775845275
    [(0, '0.140*"data" + 0.112*"analyt" + 0.106*"big" + 0.014*"smart" + 0.012*"survey"'), (1, '0.094*"analyt" + 0.087*"data" + 0.081*"big" + 0.020*"healthcar" + 0.019*"applic"'), (2, '0.067*"analyt" + 0.066*"data" + 0.044*"big" + 0.019*"firm" + 0.018*"perform"')]
    
    Coherence Score:  0.46738152034476305
    [(0, '0.033*"big" + 0.033*"analyt" + 0.033*"data" + 0.017*"capabl" + 0.017*"research"'), (1, '0.035*"big" + 0.034*"data" + 0.033*"analyt" + 0.016*"applic" + 0.016*"manag"'), (2, '0.143*"data" + 0.120*"analyt" + 0.101*"big" + 0.017*"smart" + 0.012*"applic"'), (3, '0.097*"data" + 0.091*"analyt" + 0.086*"big" + 0.019*"busi" + 0.019*"valu"'), (4, '0.100*"analyt" + 0.099*"data" + 0.091*"big" + 0.018*"iot" + 0.016*"capabl"')]
    
    Coherence Score:  0.41952511094288597
    [(0, '0.143*"data" + 0.123*"analyt" + 0.115*"big" + 0.012*"smart" + 0.012*"applic"'), (1, '0.055*"data" + 0.049*"analyt" + 0.027*"big" + 0.018*"cloud" + 0.018*"experi"'), (2, '0.110*"data" + 0.109*"analyt" + 0.061*"big" + 0.027*"survey" + 0.027*"social"'), (3, '0.025*"data" + 0.025*"big" + 0.025*"analyt" + 0.025*"smart" + 0.025*"internet"'), (4, '0.074*"data" + 0.068*"analyt" + 0.058*"big" + 0.027*"learn" + 0.012*"survey"'), (5, '0.089*"big" + 0.089*"data" + 0.080*"analyt" + 0.032*"system" + 0.032*"manag"'), (6, '0.074*"analyt" + 0.071*"data" + 0.051*"big" + 0.016*"platform" + 0.016*"stratospher"')]
    
    Coherence Score:  0.40459644569055186
    [(0, '0.074*"analyt" + 0.074*"data" + 0.059*"big" + 0.030*"mainten" + 0.030*"manag"'), (1, '0.148*"data" + 0.126*"analyt" + 0.110*"big" + 0.014*"applic" + 0.010*"learn"'), (2, '0.004*"data" + 0.004*"analyt" + 0.004*"big" + 0.004*"survey" + 0.004*"applic"'), (3, '0.083*"analyt" + 0.083*"big" + 0.083*"data" + 0.029*"toward" + 0.029*"survey"'), (4, '0.048*"big" + 0.048*"analyt" + 0.048*"data" + 0.048*"healthcar" + 0.025*"survey"'), (5, '0.088*"analyt" + 0.088*"data" + 0.078*"big" + 0.030*"iot" + 0.030*"smart"'), (6, '0.028*"big" + 0.028*"analyt" + 0.028*"data" + 0.028*"test" + 0.028*"technolog"'), (7, '0.004*"data" + 0.004*"big" + 0.004*"analyt" + 0.004*"applic" + 0.004*"survey"'), (8, '0.064*"analyt" + 0.044*"data" + 0.044*"big" + 0.023*"effect" + 0.023*"product"')]
    
    Coherence Score:  0.43172251490115615



```python
from gensim.models import CoherenceModel
```

## (2) (8 points) Generate K topics by using LSA, the number of topics K should be decided by the coherence score, then summarize what are the topics. You may refer the code here:

https://www.datacamp.com/community/tutorials/discovering-hidden-topics-python


```python
# Write your code here
import pandas as pd
```


```python
from nltk.tokenize import RegexpTokenizer
from stop_words import get_stop_words
from nltk.stem.porter import PorterStemmer
from gensim import corpora, models
import gensim

tokenizer = RegexpTokenizer(r'\w+')

# create English stop words list
en_stop = get_stop_words('en')

# Create p_stemmer of class PorterStemmer
p_stemmer = PorterStemmer()
    
# create sample list
df = pd.read_csv("Titles.csv")
data_list = df.values.tolist()
new_list = []
for items in data_list:
    for item in items:
        new_list.append(item)

# list for tokenized documents in loop
texts = []

# loop through document list
for i in new_list:
    
    # clean and tokenize document string
    raw = i.lower()
    tokens = tokenizer.tokenize(raw)

    # remove stop words from tokens
    stopped_tokens = [i for i in tokens if not i in en_stop]
    
    # stem tokens
    stemmed_tokens = [p_stemmer.stem(i) for i in stopped_tokens]
    
    # add tokens to list
    texts.append(stemmed_tokens)

# turn our tokenized documents into a id <-> term dictionary
dictionary = corpora.Dictionary(texts)
    
# convert tokenized documents into a document-term matrix
corpus = [dictionary.doc2bow(text) for text in texts]

for topic_number in range(1,10,2):
    for number_word in range (5, 10, 5):
# generate LSA model
        lsamodel = gensim.models.LsiModel(corpus, num_topics= topic_number, id2word = dictionary)
        print(lsamodel.print_topics(num_topics=topic_number, num_words=number_word))
        # Compute Perplexity
        
        # Compute Coherence Score
        coherence_model_lsa = CoherenceModel(model=lsamodel, texts=texts, dictionary=dictionary, coherence='c_v')
        coherence_lsa = coherence_model_lsa.get_coherence()
        print('\nCoherence Score: ', coherence_lsa)
```

    [(0, '0.636*"data" + 0.536*"analyt" + 0.515*"big" + 0.049*"smart" + 0.049*"survey"')]
    
    Coherence Score:  0.42262699775845264
    [(0, '0.636*"data" + 0.536*"analyt" + 0.515*"big" + 0.049*"smart" + 0.049*"survey"'), (1, '-0.613*"big" + 0.327*"data" + 0.226*"smart" + -0.191*"survey" + 0.170*"analyt"'), (2, '-0.319*"manag" + -0.309*"applic" + -0.274*"suppli" + -0.274*"chain" + 0.266*"perform"')]
    
    Coherence Score:  0.3958945878614419
    [(0, '0.636*"data" + 0.536*"analyt" + 0.515*"big" + 0.049*"smart" + 0.049*"survey"'), (1, '-0.613*"big" + 0.327*"data" + 0.226*"smart" + -0.191*"survey" + 0.170*"analyt"'), (2, '-0.319*"manag" + -0.309*"applic" + -0.274*"chain" + -0.274*"suppli" + 0.266*"perform"'), (3, '0.412*"smart" + 0.297*"manag" + 0.259*"use" + 0.222*"chain" + 0.222*"suppli"'), (4, '-0.322*"research" + -0.294*"perform" + -0.277*"firm" + -0.257*"busi" + -0.247*"capabl"')]
    
    Coherence Score:  0.38633677522370835
    [(0, '0.636*"data" + 0.536*"analyt" + 0.515*"big" + 0.049*"smart" + 0.049*"survey"'), (1, '-0.613*"big" + 0.327*"data" + 0.226*"smart" + -0.191*"survey" + 0.170*"analyt"'), (2, '-0.319*"manag" + -0.309*"applic" + -0.274*"chain" + -0.274*"suppli" + 0.266*"perform"'), (3, '0.412*"smart" + 0.297*"manag" + 0.259*"use" + 0.222*"chain" + 0.222*"suppli"'), (4, '-0.322*"research" + -0.294*"perform" + -0.277*"firm" + -0.257*"busi" + -0.247*"capabl"'), (5, '-0.339*"review" + -0.290*"research" + 0.243*"manag" + -0.234*"agenda" + 0.233*"perform"'), (6, '-0.410*"analyt" + 0.337*"data" + -0.246*"survey" + -0.227*"learn" + -0.212*"system"')]
    
    Coherence Score:  0.38781646815554544
    [(0, '0.636*"data" + 0.536*"analyt" + 0.515*"big" + 0.049*"smart" + 0.049*"survey"'), (1, '-0.613*"big" + 0.327*"data" + 0.226*"smart" + -0.191*"survey" + 0.170*"analyt"'), (2, '-0.319*"manag" + -0.309*"applic" + -0.274*"suppli" + -0.274*"chain" + 0.266*"perform"'), (3, '0.412*"smart" + 0.297*"manag" + 0.259*"use" + 0.222*"chain" + 0.222*"suppli"'), (4, '-0.322*"research" + -0.294*"perform" + -0.277*"firm" + -0.257*"busi" + -0.247*"capabl"'), (5, '-0.339*"review" + -0.290*"research" + 0.243*"manag" + -0.234*"agenda" + 0.233*"perform"'), (6, '-0.410*"analyt" + 0.337*"data" + -0.246*"survey" + -0.227*"learn" + -0.212*"system"'), (7, '0.331*"learn" + -0.294*"new" + 0.236*"survey" + -0.225*"analyt" + -0.180*"product"'), (8, '-0.426*"system" + -0.239*"applic" + 0.228*"learn" + 0.219*"challeng" + -0.208*"review"')]
    
    Coherence Score:  0.40823986410497914



```python
def prepare_corpus(doc_clean):
    """
    Input  : clean document
    Purpose: create term dictionary of our courpus and Converting list of documents (corpus) into Document Term Matrix
    Output : term dictionary and Document Term Matrix
    """
    # Creating the term dictionary of our courpus, where every unique term is assigned an index. dictionary = corpora.Dictionary(doc_clean)
    dictionary = corpora.Dictionary(doc_clean)
    # Converting list of documents (corpus) into Document Term Matrix using dictionary prepared above.
    doc_term_matrix = [dictionary.doc2bow(doc) for doc in doc_clean]
    # generate LDA model
    return dictionary,doc_term_matrix
```


```python
def create_gensim_lsa_model(doc_clean,number_of_topics,words):
    """
    Input  : clean document, number of topics and number of words associated with each topic
    Purpose: create LSA model using gensim
    Output : return LSA model
    """
    dictionary,doc_term_matrix=prepare_corpus(doc_clean)
    # generate LSA model
    lsamodel = LsiModel(doc_term_matrix, num_topics=number_of_topics, id2word = dictionary)  # train model
    print(lsamodel.print_topics(num_topics=number_of_topics, num_words=words))
    return lsamodel
```


```python
def compute_coherence_values(dictionary, doc_term_matrix, doc_clean, stop, start=2, step=3):
    """
    Input   : dictionary : Gensim dictionary
              corpus : Gensim corpus
              texts : List of input texts
              stop : Max num of topics
    purpose : Compute c_v coherence for various number of topics
    Output  : model_list : List of LSA topic models
              coherence_values : Coherence values corresponding to the LDA model with respective number of topics
    """
    coherence_values = []
    model_list = []
    for num_topics in range(start, stop, step):
        # generate LSA model
        model = LsiModel(doc_term_matrix, num_topics=number_of_topics, id2word = dictionary)  # train model
        model_list.append(model)
        coherencemodel = CoherenceModel(model=model, texts=doc_clean, dictionary=dictionary, coherence='c_v')
        coherence_values.append(coherencemodel.get_coherence())
    return model_list, coherence_values
```


```python
def plot_graph(doc_clean,start, stop, step):
    dictionary,doc_term_matrix=prepare_corpus(doc_clean)
    model_list, coherence_values = compute_coherence_values(dictionary, doc_term_matrix,doc_clean,
                                                            stop, start, step)
    # Show graph
    x = range(start, stop, step)
    plt.plot(x, coherence_values)
    plt.xlabel("Number of Topics")
    plt.ylabel("Coherence score")
    plt.legend(("coherence_values"), loc='best')
    plt.show()
```


```python
start,stop,step=1,10,2
plot_graph(clean_text,start,stop,step)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-12-91dc1e11d32d> in <module>
          1 start,stop,step=1,10,2
    ----> 2 plot_graph(clean_text,start,stop,step)
    

    NameError: name 'clean_text' is not defined



```python
!pip install stop-words
```

    Requirement already satisfied: stop-words in /opt/anaconda3/lib/python3.8/site-packages (2018.7.23)


## (3) (4 points) Compare the results generated by the two topic modeling algorithms, which one is better? You should explain the reasons in details.


```python
Comparing the results generated by the two topic modeling algorithms, both are used to compare classification of the overlapping topics. Based from the number of topics, LDA methods is better than LSA for finding accturate mixture of topics.
```
