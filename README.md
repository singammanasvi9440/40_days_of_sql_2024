# 40_days_of_sql_2024


# Day 1 

𝐐𝐮𝐞𝐬𝐭𝐢𝐨𝐧: https://datalemur.com/questions/sql-histogram-tweets

Write a SQL query to obtain a histogram of tweets posted per user in 2022. Output the tweet count per user as the bucket and the number of Twitter users who fall into that bucket.

In other words, group the users by the number of tweets they posted in 2022 and count the number of users in each group

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



# Day 2

 Solving Duplicate Job Listings in SQL 🧑‍💻
Ever wondered how to identify companies that post duplicate job listings? 


Question link: https://lnkd.in/giaK8wZY

The Problem: 

We need to find how many companies have posted duplicate job listings — listings with the same title and description within the same company. This is a common challenge when analyzing job posting data across platforms like LinkedIn.

Here's how I approached it:


Using a combination of GROUP BY and HAVING COUNT(*) > 1, we can easily identify duplicate entries and use the subquery to count the number of companies affected.


Query: 
SELECT COUNT(DISTINCT a.company_id) AS duplicate_companies
FROM (
 SELECT company_id
 FROM job_listings
 GROUP BY company_id, title, description
 HAVING COUNT(*) > 1
) AS a;


#Day 3

Number of Posts in SQL 🧑‍💻
Ever wondered how to identify average posts of person in social media? 

Question link: https://datalemur.com/questions/sql-average-post-hiatus-1

The Problem: 
We need to identify users who  who posted at least twice in a year. 

Here's how I approached it:

Using a combination of GROUP BY and HAVING to track the most recent post date for each user, and then comparing that against the current date using max and min functions.


Query: 
SELECT user_id,
extract(day from ( max(post_date)- min(post_date))) as days_between
FROM posts
where post_date between '2021-01-01' and '2021-12-31'
group by user_id 
having count(post_date)>1;


