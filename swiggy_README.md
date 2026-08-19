========================================
SWIGGY DATA ANALYSIS - INSIGHTS
========================================

Q1: How many restaurants are listed per city?
----------------------------------------------
INSIGHT: "Bikaner has the most restaurants (850) on Swiggy, surpassing major metro cities like Bangalore and Delhi. This suggests that Swiggy has a strong presence in tier-2 cities, not just metros."

Q2: Most popular cuisines across India
----------------------------------------------
INSIGHT: North indian cuisines has the most popular cuisines(2911) across india.

Q3: Restaurant chains with most branches (FIXED)
----------------------------------------------
INSIGHT:Domino’s, Pizza Hut, and KFC dominate Swiggy with the highest branch counts, showing fast-food chains lead delivery presence.

Q4:  Top 5 cities with highest average rating
----------------------------------------------
INSIGHT:Cities like **Mylapore (Chennai), South Kolkata, Frazer Town (Bangalore), Adyar (Chennai), and Bandra West (Mumbai)** stand out with the highest average restaurant ratings, showing strong customer satisfaction in these food hubs.

Q5: Average cost for two across cities (FIXED)
----------------------------------------------
INSIGHTS:High-end food hubs like **Khan Market (Delhi), Fort Colaba (Mumbai), and DLF Phase 4 (Gurgaon)** have the highest average restaurant costs, showing premium dining dominance in these areas.

Q6: Cuisines with highest average rating
----------------------------------------------
INSIGHTS:Desserts and ice creams lead with the highest average ratings, showing they are the most loved cuisines on Swiggy.

Q7: Restaurants with rating above 4.5 and 1000+ ratings (FIXED)
----------------------------------------------
INSIGHTS:You can write this insight as:  
"No restaurant in the dataset has both a very high rating (≥4.5) and a large number of reviews (≥1000), showing that extremely popular and consistently top-rated restaurants are rare."

Q8:Best value for money (high rating, low cost) (FIXED)
----------------------------------------------
INSIGHTS:You can write this insight as:  
"Cities like Mylapore (Chennai), Adyar (Chennai), and Frazer Town (Bangalore) combine high average ratings with relatively moderate costs, showing strong customer satisfaction at affordable price points."

Q1: How many restaurants are listed per city?

SELECT city, COUNT(*) AS total_restaurants
FROM swiggy
GROUP BY city
ORDER BY total_restaurants DESC
LIMIT 10;

Q2: Most popular cuisines across India

SELECT cuisine, COUNT(*) AS count
FROM swiggy
GROUP BY cuisine
ORDER BY count DESC
LIMIT 10;

Q3: Restaurant chains with most branches (FIXED)

SELECT name AS restaurant_name, COUNT(*) AS branches
FROM swiggy
GROUP BY name
ORDER BY branches DESC
LIMIT 10;

Q4: Top 5 cities with highest average rating

SELECT city,
       ROUND(AVG(CAST(rating AS FLOAT)), 2) AS avg_rating,
       COUNT(*) AS total_restaurants
FROM swiggy
WHERE rating NOT IN ('NEW', '--')
GROUP BY city
HAVING COUNT(*) > 50
ORDER BY avg_rating DESC
LIMIT 5;

Q5: Average cost for two across cities (FIXED)

SELECT city,
       ROUND(AVG(CAST(REPLACE(REPLACE(cost, '₹', ''), ',', '') AS INTEGER)), 0) AS avg_cost
FROM swiggy
WHERE cost IS NOT NULL
  AND cost != 'Too Few Ratings'
GROUP BY city
ORDER BY avg_cost DESC
LIMIT 10;

Q6: Cuisines with highest average rating

SELECT cuisine,
       ROUND(AVG(CAST(rating AS FLOAT)), 2) AS avg_rating,
       COUNT(*) AS restaurant_count
FROM swiggy
WHERE rating NOT IN ('NEW', '--')
GROUP BY cuisine
HAVING COUNT(*) > 100
ORDER BY avg_rating DESC
LIMIT 10;

Q7: Restaurants with rating above 4.5 and 1000+ ratings (FIXED)
SELECT name AS restaurant_name, city, rating, rating_count, cost
FROM swiggy
WHERE CAST(rating AS FLOAT) >= 4.5
  AND CAST(REPLACE(rating_count, '+', '') AS INTEGER) >= 1000
ORDER BY rating DESC
LIMIT 20;

Q8: Best value for money (high rating, low cost) (FIXED)

SELECT city,
       ROUND(AVG(CAST(rating AS FLOAT)), 2) AS avg_rating,
       ROUND(AVG(CAST(REPLACE(REPLACE(cost, '₹', ''), ',', '') AS INTEGER)), 0) AS avg_cost
FROM swiggy
WHERE rating NOT IN ('NEW', '--')
  AND cost != 'Too Few Ratings'
GROUP BY city
HAVING COUNT(*) > 30
ORDER BY avg_rating DESC, avg_cost ASC
LIMIT 10;