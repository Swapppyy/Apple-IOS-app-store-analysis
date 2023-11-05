## Apple IOS app store analysis

### Introduction:

I conducted an Exploratory Data Analysis on the Apple iOS App Store. This project involved examining and analyzing the dataset using SQL to extract valuable insights and trends related to app ratings, categories, monetization, and user engagement. The EDA aimed to uncover patterns and correlations within the data, providing a foundation for data-driven decision-making in the context of iOS app development and distribution.

Dataset Link: https://www.kaggle.com/datasets/ramamet4/app-store-apple-data-set-10k-apps
  
### Checkout my SQL scripts for Data Analysis:

#### Exploratory Data Analysis:

-- check no. of unique apps in both tableAppleStore --


    'select count(distinct id) AS UniqueAppIds From appleStore;'
    'select count(distinct id) AS UniqueAppIds From appleStore_description;'

-- check for missing values --

    'select count(*) as MissingValues from apple_store where track_name IS NULL OR user_rating IS NULL OR prime_genre IS NULL;'
    'select count(*) as MissingValues from store_description where app_desc IS NULL;'
    
-- Finding out the number of apps per genre --

    'SELECT prime_genre, COUNT(*) AS NumApps FROM apple_store GROUP BY prime_genre ORDER BY NumApps DESC;'

-- Overview of the apps rating --

    'SELECT MIN(user_rating) AS MinRating, MAX(user_rating) AS MaxRating, AVG(user_rating) AS AvgRating FROM apple_store;'

#### Data Analysis Part:

1. Determine whether paid apps have higher ratings than free apps?

       'SELECT CASE WHEN price > 0 THEN 'paid' ELSE 'free' END AS App_type, AVG(user_rating) AS Avg_rating FROM apple_store GROUP BY App_type;'

2. Check if apps with more supported languages have higher ratings?

       'SELECT CASE WHEN lang.num < 10 THEN '< 10 languages' WHEN lang.num BETWEEN 10 AND 30 THEN '10-30 languages' ELSE '>30 languages' END AS language_bucket, AVG(user_rating) AS Avg_rating FROM apple_store GROUP BY language_bucket ORDER BY Avg_rating DESC;'

3. Analyze genres with low ratings

       'SELECT prime_genre, AVG(user_rating) AS Avg_rating FROM apple_store GROUP BY prime_genre ORDER BY Avg_rating ASC LIMIT 10;'

4. Checking if there is a correlation between the length of description and user rating

       'SELECT CASE WHEN LENGTH(b.app_desc) < 500 THEN 'Short' WHEN LENGTH(b.app_desc) BETWEEN 500 AND 1000 THEN 'Medium' ELSE 'Large' END AS description_length_bucket, AVG(a.user_rating) AS Avg_rating FROM apple_store AS a JOIN store_description AS b ON a.id = b.id GROUP BY description_length_bucket ORDER BY Avg_rating DESC;'

5. Check the top rated apps for each genre

        'SELECT prime_genre, track_name, user_rating FROM (SELECT prime_genre, track_name, user_rating, RANK() OVER (PARTITION BY prime_genre ORDER BY user_rating DESC, rating_count_tot DESC) AS rank FROM apple_store) AS A WHERE A.rank = 1;'

My Insights:

### Paid apps have higher rating

### Apps which have between 10 and 30 languages have better ratings

### Finance and book apps have low ratings

### Apps with longer description have better ratings

### Games and entertainment have a high competition












    
