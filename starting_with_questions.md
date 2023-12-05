Answer the following questions and provide the SQL queries used to find the answer.

    
### **Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


 ## SQL Queries:
``` SQL
SELECT country, SUM(total_transaction_revenue)
FROM some_sessions
GROUP BY country
HAVING SUM(total_transaction_revenue) IS NOT NULL
```
Then check the city  
```
SELECT city, SUM(total_transaction_revenue)
FROM some_sessions
WHERE city != '(not set)' OR city != 'not available in demo dataset' 
GROUP BY city
HAVING SUM(total_transaction_revenue) IS NOT NULL
```




## Answer: 
While analyzing the transaction revenues on the site,
the United States stands out as the country with the highest total transaction revenue. 
If you investigate further into city-level details, San Francisco emerges as the city with the highest transaction revenue. 
We should note, however, that quite a significant portion of the total transaction revenue data is null,  and within the city column, many values are either not set or unavailable in this dataset, this indicates potential data gaps or inconsistencies in reporting.
This missing data introduces a level of bias and one should be cautious when drawing definitive conclusions based on this small, potentially skewed sample.
To ensure a more representative analysis, a broader dataset with complete and accurate information would be necessary. 




----

### **Question 2: What is the average number of products ordered from visitors in each city and country?**


## SQL Queries:
```
SELECT country, city, AVG(product_quantity)
FROM some_sessions
GROUP BY country, city
HAVING AVG(product_quantity) IS NOT NULL
ORDER BY AVG(product_quantity) DESC, country, city
```




## Answer: 
Through a thorough analysis of the data we can 
determine the typical quantity of products that 
visitors in various cities and nations ordered. 
Through this analysis we can determine that the city with the largest average number
 of products ordered per visitor is Madrid, Spain. 
 It is important to remember that this information is based on the dataset that is available,
 and that this average is based on a small sample size from the dataset.






----
### **Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


## SQL Queries:
```SELECT 
	country,  
	v2_product_category, 
	COUNT(*) AS category_count
FROM clean_category
GROUP BY country, v2_product_category
ORDER BY country, category_count DESC;
```


## Answer:
While looking into the data on product categories ordered by visitors 
across different cities and countries, 
a persistent trend emerges as "Apparel" stands out as the most popular 
category within the various countries. 
Diving into that, a deeper analysis unveils additional interesting finds,
 notably in Australia, where there seems to be a higher preference for brand-centric choices. 
 These observations offer valuable insights into global and regional consumer behavior,
 emphasizing the broad appeal of clothing products while highlighting unique patterns 
 in specific regions. To gather a deeper analysis, future exploration with additional 
 variables or a more extensive dataset could unveil more patterns. 




----

### **Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


## SQL Queries: 
```SELECT ss.country, ss.city, ss.product_sku, v2_product_name, SUM(ca.units_sold)
FROM some_sessions ss
JOIN clean_analytics ca USING (visit_id)
GROUP BY ss.country, ss.city, ss.product_sku, v2_product_name, ca.units_sold
```

If I want to get more specific.
```SELECT 
    country, 
    city, 
    MAX(product_sku) AS top_seller,
    MAX(v2_product_name) AS v2_product_name,
    SUM(units_sold) AS total_sold
FROM (
    SELECT 
        country,
        city,
        v2_product_name,
        product_sku,
        units_sold
    FROM some_sessions
    JOIN clean_analytics ca USING (visit_id)
    GROUP BY country, city, v2_product_name, product_sku, units_sold
) products
GROUP BY country, city
ORDER BY country, city, total_sold DESC;
```

## Answer:
While dissecting the data to identify the top-selling products in various cities and countries, 
we can discover interesting patterns.. Notably, in Sunnyville, United States, 
there appears to be a notable affinity for SPF 15 lip balm, this suggests a regional preference 
for sun protection and a distaste for chapped lips. 
Additionally, the "Google Alpine Style Backpack" emerges as a standout favorite in New York. 
This could either be an increase in fitness trends, style trends, or an array of other possible 
reasons that more data and research could help shed light upon. 
Moreover, a broader trend seems to persist, with products related to Google, YouTube, 
or Android consistently ranking high in popularity across multiple cities. 
This leads to more questions about how a consumer reaches the brand page and
if it is intentional or a result from outside website traffic. Further research would help us 
determine a more difinitive analysis of the findings in our dataset. 





----
### **Question 5: Can we summarize the impact of revenue generated from each city/country?**

## SQL Queries: 
```
SELECT country, city, SUM(total_transaction_revenue)
FROM some_sessions
GROUP BY country, city
HAVING SUM(total_transaction_revenue) IS NOT NULL
ORDER BY SUM(total_transaction_revenue)DESC, country, city
```



## Answer: 
While examining the total transaction revenue generated from each city and country,
some possible trends emerge. The data suggests a concentration of revenue in cities within 
the United States, particularly in Los Angeles. However, this is an educated guess as we have 
the city and country but do not have the state or province of the cities provided. 
If one makes certain assumptions then it would appear that a significant proportion of the 
revenue is generated in areas characterized by subtropical climates. This insight would not
 only provide a geographical view on revenue distribution but could also suggest potential 
 correlations between climatic conditions and consumer spending patterns. It's essential to
 acknowledge that these observations are based on the available dataset, and one would need 
 to analyze more specific data in order to truly confirm the hypothesis above. 







