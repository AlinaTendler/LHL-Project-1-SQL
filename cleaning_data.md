## Data Pre-cleaning

Before importing all the data I checked the raw data. 

### all_sessions.csv

1. Two columns potentially could be keys: fullVisitorId and visitId. Both columns contained duplicates and NULLs. At this stage, for both columns I deleted NULLs.
2. All the raws with (not set) for both Country and City were deleted because it’s either a bot or improperly collected data. All other (not set) were replaced with NULL.
3. The cities with “not available in demo” were replaced with NULL.
4. ${escCatTitle} and (not set) for product category replaced by NULL.
5. Time format converted to proper format =INT(C2/100)/24+MOD(C2,100)/1440
6. Date format converted to proper format =DATE(LEFT(J2,4),MID(J2,5,2),RIGHT(J2,2))
7. Columns without headers and data deleted.
8. Columns with headers and without data deleted (Search Keyword and productRefundAmount). 


### products.csv 

1. Name column was trimmed. 
2. Format for quantity was fixed.
3. Empty columns and rows were deleted.


 
## Cleaning
 
 
### analytics table

1. Table contained 4301122 rows. I assumed that there are a lot of duplicates

#number of rows and number of unique values for analytics

```sql
SELECT count(*), count(distinct fullvisitorId), count(distinct userid), count(distinct visitId)
FROM public.analytics
```
| count      | count-2      |count-3     | count-4     |
| ---------- | ------------ | ---------- |------------:|
| 4301122	   | 120018       | 0          | 148642      |

2. UserID didn’t have any values, so I dropped this column. 

```sql
ALTER TABLE public.analytics DROP COLUMN userid
```

3. Dropped visit_start_time, because it repeated visit_id

```sql
ALTER TABLE analytics
DROP COLUMN visit_start_time;
```

4. socialEngagementType column contained only 1 value for all rows, I dropped it

```sql
SELECT count(socialEngagementType) 
FROM public.analytics
Where socialEngagementType='Not Socially Engaged'
```

5. VisitorID should be a unique value, so I deleted all the duplicates using VisitorID

```SQL
DELETE T
FROM
(
SELECT *, DupRank = ROW_NUMBER()
OVER (
     PARTITION BY visitorid
     ORDER BY (SELECT NULL)
     )
FROM analytics
) AS T
WHERE DupRank > 1
```

7. Formatting dates

```SQL
ALTER TABLE public.analytics
ADD COLUMN proper_date date;
UPDATE public.analytics
SET proper_date = to_date(date::text, 'YYYYMMDD') 
SELECT date,proper_date FROM public.analytics
```
(then renamed and deleted old)

8. Unit_price divided by 1000000

```SQL
ALTER TABLE public.analytics
ADD COLUMN proper_unit_price float
UPDATE public.analytics
SET proper_unit_price = unit_price/1000000
```
(then renamed and deleted old)


9. Revenue divided by 1000000, without a new column

```SQL
UPDATE public.analytics SET revenue = revenue/1000000
```


### sales_report table 

1. All data from sales_report was in the products table, so I deleted sales_report 

```SQL
SELECT * 
FROM sales_report sr
LEFT JOIN products p
ON sr.product_sku = p.product_sku
WHERE p.product_sku IS NULL
```

```SQL
DROP TABLE sales_report
```

### sales_by_SKU

1. There were productsku in sales_by_sku not in products. I inserted those rows into products and deleted sales_by_sku.

```SQL
SELECT * 
FROM public.sales_by_SKU s
LEFT JOIN public.products p
ON s.productSKU = p.SKU
WHERE p.SKU the IS NULL
```

```SQL
INSERT INTO products (SKU, total_ordered)
SELECT s.productSKU, s.total_ordered
FROM public.sales_by_SKU s
LEFT JOIN products p ON s.productSKU = p.SKU
WHERE p.SKU IS NULL
```
```SQL
DROP TABLE sales_by_sku
```

