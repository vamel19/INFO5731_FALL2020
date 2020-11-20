# In class exercise 10

The purpose of the exercise is to practice different machine learning algorithms for text clustering
Please downlad the dataset by using the following link.  https://www.kaggle.com/PromptCloudHQ/amazon-reviews-unlocked-mobile-phones
(You can also use different text data which you want)

Apply the listed clustering methods to the dataset:

K means, 
DBSCAN,
Hierarchical clustering. 

You can refer to of the codes from  the follwing link below. 
https://www.kaggle.com/karthik3890/text-clustering 
    

In one paragraph, please compare K means, DBSCAN and Hierarchical clustering. 


```python
import pandas as pd

df = pd.read_csv('Amazon_Unlocked_Mobile.csv')
df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 413840 entries, 0 to 413839
    Data columns (total 6 columns):
     #   Column        Non-Null Count   Dtype  
    ---  ------        --------------   -----  
     0   Product Name  413840 non-null  object 
     1   Brand Name    348669 non-null  object 
     2   Price         407907 non-null  float64
     3   Rating        413840 non-null  int64  
     4   Reviews       413778 non-null  object 
     5   Review Votes  401544 non-null  float64
    dtypes: float64(2), int64(1), object(3)
    memory usage: 18.9+ MB



```python
df.head()
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
      <th>Product Name</th>
      <th>Brand Name</th>
      <th>Price</th>
      <th>Rating</th>
      <th>Reviews</th>
      <th>Review Votes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>"CLEAR CLEAN ESN" Sprint EPIC 4G Galaxy SPH-D7...</td>
      <td>Samsung</td>
      <td>199.99</td>
      <td>5</td>
      <td>I feel so LUCKY to have found this used (phone...</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>"CLEAR CLEAN ESN" Sprint EPIC 4G Galaxy SPH-D7...</td>
      <td>Samsung</td>
      <td>199.99</td>
      <td>4</td>
      <td>nice phone, nice up grade from my pantach revu...</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>"CLEAR CLEAN ESN" Sprint EPIC 4G Galaxy SPH-D7...</td>
      <td>Samsung</td>
      <td>199.99</td>
      <td>5</td>
      <td>Very pleased</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>"CLEAR CLEAN ESN" Sprint EPIC 4G Galaxy SPH-D7...</td>
      <td>Samsung</td>
      <td>199.99</td>
      <td>4</td>
      <td>It works good but it goes slow sometimes but i...</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>"CLEAR CLEAN ESN" Sprint EPIC 4G Galaxy SPH-D7...</td>
      <td>Samsung</td>
      <td>199.99</td>
      <td>4</td>
      <td>Great phone to replace my lost phone. The only...</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
df = df[['title', 'tagline', 'overview', 'genres', 'popularity']]
df.tagline.fillna('', inplace=True)
df['description'] = df['tagline'].map(str) + ' ' + df['overview']
df.dropna(inplace=True)
df.info()
```


    ---------------------------------------------------------------------------

    KeyError                                  Traceback (most recent call last)

    <ipython-input-7-20e777f62d58> in <module>
    ----> 1 df = df[['title', 'tagline', 'overview', 'genres', 'popularity']]
          2 df.tagline.fillna('', inplace=True)
          3 df['description'] = df['tagline'].map(str) + ' ' + df['overview']
          4 df.dropna(inplace=True)
          5 df.info()


    /opt/anaconda3/lib/python3.8/site-packages/pandas/core/frame.py in __getitem__(self, key)
       2804             if is_iterator(key):
       2805                 key = list(key)
    -> 2806             indexer = self.loc._get_listlike_indexer(key, axis=1, raise_missing=True)[1]
       2807 
       2808         # take() does not accept boolean indexers


    /opt/anaconda3/lib/python3.8/site-packages/pandas/core/indexing.py in _get_listlike_indexer(self, key, axis, raise_missing)
       1550             keyarr, indexer, new_indexer = ax._reindex_non_unique(keyarr)
       1551 
    -> 1552         self._validate_read_indexer(
       1553             keyarr, indexer, o._get_axis_number(axis), raise_missing=raise_missing
       1554         )


    /opt/anaconda3/lib/python3.8/site-packages/pandas/core/indexing.py in _validate_read_indexer(self, key, indexer, axis, raise_missing)
       1638             if missing == len(indexer):
       1639                 axis_name = self.obj._get_axis_name(axis)
    -> 1640                 raise KeyError(f"None of [{key}] are in the [{axis_name}]")
       1641 
       1642             # We (temporarily) allow for some missing keys with .loc, except in


    KeyError: "None of [Index(['title', 'tagline', 'overview', 'genres', 'popularity'], dtype='object')] are in the [columns]"



```python
df.head()
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
      <th>Product Name</th>
      <th>Brand Name</th>
      <th>Price</th>
      <th>Rating</th>
      <th>Reviews</th>
      <th>Review Votes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>"CLEAR CLEAN ESN" Sprint EPIC 4G Galaxy SPH-D7...</td>
      <td>Samsung</td>
      <td>199.99</td>
      <td>5</td>
      <td>I feel so LUCKY to have found this used (phone...</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>"CLEAR CLEAN ESN" Sprint EPIC 4G Galaxy SPH-D7...</td>
      <td>Samsung</td>
      <td>199.99</td>
      <td>4</td>
      <td>nice phone, nice up grade from my pantach revu...</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>"CLEAR CLEAN ESN" Sprint EPIC 4G Galaxy SPH-D7...</td>
      <td>Samsung</td>
      <td>199.99</td>
      <td>5</td>
      <td>Very pleased</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>"CLEAR CLEAN ESN" Sprint EPIC 4G Galaxy SPH-D7...</td>
      <td>Samsung</td>
      <td>199.99</td>
      <td>4</td>
      <td>It works good but it goes slow sometimes but i...</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>"CLEAR CLEAN ESN" Sprint EPIC 4G Galaxy SPH-D7...</td>
      <td>Samsung</td>
      <td>199.99</td>
      <td>4</td>
      <td>Great phone to replace my lost phone. The only...</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
import pandas as pd
df = pd.read_csv('Amazon_Unlocked_Mobile.csv')
df.info()

wiki_lst=[]
title=[]
for article in articles:
   print("loading content: ",article)
   wiki_lst.append(wikipedia.page(article).content)
   title.append(article)
print("examine content")
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 413840 entries, 0 to 413839
    Data columns (total 6 columns):
     #   Column        Non-Null Count   Dtype  
    ---  ------        --------------   -----  
     0   Product Name  413840 non-null  object 
     1   Brand Name    348669 non-null  object 
     2   Price         407907 non-null  float64
     3   Rating        413840 non-null  int64  
     4   Reviews       413778 non-null  object 
     5   Review Votes  401544 non-null  float64
    dtypes: float64(2), int64(1), object(3)
    memory usage: 18.9+ MB



    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-21-d526331eb53f> in <module>
          5 wiki_lst=[]
          6 title=[]
    ----> 7 for article in articles:
          8    print("loading content: ",article)
          9    wiki_lst.append(wikipedia.page(article).content)


    NameError: name 'articles' is not defined



```python
from sklearn.feature_extraction.text import TfidfVectorizer
vectorizer = TfidfVectorizer(stop_words={'english'})
X = vectorizer.fit_transform(wiki_lst)
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-24-1c99f04042de> in <module>
          1 from sklearn.feature_extraction.text import TfidfVectorizer
          2 vectorizer = TfidfVectorizer(stop_words={'english'})
    ----> 3 X = vectorizer.fit_transform(wiki_lst)
    

    /opt/anaconda3/lib/python3.8/site-packages/sklearn/feature_extraction/text.py in fit_transform(self, raw_documents, y)
       1838         """
       1839         self._check_params()
    -> 1840         X = super().fit_transform(raw_documents)
       1841         self._tfidf.fit(X)
       1842         # X is already a transformed view of raw_documents so


    /opt/anaconda3/lib/python3.8/site-packages/sklearn/feature_extraction/text.py in fit_transform(self, raw_documents, y)
       1196         max_features = self.max_features
       1197 
    -> 1198         vocabulary, X = self._count_vocab(raw_documents,
       1199                                           self.fixed_vocabulary_)
       1200 


    /opt/anaconda3/lib/python3.8/site-packages/sklearn/feature_extraction/text.py in _count_vocab(self, raw_documents, fixed_vocab)
       1127             vocabulary = dict(vocabulary)
       1128             if not vocabulary:
    -> 1129                 raise ValueError("empty vocabulary; perhaps the documents only"
       1130                                  " contain stop words")
       1131 


    ValueError: empty vocabulary; perhaps the documents only contain stop words



```python
import matplotlib.pyplot as plt
from sklearn.cluster import KMeans
Sum_of_squared_distances = []
K = range(2,10)
for k in K:
   km = KMeans(n_clusters=k, max_iter=200, n_init=10)
   km = km.fit(X)
   Sum_of_squared_distances.append(km.inertia_)
plt.plot(K, Sum_of_squared_distances, 'bx-')
plt.xlabel('k')
plt.ylabel('Sum_of_squared_distances')
plt.title('Amazon unlocked mobile')
plt.show()
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-25-27be90c45953> in <module>
          5 for k in K:
          6    km = KMeans(n_clusters=k, max_iter=200, n_init=10)
    ----> 7    km = km.fit(X)
          8    Sum_of_squared_distances.append(km.inertia_)
          9 plt.plot(K, Sum_of_squared_distances, 'bx-')


    NameError: name 'X' is not defined



```python
true_k = 6
model = KMeans(n_clusters=true_k, init='k-means++', max_iter=200, n_init=10)
model.fit(X)
labels=model.labels_
wiki_cl=pd.DataFrame(list(zip(title,labels)),columns=['title','cluster'])
print(wiki_cl.sort_values(by=['cluster']))
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-26-50e0599e04bf> in <module>
          1 true_k = 6
          2 model = KMeans(n_clusters=true_k, init='k-means++', max_iter=200, n_init=10)
    ----> 3 model.fit(X)
          4 labels=model.labels_
          5 wiki_cl=pd.DataFrame(list(zip(title,labels)),columns=['title','cluster'])


    NameError: name 'X' is not defined



```python
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import sklearn.cluster as cluster
import time
%matplotlib inline
sns.set_context('poster')
sns.set_color_codes()
plot_kwds = {'alpha' : 0.25, 's' : 80, 'linewidths':0}
```


```python
data = np.load('Amazon_Unlocked_Mobile.csv')
```


    ---------------------------------------------------------------------------

    ValueError                                Traceback (most recent call last)

    <ipython-input-36-ad68493712be> in <module>
    ----> 1 data = np.load('Amazon_Unlocked_Mobile.csv')
    

    /opt/anaconda3/lib/python3.8/site-packages/numpy/lib/npyio.py in load(file, mmap_mode, allow_pickle, fix_imports, encoding)
        455             # Try a pickle
        456             if not allow_pickle:
    --> 457                 raise ValueError("Cannot load file containing pickled data "
        458                                  "when allow_pickle=False")
        459             try:


    ValueError: Cannot load file containing pickled data when allow_pickle=False



```python
plt.figure(figsize=(10,6))
plt.scatter(X[:,0], X[:,1], c=y, cmap='Paired')
```




    <matplotlib.collections.PathCollection at 0x1187db220>




![png](output_13_1.png)



```python
plt.scatter(data.T[0], data.T[1], c='b', **plot_kwds)
frame = plt.gca()
frame.axes.get_xaxis().set_visible(False)
frame.axes.get_yaxis().set_visible(False)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-38-9cfe3354170e> in <module>
    ----> 1 plt.scatter(data.T[0], data.T[1], c='b', **plot_kwds)
          2 frame = plt.gca()
          3 frame.axes.get_xaxis().set_visible(False)
          4 frame.axes.get_yaxis().set_visible(False)


    NameError: name 'data' is not defined



```python
    start_time = time.time()
    labels = algorithm(*args, **kwds).fit_predict(data)
    end_time = time.time()
    palette = sns.color_palette('deep', np.unique(labels).max() + 1)
    colors = [palette[x] if x >= 0 else (0.0, 0.0, 0.0) for x in labels]
    plt.scatter(data.T[0], data.T[1], c=colors, **plot_kwds)
    frame = plt.gca()
    frame.axes.get_xaxis().set_visible(False)
    frame.axes.get_yaxis().set_visible(False)
    plt.title('Clusters found by {}'.format(str(algorithm.__name__)), fontsize=24)
    plt.text(-0.5, 0.7, 'Clustering took {:.2f} s'.format(end_time - start_time), fontsize=14)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-39-b46d1d47d6fe> in <module>
          1 start_time = time.time()
    ----> 2 labels = algorithm(*args, **kwds).fit_predict(data)
          3 end_time = time.time()
          4 palette = sns.color_palette('deep', np.unique(labels).max() + 1)
          5 colors = [palette[x] if x >= 0 else (0.0, 0.0, 0.0) for x in labels]


    NameError: name 'algorithm' is not defined



```python
from scipy.cluster.hierarchy import ward, dendrogram
from sklearn.metrics.pairwise import cosine_similarity
```


```python
def ward_hierarchical_clustering(feature_matrix):
    
    cosine_distance = 1 - cosine_similarity(feature_matrix)
    linkage_matrix = ward(cosine_distance)
    return linkage_matrix
```


```python
def plot_hierarchical_clusters(linkage_matrix, amazon_data, p=100, figure_size=(8,12)):
    # set size
    fig, ax = plt.subplots(figsize=figure_size) 
    movie_titles = amazon_data['title'].values.tolist()
    # plot dendrogram
    R = dendrogram(linkage_matrix, orientation="left", labels=movie_titles,
                    truncate_mode='lastp', 
                    p=p,  
                    no_plot=True)
    temp = {R["leaves"][ii]: movie_titles[ii] for ii in range(len(R["leaves"]))}
    def llf(xx):
        return "{}".format(temp[xx])
    ax = dendrogram(
            linkage_matrix,
            truncate_mode='lastp',
            orientation="left",
            p=p,  
            leaf_label_func=llf, 
            leaf_font_size=10.,
            )
    plt.tick_params(axis= 'x',   
                    which='both',  
                    bottom='off',
                    top='off',
                    labelbottom='off')
    plt.tight_layout()
```


```python
linkage_matrix = ward_hierarchical_clustering(cv_matrix)
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-54-200e4dd2a0bb> in <module>
    ----> 1 linkage_matrix = ward_hierarchical_clustering(cv_matrix)
    

    NameError: name 'cv_matrix' is not defined



```python
plot_hierarchical_clusters(linkage_matrix,
                           p=100,
                           amazon_data=df,
                           figure_size=(12, 14))
```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-55-62d56cc3510b> in <module>
    ----> 1 plot_hierarchical_clusters(linkage_matrix,
          2                            p=100,
          3                            amazon_data=df,
          4                            figure_size=(12, 14))


    NameError: name 'linkage_matrix' is not defined



```python
K means shows the number of clusters in a diagram, and DBSCAN forms clusters of dense areas in a diagram. Also, hierarchical clustering shows clusters that merge together and displays the hierarchical relationship between the clusters in a diagram. 
```
