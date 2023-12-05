### **Question 1: What seems to be the product with the most revenue and are there any patterns in the top 20.** 


## SQL Queries:
```
SELECT DISTINCT(product_sku), product_name, unit_price, unit_quantity,
	(SELECT unit_price * unit_quantity) AS unit_revenue
FROM fixed_products
ORDER BY unit_revenue DESC
LIMIT 20
```

## Answer: 
While analyzing the product landscape to discover top revenue earners, the Learning Thermostat 3rd Gen - USA  secures the first position in terms of revenue. Beyond this standout product, a closer examination of the top 20 revenue-generating items reveals intriguing patterns.
 Technological innovations, including security cameras, smoke and CO alarms, and smart technology thermostats, are prominent in the top revenue field. 
 Unsurprisingly technological items tend to be of a higher price range, and these specific products target safety concerns rather than frivolous fun tech toys. 
 We can also discover the diversity of the product mix when we notice that 7 out of the top 20 revenue items are stationery products.
 This suggests a potential niche or consumer demand for both a safer home and everyday office essentials.


-----

### Question 2: **How often do they actually complete the entire checkout process.**


## SQL Queries:
```
SELECT total_transaction_revenue, visit_id,page_path_level1, page_title, ecommerce_action_type,ecommerce_action_step, ecommerce_action_option
FROM all_sessions
WHERE ecommerce_action_type = 5 OR ecommerce_action_step = 3
OR ecommerce_action_option = 'Review'
OR page_title = 'Checkout Review'
OR page_path_level1 = '/revieworder.html'
```

## Answer:
While analyzing the checkout process within the dataset, a peculiar picture unfolds. 
It seems evident that only a limited number of visitors have progressed beyond the initial step, with only thirteen rows reaching the second stage of the checkout process.
 Otherwise known as the payment information section. Even more interesting is the observation that only five individuals 
 successfully completed all three steps in the checkout journey reviewing and completing their orders. 
 Furthermore, when examining the total revenue from these groupings we can see that some who have only completed the first step in the process still 
 have a transaction revenue for their order. This suggests a potential disparity between completed steps and recorded revenue. 
 should consider the possibility of incomplete or missing data, potential user drop-offs at various stages, or other factors influencing the recorded revenue information. 

-----

### **Question 3: What is brand loyalty vs. shopping for an object. Does this brand loyalty have a higher percentage in certain cities or countries?**

## SQL Queries:
```
SELECT (
SELECT COUNT(*)
FROM some_sessions
WHERE v2_product_category LIKE '%Brand%') AS "Brand Loyalty",
COUNT(*) AS "Total Product Categories"
FROM some_sessions;
— What about in certain countries. 
SELECT
    country,
    COUNT(*) AS "Total Sessions",
    SUM(CASE WHEN v2_product_category LIKE '%Brand%' THEN 1 ELSE 0 END)
 AS "Brand Loyalty"
FROM
    some_sessions
GROUP BY
    country
ORDER BY SUM(CASE WHEN v2_product_category LIKE '%Brand%' THEN 1 ELSE 0 END) DESC
```

## Answer: 
While analyzing the dataset to understand the dynamics between brand loyalty and 
shopping for specific objects, we can identify the following patterns.
 The initial query reveals that, overall, shopping for brand-related products 
 is not within the majority of items searched for. If we were to look into a country-wise analysis,
 it becomes apparent that the United States stands out with the majority of sessions dedicated
 to shopping by brand. This initial glance suggests a higher inclination towards brand loyalty
 in the U.S. when compared to other countries. However, it's important to note that this 
 observation is based on the available dataset, 
and the concept of brand loyalty may manifest differently depending on cultural contexts.

-----

### **Question 4: Which channel is bringing in the most revenue or customers.**

## SQL Queries: 
```
SELECT DISTINCT(channel_grouping), SUM(total_transaction_revenue) 
FROM some_sessions
GROUP BY channel_grouping
```


## Answer: 
While analyzing revenue dynamics across different channels we notice the
 following patterns. According to the dataset sample we have at hand, the Referral 
 channel marks itself as the leading contributor to revenue, claiming the top spot. 
 While not a close race we can see that direct traffic secures a noteworthy second place 
 in the revenue hierarchy. To gain a more comprehensive understanding, future analyses 
 should delve deeper into the specifics of each channel, considering factors such as, 
 but not limited to,  user engagement, marketing strategies, and customer behaviour. 
 These insights could provide a more in-depth perspective on the connection 
between channel dynamics and revenue generation for businesses of various sizes.

