Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


###SQL Queries:
``` SQL
SELECT country, SUM(total_transaction_revenue)
FROM some_sessions
GROUP BY country
HAVING SUM(total_transaction_revenue) IS NOT NULL
---
SELECT city, SUM(total_transaction_revenue)
FROM some_sessions
GROUP BY city
HAVING SUM(total_transaction_revenue) IS NOT NULL
```




Answer: The United States is the country with the highest level of transaction revenue, 
and San Francisco is the city with the highest level of transaction revenue. 
However most of the total transaction revenue slots are null 
and a lot of the values in the city column were unavailable in the demo dataset
, so this is a very biased small sample. 





**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
```
SELECT country, city, AVG(product_quantity)
FROM some_sessions
GROUP BY country, city
HAVING AVG(product_quantity) IS NOT NULL
ORDER BY AVG(product_quantity) DESC, country, city
```




Answer:





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







