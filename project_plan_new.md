# Project Plan Take 2
###### Last Updated October 28, 2017  
Margaret Jones (mmj32)  
3 October 2017  
Data Science for Linguists  
Final Project  

## Idea
- Cats vs. Dogs on Twitter: Who is more popular?
- Comparing the amount of likes and also comparisons between neologisms for/about cats and dogs.
  - neologisms that will be compared:
    - dogs: "doggo" and "doge"
    - cats: "kitteh" and "toe beans"
  - Plurality must also be taken into account, and because it is uncertain how Twitter handles this both will be taken.

## Plan
- I will use tweepy to gather data from Twitter on posts people have about cats and dogs.
- Then I will compare with numpy and matplotlib to show statistics and compare the data.
  - Compare when the neologism developed.
  - if a picture is present on the post or not
  - Compare location, number, and timezone.
  - Average length of the posts will also be compared.
  - If there is a particular user that posts more than others.
- This will be completed in a jupyter notebook that will also be found in this repository.

## Data Sharing
- Twitter is a little bit tricky to work with it seems when it comes to data sharing. According to what is found [here](https://blog.ldodds.com/2017/05/19/can-you-publish-tweets-as-open-data/), Twitter allows people to store ONLY tweet IDs into a dataset to distribute. However, even this has limitations on size, and time scope, as well as licensing issues. So, sharing my data isn't going to work very well. In order to stay on the safe size, I will not post my dataset of stored tweets anywhere on GitHub, and what can be seen on GitHub will only be small clips from my dataset. That way, you can see what the data looks like and what it is doing, but ultimately will be read in from a local file on my machine.
