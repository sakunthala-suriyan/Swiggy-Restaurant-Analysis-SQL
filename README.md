# 🛵 Swiggy Food Delivery Data Analysis (SQL)

An end-to-end SQL data analysis project exploring restaurant listings, dish popularity, pricing metrics, customer ratings, and geographic distribution across Swiggy orders in India.

---

## 📌 Project Overview

This project analyzes food delivery order data from Swiggy to uncover key business insights, regional dining preferences, price point distributions, and high-performing restaurant chains. 

Using **SQLite** and **DB Browser for SQLite**, raw unstructured delivery data was cleaned, queried, and analyzed using intermediate to advanced SQL techniques including aggregations, filtering, group filtering (`HAVING`), sorting, and multi-column grouping.

---

## 📊 Key Business Findings

* **Market Concentration:** Bengaluru leads significantly with over 20,000 listed menu items, followed closely by Mumbai and Hyderabad.
* **Rating & Pricing Stability:** High-volume top-rated items consistently maintain an average customer rating above **4.5** while maintaining competitive pricing in core categories.
* **Category Dominance:** Quick-serve and core staples (*Snacks, North Indian, Beverages*) dominate listings across top urban centers.

---

## 🛠️ Tools & Technologies Used

* **Database Engine:** SQLite
* **GUI / Query Environment:** DB Browser for SQLite
* **SQL Concepts:** Aggregate Functions (`COUNT`, `AVG`, `SUM`), Grouping (`GROUP BY`, `HAVING`), Sorting (`ORDER BY`), Distinct Counting (`COUNT(DISTINCT)`), Formatting (`ROUND`).
* **Output Format:** CSV

---

## 📁 Repository Structure

```text
.
├── outputs/
│   ├── q1_orders_per_city.csv
│   ├── q2_popular_categories.csv
│   ├── q3_top_expensive_dishes.csv
│   ├── q4_top_rated_dishes.csv
│   ├── q5_avg_price_by_city.csv
│   ├── q6_top_restaurant_chains.csv
│   ├── q7_category_by_city.csv
│   └── q8_category_price_vs_rating.csv
├── swiggy_project.db
├── Swiggy Raw Data Excel.csv
└── README.md


🔍 Key SQL Queries & Business
Questions1. Geographic Footprint (Top Cities by Order Volume)
Identifies top cities with the highest volume of listed menu items and orders
SELECT city, COUNT(*) AS total_orders
FROM swiggy_orders
GROUP BY city
ORDER BY total_orders DESC
LIMIT 10;

2. Popular Food Categories
Finds the most frequently ordered food categories across India.
SELECT category, COUNT(*) AS category_count
FROM swiggy_orders
GROUP BY category
ORDER BY category_count DESC
LIMIT 10;

3. Premium Menu Analysis
Identifies the top 10 highest-priced items listed across all restaurants.
SELECT restaurant_name, location, city, dish_name, category, price
FROM swiggy_orders
ORDER BY price DESC
LIMIT 10;

4. Quality & Satisfaction Tiers
Filters top-rated dishes with significant customer feedback ($\ge 4.5$ rating and $> 50$ ratings).
SELECT restaurant_name, city, dish_name, category, rating, rating_count
FROM swiggy_orders
WHERE rating >= 4.5 AND rating_count > 50
ORDER BY rating DESC, rating_count DESC
LIMIT 10;

5. Regional Pricing Benchmarks
Compares average dish pricing across major cities with over 100 listings using HAVING.
SELECT city, 
ROUND(AVG(price), 2) AS avg_dish_price,
COUNT(*) AS total_items
FROM swiggy_orders
WHERE price > 0
GROUP BY city
HAVING total_items > 100
ORDER BY avg_dish_price DESC
LIMIT 10;

6. Chain Expansion Metrics
Uncovers the largest restaurant chains based on unique location footprints and multi-city presence.
SELECT restaurant_name, 
COUNT(DISTINCT location) AS unique_locations,
COUNT(DISTINCT city) AS cities_present,
COUNT(*) AS total_dishes_listed
FROM swiggy_orders
GROUP BY restaurant_name
ORDER BY unique_locations DESC, total_dishes_listed DESC
LIMIT 10;

7. Regional Category Breakdown
Examines cuisine category popularity across key metropolitan hubs.
SELECT city, category, COUNT(*) AS dish_count
FROM swiggy_orders
WHERE city IN ('Bengaluru', 'Mumbai', 'New Delhi', 'Hyderabad')
GROUP BY city, category
ORDER BY city, dish_count DESC;

8. Price vs. Rating Satisfaction
Evaluates average pricing against customer ratings across food categories.
SELECT category, 
COUNT(*) AS total_items,
ROUND(AVG(price), 2) AS avg_price,
ROUND(AVG(rating), 2) AS avg_rating,
SUM(rating_count) AS total_ratings_received
FROM swiggy_orders
WHERE price > 0 AND rating > 0
GROUP BY category
HAVING total_items >= 50
ORDER BY avg_rating DESC, total_items DESC
LIMIT 15;


🚀 How to ReplicateClone or download this repository.
Open DB Browser for SQLite.
Open swiggy_project.db (or create a new database and import Swiggy Raw Data Excel.csv).
Execute any of the queries listed above in the Execute SQL tab.

👤 AuthorSakunthala S – Aspiring Data Analyst & Business Intelligence Developer
