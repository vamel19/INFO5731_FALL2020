# **The seventh in-class-exercise (20 points in total, 10/21/2020)**

Question description: In the last in-class-exercise (exercise-06), you collected the titles of 100 articles about data science, natural language processing, and machine learning. The 100 article titles will be used as the text corpus of this exercise. Perform the following tasks:

## (1) (8 points) Generate K topics by using LDA, the number of topics K should be decided by the coherence score, then summarize what are the topics. You may refer the code here: 

https://www.machinelearningplus.com/nlp/topic-modeling-gensim-python/


```python
# Write your code here
import pandas as pd
```


```python
df = pd.read_csv("Titles.csv")

print(df)
```

       Understanding big data: Analytics for enterprise class hadoop and streaming data
    0   Big data analytics in healthcare: promise and ...                              
    1                                  Big data analytics                              
    2                        Trends in big data analytics                              
    3   Deep learning applications and challenges in b...                              
    4    An introduction to social network data analytics                              
    ..                                                ...                              
    94  What can big data and text analytics tell us a...                              
    95  Convex optimization for big data: Scalable, ra...                              
    96  Modeling and optimization for big data analyti...                              
    97  Leveraging big data analytics to reduce health...                              
    98       Bluedbm: An appliance for big data analytics                              
    
    [99 rows x 1 columns]



```python
# Run in python console
import nltk; nltk.download('stopwords')

# Run in terminal or command prompt
python3 -m spacy download en
```


      File "<ipython-input-18-25b4fa76927e>", line 5
        python3 -m spacy download en
                   ^
    SyntaxError: invalid syntax




```python
import re
import numpy as np
import pandas as pd
from pprint import pprint

# Gensim
import gensim
import gensim.corpora as corpora
from gensim.utils import simple_preprocess
from gensim.models import CoherenceModel

# spacy for lemmatization
import spacy

# Plotting tools
import pyLDAvis
import pyLDAvis.gensim  # don't skip this
import matplotlib.pyplot as plt
%matplotlib inline

# Enable logging for gensim - optional
import logging
logging.basicConfig(format='%(asctime)s : %(levelname)s : %(message)s', level=logging.ERROR)

import warnings
warnings.filterwarnings("ignore",category=DeprecationWarning)
```


    ---------------------------------------------------------------------------

    ModuleNotFoundError                       Traceback (most recent call last)

    <ipython-input-19-b98705f8e282> in <module>
          5 
          6 # Gensim
    ----> 7 import gensim
          8 import gensim.corpora as corpora
          9 from gensim.utils import simple_preprocess


    ModuleNotFoundError: No module named 'gensim'



```python
# NLTK Stop words
from nltk.corpus import stopwords
stop_words = stopwords.words('english')
stop_words.extend(['from', 'subject', 're', 'edu', 'use'])
```


    ---------------------------------------------------------------------------

    LookupError                               Traceback (most recent call last)

    /opt/anaconda3/lib/python3.8/site-packages/nltk/corpus/util.py in __load(self)
         82                 try:
    ---> 83                     root = nltk.data.find("{}/{}".format(self.subdir, zip_name))
         84                 except LookupError:


    /opt/anaconda3/lib/python3.8/site-packages/nltk/data.py in find(resource_name, paths)
        584     resource_not_found = "\n%s\n%s\n%s\n" % (sep, msg, sep)
    --> 585     raise LookupError(resource_not_found)
        586 


    LookupError: 
    **********************************************************************
      Resource stopwords not found.
      Please use the NLTK Downloader to obtain the resource:
    
      >>> import nltk
      >>> nltk.download('stopwords')
      
      For more information see: https://www.nltk.org/data.html
    
      Attempted to load corpora/stopwords.zip/stopwords/
    
      Searched in:
        - '/Users/melissavang/nltk_data'
        - '/opt/anaconda3/nltk_data'
        - '/opt/anaconda3/share/nltk_data'
        - '/opt/anaconda3/lib/nltk_data'
        - '/usr/share/nltk_data'
        - '/usr/local/share/nltk_data'
        - '/usr/lib/nltk_data'
        - '/usr/local/lib/nltk_data'
    **********************************************************************


    
    During handling of the above exception, another exception occurred:


    LookupError                               Traceback (most recent call last)

    <ipython-input-20-e823496d40c9> in <module>
          1 # NLTK Stop words
          2 from nltk.corpus import stopwords
    ----> 3 stop_words = stopwords.words('english')
          4 stop_words.extend(['from', 'subject', 're', 'edu', 'use'])


    /opt/anaconda3/lib/python3.8/site-packages/nltk/corpus/util.py in __getattr__(self, attr)
        118             raise AttributeError("LazyCorpusLoader object has no attribute '__bases__'")
        119 
    --> 120         self.__load()
        121         # This looks circular, but its not, since __load() changes our
        122         # __class__ to something new:


    /opt/anaconda3/lib/python3.8/site-packages/nltk/corpus/util.py in __load(self)
         83                     root = nltk.data.find("{}/{}".format(self.subdir, zip_name))
         84                 except LookupError:
    ---> 85                     raise e
         86 
         87         # Load the corpus.


    /opt/anaconda3/lib/python3.8/site-packages/nltk/corpus/util.py in __load(self)
         78         else:
         79             try:
    ---> 80                 root = nltk.data.find("{}/{}".format(self.subdir, self.__name))
         81             except LookupError as e:
         82                 try:


    /opt/anaconda3/lib/python3.8/site-packages/nltk/data.py in find(resource_name, paths)
        583     sep = "*" * 70
        584     resource_not_found = "\n%s\n%s\n%s\n" % (sep, msg, sep)
    --> 585     raise LookupError(resource_not_found)
        586 
        587 


    LookupError: 
    **********************************************************************
      Resource stopwords not found.
      Please use the NLTK Downloader to obtain the resource:
    
      >>> import nltk
      >>> nltk.download('stopwords')
      
      For more information see: https://www.nltk.org/data.html
    
      Attempted to load corpora/stopwords
    
      Searched in:
        - '/Users/melissavang/nltk_data'
        - '/opt/anaconda3/nltk_data'
        - '/opt/anaconda3/share/nltk_data'
        - '/opt/anaconda3/lib/nltk_data'
        - '/usr/share/nltk_data'
        - '/usr/local/share/nltk_data'
        - '/usr/lib/nltk_data'
        - '/usr/local/lib/nltk_data'
    **********************************************************************




```python
# Import Dataset
df = pd.read_json('https://github.com/vamel19/INFO5731_FALL2020/blob/master/Titles.csv')
print(df.target_names.unique())
df.head()
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-23-7cdba92eaef1> in <module>
          1 # Import Dataset
    ----> 2 df = pd.read_json('https://github.com/vamel19/INFO5731_FALL2020/blob/master/Titles.csv')
          3 print(df.target_names.unique())
          4 df.head()


    /opt/anaconda3/lib/python3.8/site-packages/pandas/util/_decorators.py in wrapper(*args, **kwargs)
        212                 else:
        213                     kwargs[new_arg_name] = new_arg_value
    --> 214             return func(*args, **kwargs)
        215 
        216         return cast(F, wrapper)


    /opt/anaconda3/lib/python3.8/site-packages/pandas/io/json/_json.py in read_json(path_or_buf, orient, typ, dtype, convert_axes, convert_dates, keep_default_dates, numpy, precise_float, date_unit, encoding, lines, chunksize, compression)
        606         return json_reader
        607 
    --> 608     result = json_reader.read()
        609     if should_close:
        610         filepath_or_buffer.close()


    /opt/anaconda3/lib/python3.8/site-packages/pandas/io/json/_json.py in read(self)
        729             obj = self._get_object_parser(self._combine_lines(data.split("\n")))
        730         else:
    --> 731             obj = self._get_object_parser(self.data)
        732         self.close()
        733         return obj


    /opt/anaconda3/lib/python3.8/site-packages/pandas/io/json/_json.py in _get_object_parser(self, json)
        751         obj = None
        752         if typ == "frame":
    --> 753             obj = FrameParser(json, **kwargs).parse()
        754 
        755         if typ == "series" or obj is None:


    /opt/anaconda3/lib/python3.8/site-packages/pandas/io/json/_json.py in parse(self)
        855 
        856         else:
    --> 857             self._parse_no_numpy()
        858 
        859         if self.obj is None:


    /opt/anaconda3/lib/python3.8/site-packages/pandas/io/json/_json.py in _parse_no_numpy(self)
       1087         if orient == "columns":
       1088             self.obj = DataFrame(
    -> 1089                 loads(json, precise_float=self.precise_float), dtype=None
       1090             )
       1091         elif orient == "split":


    ValueError: Expected object or value


## (2) (8 points) Generate K topics by using LSA, the number of topics K should be decided by the coherence score, then summarize what are the topics. You may refer the code here:

https://www.datacamp.com/community/tutorials/discovering-hidden-topics-python


```python
#import modules
import os.path
from gensim import corpora
from gensim.models import LsiModel
from nltk.tokenize import RegexpTokenizer
from nltk.corpus import stopwords
from nltk.stem.porter import PorterStemmer
from gensim.models.coherencemodel import CoherenceModel
import matplotlib.pyplot as plt
```


    ---------------------------------------------------------------------------

    ModuleNotFoundError                       Traceback (most recent call last)

    <ipython-input-16-da5e4e592a4f> in <module>
          1 #import modules
          2 import os.path
    ----> 3 from gensim import corpora
          4 from gensim.models import LsiModel
          5 from nltk.tokenize import RegexpTokenizer


    ModuleNotFoundError: No module named 'gensim'



```python
def load_data(path,file_name):
    """
    Input  : path and file_name
    Purpose: loading text file
    Output : list of paragraphs/documents and
             title(initial 100 words considred as title of document)
    """
    documents_list = []
    titles=[]
    with open( os.path.join(path, file_name) ,"r") as fin:
        for line in fin.readlines():
            text = line.strip()
            documents_list.append(text)
    print("Total Number of Documents:",len(documents_list))
    titles.append( text[0:min(len(text),100)] )
    return documents_list,titles
```


```python
def preprocess_data(doc_set):
    """
    Input  : docuemnt list
    Purpose: preprocess text (tokenize, removing stopwords, and stemming)
    Output : preprocessed text
    """
    # initialize regex tokenizer
    tokenizer = RegexpTokenizer(r'\w+')
    # create English stop words list
    en_stop = set(stopwords.words('english'))
    # Create p_stemmer of class PorterStemmer
    p_stemmer = PorterStemmer()
    # list for tokenized documents in loop
    texts = []
    # loop through document list
    for i in doc_set:
        # clean and tokenize document string
        raw = i.lower()
        tokens = tokenizer.tokenize(raw)
        # remove stop words from tokens
        stopped_tokens = [i for i in tokens if not i in en_stop]
        # stem tokens
        stemmed_tokens = [p_stemmer.stem(i) for i in stopped_tokens]
        # add tokens to list
        texts.append(stemmed_tokens)
    return texts
```


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
start,stop,step=2,12,1
plot_graph(clean_text,start,stop,step)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-14-523db9e36454> in <module>
          1 start,stop,step=2,12,1
    ----> 2 plot_graph(clean_text,start,stop,step)
    

    NameError: name 'clean_text' is not defined


## (3) (4 points) Compare the results generated by the two topic modeling algorithms, which one is better? You should explain the reasons in details.


```python
Comparing the results generated by the two topic modeling algorithms, both are used to compare classification of the titles. LSA has a lower accuracy.
```
