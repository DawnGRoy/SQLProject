### **What are your risk areas? Identify and describe them.**
1. Data integrity: Risk of unintentionally removing relevant data or introducing errors during updates which impact the integreity of the dataset
2. Null handing: Deleting rows based on null conditions can lead to unintentional dataloss. 
3. Data type conversion: If data doesn't adhear to expected format it may lead to unexpected outcomes.
4. Inadequate documentation: lack of comprehensive and explicit documentation may lead to problems understanding the rationale behind specific decisions. 

### QA Process:
### **Describe your QA process and include the SQL queries used to execute it.**

## QA Check 1: Matching Values in "sales_report" and "products"
```
SELECT sr.product_sku, sr.total_ordered, sr.name,
       p.product_sku, p.name, p.ordered_quantity, sbs.product_sku, sbs.total_ordered
FROM sales_report sr
JOIN products p USING (product_sku)
JOIN sales_by_sku sbs USING (product_sku);
```

## QA Check 2: Compare Product Prices in "sales_report" and "some_sessions"
```
SELECT sr.product_sku, sr.total_ordered, sr.name,
       ss.product_sku, ss.v2_product_name, ss.product_price
FROM sales_report sr
JOIN some_sessions ss USING (product_sku);
```

## QA Check 3: Matching Values in "clean_category" and Other Tables
```
SELECT cc.v2_product_category, cc.product_sku, cc.product_price, cc.product_quantity, cc.product_revenue,
       cc.date, cc.country, cc.city, cc.visit_id,
       sa.units_sold, sa.unit_price, sa.revenue,
       p.name AS product_name,
       sr.total_ordered AS sales_report_total_ordered,
       sbs.total_ordered AS sales_by_sku_total_ordered,
       p.ordered_quantity AS products_ordered_quantity
FROM clean_category cc
JOIN some_sessions sa USING (visit_id)
JOIN sales_report sr USING (product_sku)
JOIN sales_by_sku sbs USING (product_sku)
JOIN products p USING (product_sku);
```

## QA FOR QUESTION 3. Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?
```
-- Create new table
CREATE TABLE clean_category AS
	SELECT
		v2_product_category,
		product_sku,
		product_price,
		product_quantity,
		product_revenue,
		date,
		country,
		city,
		visit_id
	FROM some_sessions
-- Get rid of data irrelevant to our question. (ie. no category)
DELETE FROM clean_category WHERE v2_product_category = '(not set)'
-- Clean up and organize data based on broader category
UPDATE clean_category
UPDATE clean_category
SET v2_product_category = 
CASE 
WHEN v2_product_category LIKE '%Waze%' THEN 'Waze'
WHEN v2_product_category LIKE '%Housewares%' THEN 'Housewares'
WHEN v2_product_category LIKE '%Bag%' THEN 'Bags'
WHEN v2_product_category LIKE '%Apparel%' THEN 'Apparel'
WHEN v2_product_category LIKE '%Drinkware%' THEN 'Drinkware'
WHEN v2_product_category LIKE '%Electronics%' THEN 'Electronics'
WHEN v2_product_category LIKE '%Office%' THEN 'Office'
WHEN v2_product_category LIKE '%Accessories%' THEN 'Accessories'
WHEN v2_product_category LIKE '%Lifestyle%' THEN 'Lifestyle'
WHEN v2_product_category LIKE '%Brand%' THEN 'Brand'
WHEN v2_product_category LIKE '%Nest%' THEN 'Nest'
WHEN v2_product_category LIKE '%Fruit%' THEN 'Fruit Games'
WHEN v2_product_category LIKE '%Spring%' THEN 'Spring Sale'
WHEN v2_product_category LIKE '%Gift%' THEN 'Gift Cards'
WHEN v2_product_category LIKE '%Clearance Sale%' THEN 'Clearance Sale'
WHEN v2_product_category LIKE '%Fun%' THEN 'Fun'
ELSE v2_product_category
END
```