## Apple IOS app store analysis

### Introduction:

I conducted an Exploratory Data Analysis on the Apple iOS App Store. This project involved examining and analyzing the dataset using SQL to extract valuable insights and trends related to app ratings, categories, monetization, and user engagement. The EDA aimed to uncover patterns and correlations within the data, providing a foundation for data-driven decision-making in the context of iOS app development and distribution.

Dataset Link: https://www.kaggle.com/datasets/ramamet4/app-store-apple-data-set-10k-apps
  
### Problem Statement:

1. Number of unique apps in Apple store?

2. Determine whether paid apps have higher ratings than free apps?

3. Analyzing if apps with more supported languages have higher ratings?

4. Checking genres with low ratings?

5. Examine if there is a correlation between the length of description and user rating?

6. Which are the top rated apps for each genre?

### SQL Scripts of my Analysis:

#### Performing Exploratory Data Analysis

-- check no. of unique apps in both tableAppleStore --

select
    count(distinct id) AS UniqueAppIds
From
    apple_store;

select
    count(distinct id) AS UniqueAppIds
From
    store_description;

-- check for missing values --

select
    count(*) as MissingValues
from
    apple_store
where
    track_name IS NULL
    OR user_rating IS NULL
    OR prime_genre IS NULL
select
    count(*) as MissingValues
from
    store_description
where
    app_desc IS NULL 

-- Finding out the number of apps per genre --

select
    prime_genre,
    count(*) as NumApps
From
    apple_store
group by
    prime_genre
order by
    NumApps DESC

-- Overview of the apps rating

select
    min(user_rating) as MinRating,
    max(user_rating) as MaxRating,
    avg(user_rating) as AvgRating
From
    apple_store;

#### Data Analysis scripts:

-- Determine whether paid apps have higher ratings than free apps--

select
    case
        when price > 0 then 'paid'
        else 'free'
    end as App_type,
    avg(user_rating) as Avg_rating
from
    apple_store
group by
    App_type 
    

-- check if apps with more supported languages have higher ratings --

select case 
            when lang.num < 10 then '< 10 languages'
            when lang.num between 10 and 30 then '10-30 languages'
            else '>30 languages'
        end as language_bucket,
        avg(user_rating) as Avg_rating
from apple_store
group by language_bucket
order by Avg_rating desc

-- check genres with low ratings--

select prime_genre,
        avg(user_rating) as Avg_rating
from apple_store
group by prime_genre
order by Avg_rating asc
limit 10

-- check if there is a correlation between the length of description and user rating --

select case
            when length(b.app_desc) < 500 then 'Short'
            when length(b.app_desc) between 500 and 1000 then 'Medium'
            else 'Large'
        end as description_length_bucket,
        avg(a.user_rating) as Avg_rating


from apple_store as A

Join store_description as B

on
a.id = b.id

group by description_length_bucket
order by Avg_rating desc

-- check the top rated apps for each genre --

select 
    prime_genre,
    track_name,
    user_rating
from (
    select 
    prime_genre,
    track_name,
    user_rating,
    rank() over(partition by prime_genre order by user_rating desc, rating_count_tot desc) as rank
    from
    apple_store
) as A
where 
A.rank = 1
