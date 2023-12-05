## **What issues will you address by cleaning the data?**
1. Missing or incorrect values
2. Duplicate entries
3. Whitespace in text data
4. Handling null values
5. Data type conversion
6. Scaling numeric values
7. Date format standarization




###Queries:
### **Below, provide the SQL queries you used to clean your data.**
##1. Cleaning "fixed_products" table
1.1 Create table
SQL QUERIES:
```
CREATE TABLE fixed_products AS
SELECT
    sr.product_sku AS product_sku,
    sr.name AS product_name,
    ss.product_price AS unit_price,
    sr.total_ordered AS unit_quantity
FROM some_sessions ss
JOIN sales_report sr USING (product_sku);
```
1.2 Remove rows without product sku
SQL QUERIES:
```
DELETE FROM fixed_products 
WHERE unit_price = 0 AND unit_quantity = 0;
```
1.3 Remove rows with no unit quantity
SQL QUERIES:
```
DELETE FROM fixed_products 
WHERE unit_quantity = 0;
```
1.4 Delete duplicate product skus with different prices
SQL QUERIES:
```
DELETE FROM fixed_products
WHERE (product_sku, unit_price) IN (
    SELECT
        product_sku, 
        unit_price
    FROM (
        SELECT 
            product_sku, 
            unit_price, 
            ROW_NUMBER() OVER(PARTITION BY product_sku 
                            ORDER BY count(*) DESC, unit_price) AS row_num
        FROM
            fixed_products
        GROUP BY
            product_sku, unit_price
    ) AS one_true_price
    WHERE row_num > 1
);
```
1.5 Calculate Total Revenue
SQL QUERIES:
```
SELECT DISTINCT(product_sku), product_name, unit_price, unit_quantity
FROM fixed_products
ORDER BY product_sku;
```

## 2. Cleaning Data in "sales_report" table
2.1 Remove whitespace from name column
SQL QUERIES:
```
UPDATE sales_report
SET name = TRIM(name);
```

## 3. Cleaning Data in "some_sessions" table
3.1 Create new table for cleaning
SQL QUERIES:
```
CREATE TABLE some_sessions AS
SELECT full_visitor_id, channel_grouping, country, city, total_transaction_revenue,
       date, visit_id, product_price, product_revenue, product_sku, v2_product_name, v2_product_category
FROM all_sessions;
```
3.2 Remove null/unavailble values from city and country
SQL QUERIES:
```
DELETE FROM some_sessions
WHERE city = 'not available in demo dataset' OR city = '(not set)';
```
3.3 Change date column type
SQL QUERIES:
```
ALTER TABLE some_sessions
ALTER COLUMN date TYPE DATE USING to_date(date, 'yyyymmdd');
```
3.4 Change numeric columns type
SQL QUERIES:
```
ALTER TABLE some_sessions 
ALTER COLUMN product_price TYPE float USING product_price::double precision,
ALTER COLUMN product_revenue TYPE float USING product_revenue::double precision,
ALTER COLUMN total_transaction_revenue TYPE float USING total_transaction_revenue::double precision;
```
3.5 Make numeric columns divisible by 1,000,000
SQL QUERIES:
```
UPDATE some_sessions
SET product_price = product_price/1000000, 
    product_revenue = product_revenue/1000000,
    total_transaction_revenue = total_transaction_revenue/1000000;
```

## 4. Cleaning Data in "products" Table
4.1 Remove whitespace from name column
SQL QUERIES:
```
UPDATE products
SET name = TRIM(name);
```

##  5. Cleaning Data in "analytics" table
5.1 Create new table for cleaning
SQL QUERIES:
```
CREATE TABLE clean_analytics AS
SELECT visit_id, visit_start_time, date, channel_grouping, units_sold, unit_price, revenue,
       full_visitor_id, page_views
FROM analytics;
```
5.2 Remove rows without revenue and unit price
SQL QUERIES:
```
DELETE FROM clean_analytics
WHERE revenue IS NULL AND unit_price IS NULL;
```
5.3 Remove rows without revenue and units sold
SQL QUERIES:
```
DELETE FROM clean_analytics
WHERE revenue IS NULL AND units_sold IS NULL;
```
5.4 Change unit price column type
SQL QUERIES:
```
ALTER TABLE clean_analytics
ALTER COLUMN unit_price TYPE float USING unit_price::double precision;
```
5.5 Make numeric columns divisible by 1,000,000
SQL QUERIES:
```
UPDATE clean_analytics
SET unit_price = unit_price/1000000,
    revenue = revenue/1000000;
```
