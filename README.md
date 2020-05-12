# Data Wrangling Report

## Gather

We gather three pieces of data of WeRateDogs Twitter Archive for this project. Each of these datsets are described below:

1. **The WeRateDogs Twitter archive**: This dataset is the tweet archive of Twitter user @dog_rates, also known as WeRateDogs. WeRateDogs is a Twitter account that rates people's dogs with a humorous comment about the dog. WeRateDogs has over 4 million followers and has received international media coverage.
2. **The tweet image predictions**: The dataset shows what breed of dog (or other object, animal, etc.) is present in each tweet according to a neural network. This file (image_predictions.tsv) is hosted on Udacity's servers and should be downloaded programmatically using the Requests library and the following URL: https://d17h27t6h515a5.cloudfront.net/topher/2017/August/599fd2ad_image-predictions/image-predictions.tsv
3. **Each tweet's retweet and favorite counts**: The dataset contains retweet and favorite counts, which are retrieved by using using the tweet IDs in the WeRateDogs Twitter archive, query the Twitter API for each tweet's JSON data using Python's Tweepy library and store each tweet's entire set of JSON data in a file called tweet_json.txt file. 

## Assess

#### Quality

***`twitter_archive` table***
- Missing expanded url information 
- Some dog stages contain two categories
- We don't need retweeted tweets in the dataset
- In reply status columns are redundant
- Retweeted status columns can be removed after filtering the original tweets
- Some denominators do not equal to 10
- Some numerators are incorrectly entered
- We only want tweets with image

***`image_pred`***
- Breed names mix with uppercase and lowercase.

#### Tidiness

- The variable of dog stage is divided into four columns.
- There should only be one table. First, the `counts` table only contains retweet and favorite counts. They seem to be the omissions from the `twitter_archive` table. Since we only want ratings that have images, we can merge `twitter_archive` and `image_predictions`. One is the original WeRateDogs archive including retweet and favorite counts, and the other one is the image prediction table. 
- Columns of `rating_numerator` and `rating_denominator` should be combined into one rating column.

## Clean

In this section, we will address types of quality and tidiness issues listed in the previous section. The process of data cleaning is described as below.

1. **Define**: Types of issues that we found in assess step will be converted to cleaning tasks. It can also be an instruction to others or future self. 
2. **Code**: The definitions are converted into code to actually clean the dataset.
3. **Test**: The cleaned dataset will be tested to see if our goals are met. 

All detected issues will be treated in this order:
1. **Missing Data**
2. **Tidiness Issue** 
3. **Quality Issue**

Details of each cleaning process are shown as follows.

### Missing Data

**1. `twitter_archive`: Missing `expanded_urls` data**

***Define***

Fill the entire `expanded_urls` column, including missing url data, with standard url and ID number. 

### Tidiness

**2. *Dog stage* variable is divided into four columns.**

***Define***

Create a `dog_stage` column which contains all stages in four columns and drop the redundant columns after creating `dog_stage`. 

**3. We only want original tweets with images, and the image information is contained in another table. Also, retweet and favorite counts are seen as omissions from the `twitter_archive` table. Therefore, these three tables can be combined into one.**

***Define***

Combine three tables by ID numbers.

### Quality

**4. We only want original tweets**

***Define***

The retweets will be dropped from the dataset by detecting the rows where the `retweeted_status_id` is not null.

**5. Since *reply tweets* are viewed as original tweets, the related columns are redundant in the dataset. In addition, *retweeted* columns can also be removed from the dataset since all retweets have been filtered out.**

***Define***

Remove all *reply* (`in_reply_to_status_id`, `in_reply_to_user_id`) and *retweet* (`retweeted_status_id`, `retweeted_status_user_id`, `retweeted_status_timestamp`) columns from the dataset.

**6. Some denominators are not 10**

***Define***

Put value of 10 to all cell in `rating_denominator` column.

**7. Some numerators are incorrectly entered.**

***Define***

Replace all value greater than 15 with 15. 

**8. We only want tweets with images.**

***Define***

Drop the rows without image by detecting the rows where its `jpg_url` is null.

**9. Breed names are inconsistent. It will be hard to count the total number of each dog breed.**

***Define***

Change all first letters to uppercase and replace "_" with space.

**10. We only need one column for ratings.**

***Define***

Create a new column to contain the values of dividing `numerator` by `denominator`. 
