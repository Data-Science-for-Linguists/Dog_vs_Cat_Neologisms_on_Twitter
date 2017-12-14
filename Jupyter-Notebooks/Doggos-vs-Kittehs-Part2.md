
# Doggos-vs-Kittehs-Part 2  
Margaret Jones (mmj32)  
Univeristy of Pittsburgh  
Data Science for Linguists  

## Table of Contents  
- [Getting the data](#pulling-in-the-data)  
- [A Quick Review](#a-quick-review)  
- [Comparison by Hashtag](#let's-look-specifically-at-hashtag)

# Pulling in the data


```python
#First, The Imports
import tweepy
import pandas as pd
import matplotlib.pyplot as plt
from pprint import pprint

# Every returned Out[] is displayed, not just the last one. (Thanks NaRae!)
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
```


```python
# filepath for the saved csv files
filepath = "/Users/Margaret/Documents/Data_Science/Project_Margaret/"
```


```python
#read in old csv files
tcat_df = pd.read_csv(filepath + 'cat_tweets.csv')
tdog_df = pd.read_csv(filepath + 'dog_tweets.csv')
```


```python
tcat_df.size
tdog_df.size
```




    79185






    197595



# A quick review


```python
#let's take a closer look at that, shall we?
#getting sums
cat_fav_total = tcat_df['tweetFavoriteCt'].sum()
cat_rt_total = tcat_df['tweetRetweetCt'].sum()

dog_fav_total = tdog_df['tweetFavoriteCt'].sum()
dog_rt_total = tdog_df['tweetRetweetCt'].sum()
```


```python
# okay, now for the bar graph, again
fav = pd.DataFrame(columns = ['CATS', 'DOGS'])
rt = pd.DataFrame(columns = ['CATS', 'DOGS'])
fav.set_value('Favorites', 'CATS', cat_fav_total)
rt.set_value('Retweets', 'CATS', cat_rt_total)
fav.set_value('Favorites', 'DOGS', dog_fav_total)
rt.set_value('Retweets', 'DOGS', dog_rt_total)

fav.plot.bar(figsize=(4,4))
plt.show()
rt.plot.bar(figsize=(4,4))
plt.show()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CATS</th>
      <th>DOGS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Favorites</th>
      <td>3838</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>






<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CATS</th>
      <th>DOGS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Retweets</th>
      <td>12010831</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>






<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CATS</th>
      <th>DOGS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Favorites</th>
      <td>3838</td>
      <td>10625</td>
    </tr>
  </tbody>
</table>
</div>






<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CATS</th>
      <th>DOGS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Retweets</th>
      <td>12010831</td>
      <td>564744</td>
    </tr>
  </tbody>
</table>
</div>






    <matplotlib.axes._subplots.AxesSubplot at 0x10eaf53c8>




![png](output_9_5.png)





    <matplotlib.axes._subplots.AxesSubplot at 0x10d778780>




![png](output_9_7.png)


# Let's look specifically at hashtag

### Cats first


```python
hash_cat = tcat_df[['Hashtag','tweetRetweetCt', 'tweetFavoriteCt']]
hash_cat.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Hashtag</th>
      <th>tweetRetweetCt</th>
      <th>tweetFavoriteCt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>cat</td>
      <td>5</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>cat</td>
      <td>12</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>cat</td>
      <td>6</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>cat</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>cat</td>
      <td>5</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
## COUNTING THE NUMBER OF RETWEETS PER CAT HASHTAG 
#separating out #cat
i = 0
hcat_rt = 0
for v in hash_cat['Hashtag']:
    if v == 'cat':
        hcat_rt += hash_cat['tweetRetweetCt'][i]
    i +=1
hcat_rt

#separating out #cats
i = 0
hcats_rt = 0
for v in hash_cat['Hashtag']:
    if v == 'cats':
        hcats_rt += hash_cat['tweetRetweetCt'][i]
    i +=1
hcats_rt

#separating out #kitteh
i = 0
hkitteh_rt = 0
for v in hash_cat['Hashtag']:
    if v == 'kitteh':
        hkitteh_rt += hash_cat['tweetRetweetCt'][i]
    i +=1
hkitteh_rt

#separating out #toebeans
i = 0
htoebeans_rt = 0
for v in hash_cat['Hashtag']:
    if v == 'toebeans':
        htoebeans_rt += hash_cat['tweetRetweetCt'][i]
    i +=1
htoebeans_rt
```




    12000759






    7409






    1330






    1333




```python
## COUNTING THE NUMBER OF FAVORITES PER CAT HASHTAG 
#separating out #cat
i = 0
hcat_fav = 0
for v in hash_cat['Hashtag']:
    if v == 'cat':
        hcat_fav += hash_cat['tweetFavoriteCt'][i]
    i +=1
hcat_fav

#separating out #cats
i = 0
hcats_fav = 0
for v in hash_cat['Hashtag']:
    if v == 'cats':
        hcats_fav += hash_cat['tweetFavoriteCt'][i]
    i +=1
hcats_fav

#separating out #kitteh
i = 0
hkitteh_fav = 0
for v in hash_cat['Hashtag']:
    if v == 'kitteh':
        hkitteh_fav += hash_cat['tweetFavoriteCt'][i]
    i +=1
hkitteh_fav

#separating out #toebeans
i = 0
htoebeans_fav = 0
for v in hash_cat['Hashtag']:
    if v == 'toebeans':
        htoebeans_fav += hash_cat['tweetFavoriteCt'][i]
    i +=1
htoebeans_fav
```




    2278






    55






    361






    1144



### Dogs


```python
hash_dog = tdog_df[['Hashtag','tweetRetweetCt', 'tweetFavoriteCt']]
hash_dog.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Hashtag</th>
      <th>tweetRetweetCt</th>
      <th>tweetFavoriteCt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>dog</td>
      <td>30</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>dog</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>dog</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>dog</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>dog</td>
      <td>0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
## COUNTING THE NUMBER OF RETWEETS PER DOG HASHTAG 
#separating out #dog
i = 0
hdog_rt = 0
for v in hash_dog['Hashtag']:
    if v == 'dog':
        hdog_rt += hash_dog['tweetRetweetCt'][i]
    i +=1
hdog_rt

#separating out #dogs
i = 0
hdogs_rt = 0
for v in hash_dog['Hashtag']:
    if v == 'dogs':
        hdogs_rt += hash_dog['tweetRetweetCt'][i]
    i +=1
hdogs_rt

#separating out #doge
i = 0
hdoge_rt = 0
for v in hash_dog['Hashtag']:
    if v == 'doge':
        hdoge_rt += hash_dog['tweetRetweetCt'][i]
    i +=1
hdoge_rt

#separating out #dogs
i = 0
hdoggo_rt = 0
for v in hash_dog['Hashtag']:
    if v == 'doggo':
        hdoggo_rt += hash_dog['tweetRetweetCt'][i]
    i +=1
hdoggo_rt
```




    139489






    44477






    369994






    10784




```python
## COUNTING THE NUMBER OF RETWEETS PER DOG HASHTAG 
#separating out #dog
i = 0
hdog_fav = 0
for v in hash_dog['Hashtag']:
    if v == 'dog':
        hdog_fav += hash_dog['tweetFavoriteCt'][i]
    i +=1
hdog_fav

#separating out #dogs
i = 0
hdogs_fav = 0
for v in hash_dog['Hashtag']:
    if v == 'dogs':
        hdogs_fav += hash_dog['tweetFavoriteCt'][i]
    i +=1
hdogs_fav

#separating out #doge
i = 0
hdoge_fav = 0
for v in hash_dog['Hashtag']:
    if v == 'doge':
        hdoge_fav += hash_dog['tweetFavoriteCt'][i]
    i +=1
hdoge_fav

#separating out #dogs
i = 0
hdoggo_fav = 0
for v in hash_dog['Hashtag']:
    if v == 'doggo':
        hdoggo_fav += hash_dog['tweetFavoriteCt'][i]
    i +=1
hdoggo_fav
```




    2365






    567






    2143






    5550



### Some more exploding pie charts


```python
# Pie chart, where the slices will be ordered and plotted counter-clockwise:
labels = ['cat','cats', 'kitteh', 'toebeans', 'dog', 'dogs', 'doggo', 'doge']
sizes = [hcat_rt, hcats_rt, hkitteh_rt, htoebeans_rt, hdog_rt, hdogs_rt, hdoggo_rt, hdoge_rt ]
explode = (0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5, 0.5)

fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%',
        shadow=True, startangle=90)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.show()
```




    ([<matplotlib.patches.Wedge at 0x10d801588>,
      <matplotlib.patches.Wedge at 0x10da3ab00>,
      <matplotlib.patches.Wedge at 0x10da4b400>,
      <matplotlib.patches.Wedge at 0x10da53cc0>,
      <matplotlib.patches.Wedge at 0x10da635c0>,
      <matplotlib.patches.Wedge at 0x10d9b2f98>,
      <matplotlib.patches.Wedge at 0x10d873ba8>,
      <matplotlib.patches.Wedge at 0x10da81080>],
     [<matplotlib.text.Text at 0x10da32fd0>,
      <matplotlib.text.Text at 0x10da428d0>,
      <matplotlib.text.Text at 0x10da531d0>,
      <matplotlib.text.Text at 0x10da5ba90>,
      <matplotlib.text.Text at 0x10da6c390>,
      <matplotlib.text.Text at 0x10d781a58>,
      <matplotlib.text.Text at 0x10da75550>,
      <matplotlib.text.Text at 0x10da81e10>],
     [<matplotlib.text.Text at 0x10da3a588>,
      <matplotlib.text.Text at 0x10da42e48>,
      <matplotlib.text.Text at 0x10da53748>,
      <matplotlib.text.Text at 0x10da63048>,
      <matplotlib.text.Text at 0x10da6c908>,
      <matplotlib.text.Text at 0x10d8e8278>,
      <matplotlib.text.Text at 0x10da75ac8>,
      <matplotlib.text.Text at 0x10da893c8>])






    (-1.1786447740658623,
     1.0363378674045489,
     -1.6525072617473355,
     1.64788342993279)




![png](output_19_2.png)


### okay so my means of retweets, cat is the most populat hashtag by far. But let's remove it and see how the others distribute...


```python
# Pie chart, where the slices will be ordered and plotted counter-clockwise:
labels = ['cats', 'kitteh', 'toebeans', 'dog', 'dogs', 'doggo', 'doge']
sizes = [hcats_rt, hkitteh_rt, htoebeans_rt, hdog_rt, hdogs_rt, hdoggo_rt, hdoge_rt ]
explode = (0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1)

fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%',
        shadow=True, startangle=90)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.show()
```




    ([<matplotlib.patches.Wedge at 0x10da9f278>,
      <matplotlib.patches.Wedge at 0x10df904a8>,
      <matplotlib.patches.Wedge at 0x10df97d68>,
      <matplotlib.patches.Wedge at 0x10dfaa668>,
      <matplotlib.patches.Wedge at 0x10dfb3ef0>,
      <matplotlib.patches.Wedge at 0x10dfc27f0>,
      <matplotlib.patches.Wedge at 0x10dfd40f0>],
     [<matplotlib.text.Text at 0x10df88978>,
      <matplotlib.text.Text at 0x10df97278>,
      <matplotlib.text.Text at 0x10dfa1b38>,
      <matplotlib.text.Text at 0x10dfb3400>,
      <matplotlib.text.Text at 0x10dfb9cc0>,
      <matplotlib.text.Text at 0x10dfcc5c0>,
      <matplotlib.text.Text at 0x10dfd4e80>],
     [<matplotlib.text.Text at 0x10df88ef0>,
      <matplotlib.text.Text at 0x10df977f0>,
      <matplotlib.text.Text at 0x10dfaa0f0>,
      <matplotlib.text.Text at 0x10dfb3978>,
      <matplotlib.text.Text at 0x10dfc2278>,
      <matplotlib.text.Text at 0x10dfccb38>,
      <matplotlib.text.Text at 0x10dfdb438>])






    (-1.2028207684297623,
     1.2072394698045146,
     -1.1638741642331603,
     1.2077176549369624)




![png](output_21_2.png)


### So other than #cat it seems like the dog hashtags retweet more than the cat tweets  
#### let's break this down further


```python
# Pie chart, where the slices will be ordered and plotted counter-clockwise:
labels = ['cats', 'kitteh', 'toebeans']
sizes = [hcats_rt, hkitteh_rt, htoebeans_rt]
explode = (0.1, 0.1, 0.1)

fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%',
        shadow=True, startangle=90)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.show()

# Pie chart, where the slices will be ordered and plotted counter-clockwise:
labels = ['dog', 'dogs', 'doggo', 'doge']
sizes = [hdog_rt, hdogs_rt, hdoggo_rt, hdoge_rt ]
explode = (0.1, 0.1, 0.1, 0.1)

fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%',
        shadow=True, startangle=90)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.show()
```




    ([<matplotlib.patches.Wedge at 0x10dabc940>,
      <matplotlib.patches.Wedge at 0x10e0e7940>,
      <matplotlib.patches.Wedge at 0x10e0f8240>],
     [<matplotlib.text.Text at 0x10e0dde10>,
      <matplotlib.text.Text at 0x10e0f0710>,
      <matplotlib.text.Text at 0x10e0f8f60>],
     [<matplotlib.text.Text at 0x10e0e73c8>,
      <matplotlib.text.Text at 0x10e0f0c88>,
      <matplotlib.text.Text at 0x10e0fe518>])






    (-1.2017941089200854,
     1.212995256394122,
     -1.192933519563907,
     1.2002617254366041)




![png](output_23_2.png)





    ([<matplotlib.patches.Wedge at 0x10e015668>,
      <matplotlib.patches.Wedge at 0x10e17c7f0>,
      <matplotlib.patches.Wedge at 0x10e3d50f0>,
      <matplotlib.patches.Wedge at 0x10e3dc9b0>],
     [<matplotlib.text.Text at 0x10e174cc0>,
      <matplotlib.text.Text at 0x10e1825c0>,
      <matplotlib.text.Text at 0x10e3d5e80>,
      <matplotlib.text.Text at 0x10e3e5780>],
     [<matplotlib.text.Text at 0x10e17c278>,
      <matplotlib.text.Text at 0x10e182b38>,
      <matplotlib.text.Text at 0x10e3dc438>,
      <matplotlib.text.Text at 0x10e3e5cf8>])






    (-1.208352854211431,
     1.2022360857818759,
     -1.1610208483697912,
     1.1776796064295183)




![png](output_23_5.png)


### So for cat retweets (excluding #cat) kitteh and toebeans seem to share the same percentage, and then cats is the second most popular  
### For dogs, it is ordered as such: #doge, #dog, #dogs, #doggo

## Let's do the same thing but with favorites


```python
# Pie chart, where the slices will be ordered and plotted counter-clockwise:
labels = ['cat','cats', 'kitteh', 'toebeans', 'dog', 'dogs', 'doggo', 'doge']
sizes = [hcat_fav, hcats_fav, hkitteh_fav, htoebeans_fav, hdog_fav, hdogs_fav, hdoggo_fav, hdoge_fav]
explode = (0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1, 0.1)

fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%',
        shadow=True, startangle=90)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.show()
```




    ([<matplotlib.patches.Wedge at 0x10dac23c8>,
      <matplotlib.patches.Wedge at 0x10e4fd7f0>,
      <matplotlib.patches.Wedge at 0x10e50f0f0>,
      <matplotlib.patches.Wedge at 0x10e5159b0>,
      <matplotlib.patches.Wedge at 0x10e5262b0>,
      <matplotlib.patches.Wedge at 0x10e52fb70>,
      <matplotlib.patches.Wedge at 0x10e53e470>,
      <matplotlib.patches.Wedge at 0x10e546d30>],
     [<matplotlib.text.Text at 0x10e4f6cc0>,
      <matplotlib.text.Text at 0x10e5055c0>,
      <matplotlib.text.Text at 0x10e50fe80>,
      <matplotlib.text.Text at 0x10e51e780>,
      <matplotlib.text.Text at 0x10e52f080>,
      <matplotlib.text.Text at 0x10e539940>,
      <matplotlib.text.Text at 0x10e546240>,
      <matplotlib.text.Text at 0x10e550b00>],
     [<matplotlib.text.Text at 0x10e4fd278>,
      <matplotlib.text.Text at 0x10e505b38>,
      <matplotlib.text.Text at 0x10e515438>,
      <matplotlib.text.Text at 0x10e51ecf8>,
      <matplotlib.text.Text at 0x10e52f5f8>,
      <matplotlib.text.Text at 0x10e539eb8>,
      <matplotlib.text.Text at 0x10e5467b8>,
      <matplotlib.text.Text at 0x10e5590b8>])






    (-1.2118564998714854,
     1.2008997432360842,
     -1.182983974057269,
     1.1975668696970021)




![png](output_26_2.png)


## For this is it pretty clear that the dogs get more favorites 

### Only cats


```python
# Pie chart, where the slices will be ordered and plotted counter-clockwise:
labels = ['cat','cats', 'kitteh', 'toebeans']
sizes = [hcat_fav, hcats_fav, hkitteh_fav, htoebeans_fav]
explode = (0.1, 0.1, 0.1, 0.1)

fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%',
        shadow=True, startangle=90)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.show()
```




    ([<matplotlib.patches.Wedge at 0x10dac2390>,
      <matplotlib.patches.Wedge at 0x10e6691d0>,
      <matplotlib.patches.Wedge at 0x10e670a90>,
      <matplotlib.patches.Wedge at 0x10e682390>],
     [<matplotlib.text.Text at 0x10e5d26a0>,
      <matplotlib.text.Text at 0x10e669f60>,
      <matplotlib.text.Text at 0x10e67a860>,
      <matplotlib.text.Text at 0x10e689160>],
     [<matplotlib.text.Text at 0x10e5d2c18>,
      <matplotlib.text.Text at 0x10e670518>,
      <matplotlib.text.Text at 0x10e67add8>,
      <matplotlib.text.Text at 0x10e6896d8>])






    (-1.2176858409136184,
     1.2027205081133758,
     -1.1455016747390809,
     1.1642564890316611)




![png](output_28_2.png)


## Scale: #cat, #toebeans, #kitteh, #cats  

### Now, dogs


```python
# Pie chart, where the slices will be ordered and plotted counter-clockwise:
labels = ['dog', 'dogs', 'doggo', 'doge']
sizes = [hdog_fav, hdogs_fav, hdoggo_fav, hdoge_fav]
explode = (0.1, 0.1, 0.1, 0.1)

fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%',
        shadow=True, startangle=90)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.show()
```




    ([<matplotlib.patches.Wedge at 0x10e58f860>,
      <matplotlib.patches.Wedge at 0x10e3e5f98>,
      <matplotlib.patches.Wedge at 0x10e1200b8>,
      <matplotlib.patches.Wedge at 0x10e3ee198>],
     [<matplotlib.text.Text at 0x10e169198>,
      <matplotlib.text.Text at 0x10e120a90>,
      <matplotlib.text.Text at 0x10e404470>,
      <matplotlib.text.Text at 0x10e009e48>],
     [<matplotlib.text.Text at 0x10e169828>,
      <matplotlib.text.Text at 0x10e120cf8>,
      <matplotlib.text.Text at 0x10e3ee668>,
      <matplotlib.text.Text at 0x10e0109e8>])






    (-1.2068592869706243,
     1.1386614029051518,
     -1.2151528637608773,
     1.1899087015793579)




![png](output_30_2.png)


### Ordering: #doggo, #dog, #doge, #dogs


```python

```
