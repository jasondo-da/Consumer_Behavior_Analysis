# Consumer Behavior Analysis

![image](https://github.com/jasondo-da/Consumer_Behavior_Analysis/assets/138195365/a26d5a90-a60e-42d2-b89c-8119f2edc676)

## Table of Contents

- [Project Introduction](#project-introduction)
    - [Customer Analysis SQL Queries](#customer-analysis-sql-queries)
    - [Consumer Behavior Analysis Dataset](#consumer-behavior-analysis-dataset)
- [Analysis Outline](#analysis-outline)

## Project Introduction

This is a Kaggle-sourced dataset used to refine my data analytics skills further and gain more data science experience. The “Customer Behavior and Shopping Habits Dataset” contains a variety of intricate insights into customer preferences and mannerisms when shopping from an untitled online retailer. The purpose of this project is to be part of an ongoing process to refine and develop my data analysis skills. In this customer analysis, I will use SQL to discover new insights within the dataset to better understand the customers and their purchasing preferences. Key areas of focus include finding the primary demographic, and their product preferences.

## Customer Analysis SQL Queries
All SQL queries on GitHub.

Link: [Consumer Behavior Analysis](https://github.com/jasondo-da/Consumer_Behavior_Analysis/blob/main/cba_queries.sql)

## Consumer Behavior Analysis Dataset

The Consumer Behavior and Shopping Habits Dataset provides a detailed overview of consumer preferences and purchasing behaviors. It includes demographic information, purchase history, product preferences, and preferred shopping channels. This dataset is essential for businesses aiming to tailor their strategies to meet customer needs and enhance their shopping experience, ultimately driving sales and loyalty. This project was originally completed using the MySQL platform and the original dataset formatting has been adjusted and cleaned to work on the MySQL platform.

Link: [Original Kaggle Dataset](https://www.kaggle.com/datasets/zeesolver/consumer-behavior-and-shopping-habits-dataset/)

Link: [Cleaned Shopping Behavior Dataset](https://github.com/jasondo-da/Consumer_Behavior_Analysis/blob/main/sb_clean.csv)

| Shopping Behavior Dataset Tables | Table Description |
| :------------- | :------------ |
| Customer ID | A unique identifier for each individual customer |
| Age | The age of the customer, providing demographic information for segmentation and targeted marketing strategies |
| Gender | The gender identification of the customer, a key demographic variable influencing product preferences and purchasing patterns |
| Item Purchased | The specific product bought by the customer |
| Category | The broad classification of the purchased item |
| Purchase Amount (USD) | The monetary transaction value, denoted in USD |
| Location | The geographical location where the purchase was made |
| Size | The size of the purchased item, if applicable |
| Color | The color variant of the purchased item |
| Season | The season in which the item was purchased |
| Review Rating | A customer assessment of the product out of 5 stars |
| Subscription Status | Indicates whether the customer has opted for a subscription service |
| Shipping Type | Specifies the method used to deliver the purchased item |
| Discount Applied | Indicates if any promotional discounts were applied to the purchase |
| Promo Code Used | Notes whether a promotional code or coupon was utilized during the transaction |
| Previous Purchases | Provides information on the number or frequency of prior purchases made by the customer |
| Payment Method | Specifies the mode of payment employed by the customer |
| Frequency of Purchases | Indicates how often the customer engages in purchasing activities | 


## Analysis Outline

```sql
/* Uncovering the main customer demographic’s age and gender */
SELECT gender,
    COUNT(gender) total_customers,
    ROUND(avg(age), 1) avg_age
FROM customer_orders
GROUP BY gender
ORDER BY total_customers DESC
```

Output:
| gender | total_customers | avg_age | 
| :-----------: | :----------: | :-----------: |
| male | 2652 | 44.1 |
| female | 1248 | 44.0 |


```sql
/* Calculating customer concentrations for each state with the amount of revenue generated */
SELECT location,
    COUNT(customer_id) customer_count,
    SUM(purchase_total) state_revenue
FROM customer_orders
GROUP BY location
ORDER BY state_revenue DESC
```

[Output Data](https://github.com/jasondo-da/Consumer_Behavior_Analysis/blob/main/customer_concentration_and_revenue.csv)


```sql
/* Finding customer favorite products */
SELECT item_sub_cat product,
    category,
    COUNT(item_sub_cat) quantity_sold,
    SUM(purchase_total) product_revenue
FROM customer_orders
GROUP BY product, category
ORDER BY product_revenue DESC
```

[Output Data](https://github.com/jasondo-da/Consumer_Behavior_Analysis/blob/main/product_popularity.csv)


```sql
/* Calculating the average customer rating for company products */
SELECT category, ROUND(AVG(review_rating), 2) avg_rating
FROM customer_orders
GROUP BY category
ORDER BY avg_rating DESC
```

Output:
| category | avg_rating |
| :-----------: | :----------: |
| footwear | 3.79 |
| accessories | 3.77 |
| outerwear | 3.75 |
| clothing | 3.72 |


```sql
/* Gauging customer sentiment through paid shipping preferences */
SELECT (SELECT COUNT(shipping_type)
        FROM customer_orders
        WHERE shipping_type = 'free shipping' 
            OR shipping_type = 'store pickup') free_shipping_count,
        (SELECT COUNT(shipping_type)
        FROM customer_orders
        WHERE shipping_type = 'express' 
            OR shipping_type = 'next day air' 
            OR shipping_type = 'standard' 
            OR shipping_type = '2-day shipping') paid_prem_shipping_count
FROM customer_orders
LIMIT 1
```

Output:
| free_shipping_count | paid_prem_shipping_count |
| :-----------: | :----------: |
| 1325 | 2575 |


```sql
/* Customers with x number of previous orders */
SELECT (SELECT COUNT(previous_orders)
    	FROM customer_orders
    	WHERE previous_orders > 5
    	ORDER BY previous_orders DESC) '5+_orders_customers',
    	(SELECT COUNT(previous_orders)
    	FROM customer_orders
    	WHERE previous_orders > 10
    	ORDER BY previous_orders DESC) '10+_orders_customers',
    	(SELECT COUNT(previous_orders)
    	FROM customer_orders
    	WHERE previous_orders > 20
    	ORDER BY previous_orders DESC) '20+_orders_customers'
FROM customer_orders
LIMIT 1
```

Output:
| 5+_orders_customers | 10+_orders_customers | 20+_orders_customers |
| :-----------: | :-----------: | :-----------: |
| 3476 | 3116 | 2339 |
