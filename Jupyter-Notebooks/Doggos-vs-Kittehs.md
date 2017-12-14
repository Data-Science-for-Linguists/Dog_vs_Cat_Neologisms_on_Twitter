
# Doggos-vs-Kittehs 
Margaret Jones (mmj32)  
Univeristy of Pittsburgh  
Data Science for Linguists  

# Table of Contents 
- [Data collection](#let's-get-that-data!)  
- [Data Frame Creation](#creating-data-frames)  
- [First Analysis - Favorites vs. Retweets](##let's-attempt-some-analysis.)  
- [Finding the Most Popular Places](#let's-look-at-location-and-find-the-most-popular-place-for-cat-posts-vs.-dog-posts)  
- [Where the Tweets are Coming From by Location](#but-let's-go-a-little-deeper)
- [Where the Tweets are Coming From by Time Zone](#moving-on-to-timezone)

## Let's Get That Data!


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
# the key variables
consumerKey = "XXXXXXX"
consumerSecret ="XXXXXXX"
```


```python
#authentication
auth = tweepy.OAuthHandler(consumerKey, consumerSecret)

#connecting to Twitter API
api = tweepy.API(auth)
```


```python
# this is helpful for later but I need to read in the .csv files so I can append them later
#tcat = pd.read_csv(r'cat_tweets.csv')
#tdog = pd.read_csv(r'dog_tweets.csv')
filepath = "/Users/Margaret/Documents/Data_Science/Project_Margaret/"
```


```python
#getting cat and dog search results
#NOTE: The dog neologisms I will compare: doggo and doge
#NOTE: The cat neologisms I will compare: kitteh and toebeans
rDog = api.search(q='%23dog')
rDogs = api.search(q='%23dogs')
rDoggo = api.search(q='%23doggo')
rDoge = api.search(q='%23doge')
rCat = api.search(q='%23cat')
rCats = api.search(q='%23cats')
rToeBeans = api.search(q='%23toebeans')
rKitteh = api.search(q='%23kitteh')
```


```python
#Get the first 1000 items based on the search query
for tweet in tweepy.Cursor(api.search, q='%23dog').items(1000):
    rDog.append(tweet)
for tweet in tweepy.Cursor(api.search, q='%23doggo').items(1000):
    rDoggo.append(tweet)
for tweet in tweepy.Cursor(api.search, q='%23doge').items(1000):
    rDoge.append(tweet)
for tweet in tweepy.Cursor(api.search, q='%23cat').items(1000):
    rCat.append(tweet)
for tweet in tweepy.Cursor(api.search, q='%23toebeans').items(1000):
    rToeBeans.append(tweet)
for tweet in tweepy.Cursor(api.search, q='%23kitteh').items(1000):
    rKitteh.append(tweet)
```


```python
#Adding in the plurals
for tweet in tweepy.Cursor(api.search, q='%23dogs').items(1000):
    rDogs.append(tweet)
for tweet in tweepy.Cursor(api.search, q='%23cats').items(1000):
    rCats.append(tweet)
```


```python
#Verify the number of items returned
len(rDog)
len(rDogs)
len(rDoge)
len(rDoggo)
len(rCat)
len(rCats)
len(rToeBeans)
len(rKitteh)
```




    113






    112






    110






    115






    109






    109






    74






    115



### Even already it is interesting to see that ToeBeans and Kitteh are already returning a lot less results. This likely means that they are fairly unpopular terms (at least to tag your tweet with) which is interesting.  
#### This also means that if this trend continues (which it has been up to this point), it is already clear that Dogs get more posts than cats.
  
### Below is an example of a tweet as well as some additional information about it. This is the only tweet I will show, in attempts to not go against the data-sharing agreement. However, this is what the data I'm working with looks like.


```python
#this is an example of a tweet
tweet1 = rDog[1] #get the data of the first tweet.

#then, print tweet. 
print(tweet1.text)

#other additional info about the tweet for reference
tweet1.lang                    #the tweet is in English 
tweet1.user.screen_name        #this is the username
tweet1.user.description        #self description of user by user posted on profile
tweet1.created_at              #date and time tweet was created
tweet1.id                      #Tweet ID #
tweet1.favorite_count          #how many likes the tweet received (in this case 2)
```

    Two lion pugs will make you smile - YouTube
    https://t.co/0URNJIoHnu
    #pugs #retweet #dogs #pug #follow #like #dog… https://t.co/7QSVWGH0zo





    'en'






    'garwar2010'






    'Please subscribe,like,comment,retweet and share pugs for life my youtube channel. I am a video creator I love pugs and other things that make me laugh'






    datetime.datetime(2017, 12, 14, 1, 28, 18)






    941117613140213761






    2




```python
# this is a function found here: http://blog.impiyush.com/2015/03/data-analysis-using-twitter-api-and.html
# I am using this to clean my data into a dataframe, it has been altered slightly (mostly deletion) for my project.
def toDataFrame(tweets):

    DataSet = pd.DataFrame()

    DataSet['tweetID'] = [tweet.id for tweet in tweets]
    DataSet['tweetRetweetCt'] = [tweet.retweet_count for tweet 
    in tweets]
    DataSet['tweetFavoriteCt'] = [tweet.favorite_count for tweet 
    in tweets]
    DataSet['tweetSource'] = [tweet.source for tweet in tweets]
    DataSet['tweetCreated'] = [tweet.created_at for tweet in tweets]


    DataSet['userID'] = [tweet.user.id for tweet in tweets]
    DataSet['userScreen'] = [tweet.user.screen_name for tweet 
    in tweets]
    DataSet['userName'] = [tweet.user.name for tweet in tweets]
    DataSet['userLocation'] = [tweet.user.location for tweet in tweets]
    DataSet['userTimezone'] = [tweet.user.time_zone for tweet 
    in tweets]

    return DataSet
```

## Creating Data Frames


```python
#Do this funciton on the tweets, then connect the dataframes
dog_df = toDataFrame(rDog)
dogs_df = toDataFrame(rDogs)
doge_df = toDataFrame(rDoge)
doggo_df = toDataFrame(rDoggo)
cat_df = toDataFrame(rCat)
cats_df = toDataFrame(rCats)
toebeans_df = toDataFrame(rToeBeans)
kitteh_df = toDataFrame(rKitteh)

#Adding a column to say which hashtag it came from
dog_df['Hashtag'] = "dog"
dogs_df['Hashtag'] = "dogs" 
doge_df['Hashtag'] = "doge"
doggo_df['Hashtag'] = "doggo"
cat_df['Hashtag'] = "cat"
cats_df['Hashtag'] = "cats"
toebeans_df['Hashtag'] = "toebeans"
kitteh_df['Hashtag'] = "kitteh"

# now to combine into 2 larger ones, overall cat related, overall dog related
tcat_df = pd.concat([cat_df, cats_df, toebeans_df, kitteh_df])
tdog_df = pd.concat([dog_df, dogs_df, doge_df, doggo_df])

```


```python
#read in old csv files
tcat_df = pd.read_csv(filepath + 'cat_tweets.csv')
tdog_df = pd.read_csv(filepath + 'dog_tweets.csv')
```


```python
#append the old and new csv
tcat_df = tcat_df.append(tcat_df)
tdog_df = tdog_df.append(tdog_df)
```


```python
# Write out the DF as a new CSV file
tcat_df.to_csv('cat_tweets.csv')
tdog_df.to_csv('dog_tweets.csv')
```


```python
tcat_df.size
tdog_df.size
```




    158370






    395190



## Let's attempt some analysis.  
- again not completely sure how much I can share, so for now I'm only going to use the most recently gathered data. (The only rule is "as much as necissary but no more" and I don't really know what can be classified as "necessary").


```python
# Trying a favorite vs. retweet comparison
cat_fav = tcat_df['tweetFavoriteCt'].mean()
cat_fav
cat_rt = tcat_df['tweetRetweetCt'].mean()
cat_rt
dog_fav = tdog_df['tweetFavoriteCt'].mean()
dog_fav
dog_rt = tdog_df['tweetRetweetCt'].mean()
dog_rt
```




    0.7270316347793143






    2275.2095093767757






    0.8065740529871708






    42.87132771578228




```python
# okay, now for the bar graph
grouped = pd.DataFrame(columns = ['CATS', 'DOGS'])
grouped.set_value('Favorites', 'CATS', cat_fav)
grouped.set_value('Retweets', 'CATS', cat_rt)
grouped.set_value('Favorites', 'DOGS', dog_fav)
grouped.set_value('Retweets', 'DOGS', dog_rt)

grouped.plot.bar(figsize=(4,10))
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
      <td>0.727032</td>
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
      <td>0.727032</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>Retweets</th>
      <td>2275.21</td>
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
      <td>0.727032</td>
      <td>0.806574</td>
    </tr>
    <tr>
      <th>Retweets</th>
      <td>2275.21</td>
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
      <td>0.727032</td>
      <td>0.806574</td>
    </tr>
    <tr>
      <th>Retweets</th>
      <td>2275.21</td>
      <td>42.8713</td>
    </tr>
  </tbody>
</table>
</div>






    <matplotlib.axes._subplots.AxesSubplot at 0x11a943f60>




![png](output_22_5.png)


### The number of favorites vs. the number of retweets, is so different you have to make the height 50 before you even see anything for the favorites, so why not take a closer look with two graphs?.. but this time using total sums instead of means...


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
      <td>7676</td>
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
      <td>24021662</td>
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
      <td>7676</td>
      <td>21250</td>
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
      <td>24021662</td>
      <td>1129488</td>
    </tr>
  </tbody>
</table>
</div>






    <matplotlib.axes._subplots.AxesSubplot at 0x1195b5e10>




![png](output_25_5.png)





    <matplotlib.axes._subplots.AxesSubplot at 0x1197f5470>




![png](output_25_7.png)



```python
#figuring out percents
cd_fav_sum = cat_fav_total + dog_fav_total
cd_rt_sum = cat_rt_total + dog_rt_total

cat_fav_per = (cat_fav_total/cd_fav_sum)*100
cat_fav_per
dog_fav_per = (dog_fav_total/cd_fav_sum)*100
dog_fav_per

cat_rt_per = (cat_rt_total/cd_rt_sum)*100
cat_rt_per
dog_rt_per = (dog_rt_total/cd_rt_sum)*100
dog_rt_per

#pie charts

# Pie chart, where the slices will be ordered and plotted counter-clockwise:
labels = ['CATS','DOGS']
sizes = [cat_fav_per, dog_fav_per]
explode = (0.1, 0)

fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%',
        shadow=True, startangle=90)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.show()

labels = ['CATS','DOGS']
sizes = [cat_rt_per, dog_rt_per]
explode = (0.1, 0)

fig1, ax1 = plt.subplots()
ax1.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%',
        shadow=True, startangle=90)
ax1.axis('equal')  # Equal aspect ratio ensures that pie is drawn as a circle.

plt.show()
```




    26.536679803636865






    73.46332019636314






    95.50919938054523






    4.4908006194547765






    ([<matplotlib.patches.Wedge at 0x1198670f0>,
      <matplotlib.patches.Wedge at 0x11a3e04e0>],
     [<matplotlib.text.Text at 0x11a3d99b0>,
      <matplotlib.text.Text at 0x11a3ea2b0>],
     [<matplotlib.text.Text at 0x11a3d9f28>,
      <matplotlib.text.Text at 0x11a3ea828>])






    (-1.1878715515438656,
     1.1231293190766627,
     -1.1205800752252828,
     1.1713967424617027)




![png](output_26_6.png)





    ([<matplotlib.patches.Wedge at 0x11a3d1c88>,
      <matplotlib.patches.Wedge at 0x1197eeef0>],
     [<matplotlib.text.Text at 0x119843668>,
      <matplotlib.text.Text at 0x1195aa828>],
     [<matplotlib.text.Text at 0x1195ce160>,
      <matplotlib.text.Text at 0x1195c34a8>])






    (-1.1210833933672508,
     1.0936852457730111,
     -1.2120371100431815,
     1.1053351074001636)




![png](output_26_9.png)


### It loks like dog posts are more likely to be favorites on average than cat posts on Twitter, but cat posts are more likelt to be retweeted? - this is an interesting change 
### It is also clear that retweeting is much more common than favoriting posts

## Let's look at location and find the most popular place(s) for cat posts vs. dog posts


```python
timecat_df = tcat_df[tcat_df.userTimezone.notnull()]
timecat_df = timecat_df[['Hashtag', 'tweetCreated', 'tweetFavoriteCt', 'tweetID', 'tweetRetweetCt', 'tweetSource', 
                         'userID', 'userLocation', 'userName', 'userScreen', 'userTimezone']]
timecat_df.head()
timecat_df.size
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
      <th>tweetCreated</th>
      <th>tweetFavoriteCt</th>
      <th>tweetID</th>
      <th>tweetRetweetCt</th>
      <th>tweetSource</th>
      <th>userID</th>
      <th>userLocation</th>
      <th>userName</th>
      <th>userScreen</th>
      <th>userTimezone</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>cat</td>
      <td>2017-11-19 04:09:14</td>
      <td>0</td>
      <td>932098414392893440</td>
      <td>5</td>
      <td>Tweedle-deee</td>
      <td>771578403300401153</td>
      <td>Los Angeles, CA</td>
      <td>Daily Cat Fill</td>
      <td>DailyCatFill</td>
      <td>Pacific Time (US &amp; Canada)</td>
    </tr>
    <tr>
      <th>5</th>
      <td>cat</td>
      <td>2017-11-19 04:09:07</td>
      <td>0</td>
      <td>932098385665855488</td>
      <td>0</td>
      <td>twittbot.net</td>
      <td>817971712767574023</td>
      <td>NaN</td>
      <td>cat movie 博士の猫動画&amp;画像</td>
      <td>irohamovie</td>
      <td>Pacific Time (US &amp; Canada)</td>
    </tr>
    <tr>
      <th>7</th>
      <td>cat</td>
      <td>2017-11-19 04:09:05</td>
      <td>0</td>
      <td>932098375889043456</td>
      <td>1</td>
      <td>CatBot13</td>
      <td>911431574058602496</td>
      <td>America</td>
      <td>CatBot</td>
      <td>CatBot13</td>
      <td>Eastern Time (US &amp; Canada)</td>
    </tr>
    <tr>
      <th>8</th>
      <td>cat</td>
      <td>2017-11-19 04:09:04</td>
      <td>0</td>
      <td>932098374362214401</td>
      <td>2</td>
      <td>Twitter for Android</td>
      <td>179379870</td>
      <td>Beautiful British Columbia</td>
      <td>(⌐■_■)</td>
      <td>Al_SportsLover</td>
      <td>Pacific Time (US &amp; Canada)</td>
    </tr>
    <tr>
      <th>9</th>
      <td>cat</td>
      <td>2017-11-19 04:09:04</td>
      <td>0</td>
      <td>932098372344975361</td>
      <td>1</td>
      <td>Just Love Kawaii</td>
      <td>917714684119257090</td>
      <td>NaN</td>
      <td>Just Love Kawaii</td>
      <td>JustLoveKawaii</td>
      <td>Pacific Time (US &amp; Canada)</td>
    </tr>
  </tbody>
</table>
</div>






    74272




```python
#Get rid of none values for the timezone
pop_cat = timecat_df.groupby('userLocation')[['tweetRetweetCt', 'tweetFavoriteCt']].mean()
pop_cat.head()
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
      <th>tweetRetweetCt</th>
      <th>tweetFavoriteCt</th>
    </tr>
    <tr>
      <th>userLocation</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Oregon Coast</th>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Lahore, Punjab, Pakistan</th>
      <td>18517.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Last Tango in Wentworth, UK</th>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>PENNSYLVANIA</th>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>UK</th>
      <td>0.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
pop_cat.describe()
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
      <th>tweetRetweetCt</th>
      <th>tweetFavoriteCt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1159.000000</td>
      <td>1159.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>4478.327529</td>
      <td>0.365175</td>
    </tr>
    <tr>
      <th>std</th>
      <td>7904.801218</td>
      <td>1.834889</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>4630.125000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>38138.000000</td>
      <td>30.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
timedog_df = tdog_df[tdog_df.userTimezone.notnull()]
timedog_df = timedog_df[['Hashtag', 'tweetCreated', 'tweetFavoriteCt', 'tweetID', 'tweetRetweetCt', 'tweetSource', 
                         'userID', 'userLocation', 'userName', 'userScreen', 'userTimezone']]
timedog_df.head()
timedog_df.size
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
      <th>tweetCreated</th>
      <th>tweetFavoriteCt</th>
      <th>tweetID</th>
      <th>tweetRetweetCt</th>
      <th>tweetSource</th>
      <th>userID</th>
      <th>userLocation</th>
      <th>userName</th>
      <th>userScreen</th>
      <th>userTimezone</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>dog</td>
      <td>2017-11-19 04:07:57</td>
      <td>0</td>
      <td>932098092425396224</td>
      <td>30</td>
      <td>Twitter for iPhone</td>
      <td>1277021305</td>
      <td>氷の中</td>
      <td>柳生ジル</td>
      <td>etude_nine</td>
      <td>Irkutsk</td>
    </tr>
    <tr>
      <th>1</th>
      <td>dog</td>
      <td>2017-11-19 04:07:14</td>
      <td>0</td>
      <td>932097913932488704</td>
      <td>0</td>
      <td>twittbot.net</td>
      <td>741912323556675584</td>
      <td>United States</td>
      <td>Dog Food for A to Z</td>
      <td>dogfoodforatoz</td>
      <td>Pacific Time (US &amp; Canada)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>dog</td>
      <td>2017-11-19 04:07:03</td>
      <td>0</td>
      <td>932097867178655744</td>
      <td>0</td>
      <td>dlvr.it</td>
      <td>1402068510</td>
      <td>NaN</td>
      <td>The Pets World</td>
      <td>MabyMema</td>
      <td>Casablanca</td>
    </tr>
    <tr>
      <th>3</th>
      <td>dog</td>
      <td>2017-11-19 04:06:46</td>
      <td>0</td>
      <td>932097793547649024</td>
      <td>0</td>
      <td>Instagram</td>
      <td>239202335</td>
      <td>Laplace, La</td>
      <td>COURTNEY S. WILSON</td>
      <td>PUTTY2000</td>
      <td>Pacific Time (US &amp; Canada)</td>
    </tr>
    <tr>
      <th>4</th>
      <td>dog</td>
      <td>2017-11-19 04:06:44</td>
      <td>0</td>
      <td>932097786304258048</td>
      <td>0</td>
      <td>IFTTT</td>
      <td>11497892</td>
      <td>東京</td>
      <td>デカチワ</td>
      <td>dekachiwa</td>
      <td>Tokyo</td>
    </tr>
  </tbody>
</table>
</div>






    177738




```python
pop_dog = timedog_df.groupby('userLocation')[['tweetRetweetCt', 'tweetFavoriteCt']].mean()
pop_dog.head()
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
      <th>tweetRetweetCt</th>
      <th>tweetFavoriteCt</th>
    </tr>
    <tr>
      <th>userLocation</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Oregon Coast</th>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Mesa, AZ</th>
      <td>9.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>19° 1'3.41N,  72°51'22.19E</th>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Arizona</th>
      <td>3.0</td>
      <td>0.0</td>
    </tr>
    <tr>
      <th>Colorado now - Heaven Bound</th>
      <td>6.0</td>
      <td>0.0</td>
    </tr>
  </tbody>
</table>
</div>




```python
pop_dog.describe()
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
      <th>tweetRetweetCt</th>
      <th>tweetFavoriteCt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>1742.000000</td>
      <td>1742.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>41.941208</td>
      <td>0.814792</td>
    </tr>
    <tr>
      <th>std</th>
      <td>271.277113</td>
      <td>4.614692</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.720779</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>8.000000</td>
      <td>0.302198</td>
    </tr>
    <tr>
      <th>max</th>
      <td>6110.000000</td>
      <td>123.000000</td>
    </tr>
  </tbody>
</table>
</div>



#### What can we gather from this so far:
- dogs are retweeted more often than cats, but cats are liked more (as of Nov, 1) (THIS CHANGED SUDDENLY)  
    -just kidding. cats are retweeted more and dogs are favorited more. (Anytime after Nov, 6)  
    -with old data the above was true but now it is false, that is very interesting.
- cat mean: rt: 4478, fav: 0.36
- dog mean: rt: 41.7, fav: 0.81
- it looks like the cat df has at least 1 extremely popular post with over 30,000 rt
- the most popular dog post is 1/5 of that

## But let's go a little deeper


```python
# finding the locations of the max retweets and the hashtags (cats)
timecat_df['tweetRetweetCt'].max()
#the max is 38138 retweets
timecat_df.index[timecat_df['tweetRetweetCt'] == timecat_df['tweetRetweetCt'].max()].tolist()
# this returns a list of 4: 488, 532, 737, 831
timecat_df['userLocation'][488] #this one os from probably portugal 
timecat_df['Hashtag'][488]      #hashtag: cat

timecat_df['userLocation'][532] #this one is empty
timecat_df['Hashtag'][532]      #hashtag: cat

timecat_df['userLocation'][737] #this one is from singapore
timecat_df['Hashtag'][737]      #hashtag: cat

timecat_df['userLocation'][831] #this one is from singapore
timecat_df['Hashtag'][831]      #hashtag: cat
```




    38138






    [488, 532, 737, 831, 488, 532, 737, 831]






    488    ❁❁ nas's feyouncé ❁❁ 
    488    ❁❁ nas's feyouncé ❁❁ 
    Name: userLocation, dtype: object






    488    cat
    488    cat
    Name: Hashtag, dtype: object






    532    NaN
    532    NaN
    Name: userLocation, dtype: object






    532    cat
    532    cat
    Name: Hashtag, dtype: object






    737    singapore
    737    singapore
    Name: userLocation, dtype: object






    737    cat
    737    cat
    Name: Hashtag, dtype: object






    831    Singapore
    831    Singapore
    Name: userLocation, dtype: object






    831    cat
    831    cat
    Name: Hashtag, dtype: object



#### Noted trends: the most retweeted cat tweets seem to be from singapore, and the cat hashtag seems to be the most popular.


```python
# finding the locations of the max retweets and the hashtags (dogs)
timedog_df['tweetRetweetCt'].max()
#the max is 6110 retweets
timedog_df.index[timedog_df['tweetRetweetCt'] == timedog_df['tweetRetweetCt'].max()].tolist()
# this returns a list of 3: 3162, 3176, 3190
timedog_df['userLocation'][3162] #this one is empty
timedog_df['Hashtag'][3162]      #hashtag: dog

timedog_df['userLocation'][3176] #this one is from Poquoson, Virginia
timedog_df['Hashtag'][3176]      #hashtag: dog

timedog_df['userLocation'][3190] #this one is from Phoenix, Arizona
timedog_df['Hashtag'][3190]      #hashtag: dog
```




    6110






    [3162, 3176, 3190, 3162, 3176, 3190]






    3162    NaN
    3162    NaN
    Name: userLocation, dtype: object






    3162    dog
    3162    dog
    Name: Hashtag, dtype: object






    3176    Poquoson, Virginia USA
    3176    Poquoson, Virginia USA
    Name: userLocation, dtype: object






    3176    dog
    3176    dog
    Name: Hashtag, dtype: object






    3190    Phoenix - 85020
    3190    Phoenix - 85020
    Name: userLocation, dtype: object






    3190    dog
    3190    dog
    Name: Hashtag, dtype: object



#### Noted trends: Popularly retweeted dog tweets seem to come from America and dog seems to be the prefered hashtag


```python
# finding the locations of the max favorites and the hashtags (cats)
timecat_df['tweetFavoriteCt'].max()
#the max is 118
timecat_df.index[timecat_df['tweetFavoriteCt'] == timecat_df['tweetFavoriteCt'].max()].tolist()
# this returns a list of 1: 3489
timecat_df['userLocation'][3489] #this one is form somewhere in the US
timecat_df['Hashtag'][3489]      #hashtag: cat
```




    118






    [3489, 3489]






    3489    United States
    3489    United States
    Name: userLocation, dtype: object






    3489    cat
    3489    cat
    Name: Hashtag, dtype: object



#### Noted trend: the most favorited cat tweet is from America, and the hashtag is still cat, so it seems there is a strong preference for the more simple hashtags rather than the newer/more particular ones


```python
# finding the locations of the max favorites and the hashtags (dogs)
timedog_df['tweetFavoriteCt'].max()
#the max is 118
timedog_df.index[timedog_df['tweetFavoriteCt'] == timedog_df['tweetFavoriteCt'].max()].tolist()
# this returns a list of 1: 6825
timedog_df['userLocation'][6825] #this one is empty
timedog_df['Hashtag'][6825]      #hashtag: dog
```




    140






    [6825, 6825]






    6825    茨城県取手市
    6825    茨城県取手市
    Name: userLocation, dtype: object






    6825    dog
    6825    dog
    Name: Hashtag, dtype: object



#### Noted trend: the most favorited dog tweet is from somewhere in Japan ([Toride, Ibaraki](https://en.wikipedia.org/wiki/Toride,_Ibaraki)), and the hashtag is still dog, so it seems there is a strong preference for the more simple hashtags rather than the newer/more particular ones

## Moving on to Timezone  


```python
tz_cat = timecat_df.groupby('userTimezone')[['tweetRetweetCt', 'tweetFavoriteCt']].mean()
tz_cat.head()
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
      <th>tweetRetweetCt</th>
      <th>tweetFavoriteCt</th>
    </tr>
    <tr>
      <th>userTimezone</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Abu Dhabi</th>
      <td>18517.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>Adelaide</th>
      <td>6172.666667</td>
      <td>0.500000</td>
    </tr>
    <tr>
      <th>Alaska</th>
      <td>2388.615385</td>
      <td>0.205128</td>
    </tr>
    <tr>
      <th>America/Anguilla</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>America/Chicago</th>
      <td>1854.600000</td>
      <td>0.300000</td>
    </tr>
  </tbody>
</table>
</div>




```python
tz_cat.describe()
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
      <th>tweetRetweetCt</th>
      <th>tweetFavoriteCt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>108.000000</td>
      <td>108.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>4958.203298</td>
      <td>0.509712</td>
    </tr>
    <tr>
      <th>std</th>
      <td>6468.719789</td>
      <td>1.480354</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.925000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>1964.207407</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>8115.061905</td>
      <td>0.308355</td>
    </tr>
    <tr>
      <th>max</th>
      <td>18857.000000</td>
      <td>11.600000</td>
    </tr>
  </tbody>
</table>
</div>




```python
tz_dog = timedog_df.groupby('userTimezone')[['tweetRetweetCt', 'tweetFavoriteCt']].mean()
tz_dog.head()
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
      <th>tweetRetweetCt</th>
      <th>tweetFavoriteCt</th>
    </tr>
    <tr>
      <th>userTimezone</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Abu Dhabi</th>
      <td>128.200000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>Adelaide</th>
      <td>1.111111</td>
      <td>1.111111</td>
    </tr>
    <tr>
      <th>Alaska</th>
      <td>28.753425</td>
      <td>0.438356</td>
    </tr>
    <tr>
      <th>Almaty</th>
      <td>409.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>America/Argentina/Buenos_Aires</th>
      <td>0.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
tz_dog.describe()
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
      <th>tweetRetweetCt</th>
      <th>tweetFavoriteCt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>134.000000</td>
      <td>134.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>59.464546</td>
      <td>0.567645</td>
    </tr>
    <tr>
      <th>std</th>
      <td>115.471011</td>
      <td>0.992410</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>10.016667</td>
      <td>0.250000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>36.192630</td>
      <td>0.829545</td>
    </tr>
    <tr>
      <th>max</th>
      <td>612.400000</td>
      <td>9.000000</td>
    </tr>
  </tbody>
</table>
</div>



## Finding the most popular retweet timezones  
### First, cats


```python
# finding the timezone of the max retweets (cats)
tz_cat['tweetRetweetCt'].max()
#the max is 18857 retweets
tz_cat.index[tz_cat['tweetRetweetCt'] == tz_cat['tweetRetweetCt'].max()].tolist()
```




    18857.0






    ['Dublin']



#### What's happening here:
- the most popular timezone for cat retweets is Dublin, Germany
- full bar plot can be seen below


```python
#bar plot for cat retweet counts
ax = tz_cat['tweetRetweetCt'].plot(kind='barh', title ="Cat Timezone Retweet Count", figsize=(10, 25), 
                                    legend=True, fontsize=10)
ax.set_ylabel("Timezone", fontsize=12)
ax.set_xlabel("Count", fontsize=12)
plt.show()
```




    <matplotlib.text.Text at 0x1191a5eb8>






    <matplotlib.text.Text at 0x1195b5f28>




![png](output_52_2.png)


### Now, dogs


```python
# finding the timezone of the max retweets (dogs)
tz_dog['tweetRetweetCt'].max()
#the max is 612 retweets
tz_dog.index[tz_dog['tweetRetweetCt'] == tz_dog['tweetRetweetCt'].max()].tolist()
```




    612.39999999999998






    ['America/Phoenix']



#### What's happening here:
- the most popular timezone for dog retweets is Phoenix, Arizona
- full plot below


```python
#bar plot for dog retweet counts
ax = tz_dog['tweetRetweetCt'].plot(kind='barh', title ="Dog Timezone Retweet Count", figsize=(10, 25), 
                                    legend=True, fontsize=10)
ax.set_ylabel("Timezone", fontsize=12)
ax.set_xlabel("Count", fontsize=12)
plt.show()
```




    <matplotlib.text.Text at 0x11d315d68>






    <matplotlib.text.Text at 0x11d19d668>




![png](output_56_2.png)


## Moving on to the most favorited timezones  
### First, cats


```python
# finding the timezone of the max favorites (cats)
tz_cat['tweetFavoriteCt'].max()
#the max is 11.6 favorites
tz_cat.index[tz_cat['tweetFavoriteCt'] == tz_cat['tweetFavoriteCt'].max()].tolist()
```




    11.6






    ['Volgograd']



#### What this means:
- Volgograd, Russia is where cat tweets are most commonly favorited.
- full plot can be seen below


```python
#bar plot for cat favorite counts
ax = tz_cat['tweetFavoriteCt'].plot(kind='barh', title ="Cat Timezone Favorite Count", figsize=(10, 25), 
                                    legend=True, fontsize=10)
ax.set_ylabel("Timezone", fontsize=12)
ax.set_xlabel("Count", fontsize=12)
plt.show()
```




    <matplotlib.text.Text at 0x11e6feb00>






    <matplotlib.text.Text at 0x11e4a73c8>




![png](output_60_2.png)


### Now, dogs


```python
# finding the timezone of the max favorites (dogs)
tz_dog['tweetFavoriteCt'].max()
#the max is 9 
tz_dog.index[tz_dog['tweetFavoriteCt'] == tz_dog['tweetFavoriteCt'].max()].tolist()
```




    9.0






    ['Karachi']



#### What this means:
-  Karach, Pakistan is where dog tweets are most commonly favorited.
- a full plot can be seen in the bar graph below


```python
#bar plot for dog favorite counts
ax = tz_dog['tweetFavoriteCt'].plot(kind='barh', title ="Dog Timezone Favorite Count", figsize=(10, 25), 
                                    legend=True, fontsize=10)
ax.set_ylabel("Timezone", fontsize=12)
ax.set_xlabel("Count", fontsize=12)
plt.show()
```




    <matplotlib.text.Text at 0x11ef54748>






    <matplotlib.text.Text at 0x11ecd2a90>




![png](output_64_2.png)


### Points of interest:
- Cat retweet counts have the most on average (which isn't surprising, given the previous findings) where Dublin just barely is the one with the most retweets.
- Cat retweets and favorites seem to be more dispersed globally, where dog retweets and favorites seem to be very clustered into specific places. 

### Some other interesting findings:
- you can actually watch with this stuff (if you update frequently enough) specific memes gain popularity.
- there were a few days (in early Novemeber) where the data was very back and forth, however, now it seems to have solidified into the curent results. 


```python

```
