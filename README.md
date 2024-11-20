# 40_days_of_sql_2024


# Day 1 

𝐐𝐮𝐞𝐬𝐭𝐢𝐨𝐧: https://datalemur.com/questions/sql-histogram-tweets

Write a SQL query to obtain a histogram of tweets posted per user in 2022. Output the tweet count per user as the bucket and the number of Twitter users who fall into that bucket.

In other words, group the users by the number of tweets they posted in 2022 and count the number of users in each group
𝐁𝐭𝐰, 𝐲𝐨𝐮 𝐜𝐚𝐧 𝐟𝐢𝐧𝐝 𝐭𝐡𝐞 𝐪𝐮𝐞𝐬𝐭𝐢𝐨𝐧 𝐡𝐞𝐫𝐞: 
https://datalemur.com/questions/sql-histogram-tweets
𝐌𝐲 𝐓𝐡𝐨𝐮𝐠𝐡𝐭 𝐏𝐫𝐨𝐜𝐞𝐬𝐬:

1. 𝐈𝐝𝐞𝐧𝐭𝐢𝐟𝐲𝐢𝐧g frequency of tweets by each user:
I started finding how many tweets each user has posted in 2022 using **group by** and **count function.**

SELECT user_id, COUNT(*) AS tweet_count
    FROM tweets
    WHERE tweet_date >= '2022-01-01' AND tweet_date < '2023-01-01'
    GROUP BY user_id
**O/P for the inner query :**
User 111 has 2 tweets in 2022.
User 254 has 1 tweet in 2022.
User 148 has 1 tweet in 2022.

2. 𝐂𝐚𝐥𝐜𝐮𝐥𝐚𝐭𝐢𝐧𝐠 𝐭𝐡𝐞 Histogram-level count:
I Found tweet count per user and the number of Twitter users who fall into that bucket using **group_by** and **count function**.

SELECT tweet_count AS tweet_bucket,
       COUNT(*) AS users_num
ORDER BY tweet_count
Thi gives 0/p as : 2 users posted exactly 1 tweet in 2022.
                   1 user posted exactly 2 tweets in 2022.


4. 𝐅𝐢𝐧𝐚𝐥𝐥𝐲, I used subqueries 
Here's the SQL query that will accomplish this:


SELECT 
  tweet_count_per_user AS tweet_bucket, 
  COUNT(user_id) AS users_num 
FROM (
  SELECT 
    user_id, 
    COUNT(tweet_id) AS tweet_count_per_user 
  FROM tweets 
  WHERE tweet_date BETWEEN '2022-01-01' 
    AND '2022-12-31'
  GROUP BY user_id) AS total_tweets 
GROUP BY tweet_count_per_user;

This query can be adapted for larger datasets to obtain the desired histogram of tweet counts per user.

