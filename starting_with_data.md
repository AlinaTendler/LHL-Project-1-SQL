## Question 1: What products are the most popular? Top-5

### SQL Queries:

```SQL
SELECT distinct productSKU, orderedQuantity, p.name 
FROM public.all_sessions a
JOIN public.products p ON productSKU = SKU
GROUP BY productSKU, order quantity, p.name
ORDER BY orderedQuantity DESC
```

### Answer: 

Top-5 products by Popularity:

"Kick Ball"
"Kick Ball"
"22 oz Water Bottle"
"22 oz Water Bottle"
"22 oz Water Bottle"
"Sunglasses"
"Sunglasses"
"Sunglasses"
"Spiral Journal with Pen"
"Custom Decals"


## Question 2: What products generate the most revenue? Top-5

### SQL Queries:

```SQL
SELECT distinct productSKU, productPrice, orderedQuantity, SUM(productPrice*orderedQuantity), p.name 
FROM public.all_sessions a
JOIN public.products p ON productSKU = SKU
GROUP BY productSKU, productPrice, orderedQuantity, p.name
ORDER BY SUM(productPrice * orderedQuantity) DESC
```

### Answer:

Top-5 products by Revenue:

"Cam Outdoor Security Camera - USA"
"Learning Thermostat 3rd Gen-USA - Stainless Steel"
"Cam Indoor Security Camera - USA"
"Cam Outdoor Security Camera - USA"
"Cam Indoor Security Camera - USA"

## Question 3: Is our data results for cities/countries reliable?

### SQL Queries:

```SQL
SELECT count(*), count(country) AS country, count(city) AS city
FROM public.all_sessions
```
### Answer:
14172 - rows overall,	14172 - rows with non-null country, and only 6069 rows with non-null city. We can't rely on any results based on the city.

## Question 4: Is everything okay with URLs in our data?

### SQL Queries:

```SQL
SELECT count (pagePathLevel1) from public.all_sessions
where pagePathLevel1 = '/google+redesign/'
```

### Answer:
13401 rows out of 14172 have the same value '/google+redesign/'. So we can't use this column for analyze.

## Question 5: 

### SQL Queries:

Answer:
