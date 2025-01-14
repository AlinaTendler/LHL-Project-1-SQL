## Question 1: Which cities and countries have the highest level of transaction revenues on the site?

### SQL Query: 

```sql
SELECT SUM(units_sold*unit_price), country
FROM public.analytics an
JOIN public.all_sessions al ON an.visitId=al.visitId
GROUP BY country
HAVING SUM(units_sold*unit_price) IS NOT NULL
```
```sql
SELECT AVG(units_sold)::numeric(10,2), city
FROM public.analytics an
JOIN public.all_sessions al ON an.visitId=al.visitId
GROUP BY city
HAVING AVG(units_sold) IS NOT NULL
ORDER BY AVG(units_sold) DESC
```

### Answer: 

**United States and Pittsburgh.**

The query is very  simple, but the problem is that we don't have enough data to provide the answer, *transactionRevenue* column in *all_sessions* table has only 4 values, and *totalTransactionRevenue* has only 80 values, which is more, but yet not enough to provide the answer. We could simply multiply *productQuantity* by *productPrice*, but *productQuantity* has only 53 values. 

The most complete information we can probably get from the analytics table, by multiplying *units_sold* by *unit_price* (it has 2549 values). And it will give us United States with 1501.71 and Pittsburgh with 239.84 results.


## Question 2: What is the average number of products ordered from visitors in each city and country?

### SQL Query:

```sql
SELECT AVG(units_sold), country
FROM public.analytics an
JOIN public.all_sessions al ON an.visitId=al.visitId
GROUP BY country
HAVING AVG(units_sold) IS NOT NULL
```
```sql
SELECT AVG(units_sold), city
FROM public.analytics an
JOIN public.all_sessions al ON an.visitId=al.visitId
GROUP BY city
HAVING AVG(units_sold) IS NOT NULL
```

Answer:

| AVG_Number | Country       |
| ---------- |:-------------:|
| 1.67       | United States |
| 1.00       | Egypt         |
| 1.00       | India         |

| AVG_Number | City          |
| ---------- |:-------------:|
| 4.00	     | Pittsburgh    |
| 2.00	     | Houston       |
| 1.00       | Palo Alto     |
| 1.00	     | San Bruno     |
| 1.00	     | Ann Arbor     |
| 1.00	     | Mountain View |
| 1.00	     | Sunnyvale     |
| 1.00	     | Austin        |
| 1.00	     | San Francisco |


## Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?
## +Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?
### SQL Query:

```sql
CREATE VIEW questions3and4 AS
SELECT country, city, productQuantity, productPrice, v2ProductCategory, productSKU
FROM public.all_sessions
WHERE productQuantity > 0
```
```sql
SELECT productSKU, country, city, productQuantity, productPrice, v2ProductCategory, p.name
FROM questions3and4 q3 
INNER JOIN products p
ON q3."productSKU" = "SKU"
ORDER BY country
```

### Answer:
Visitors from USA order many security cameras, thermostats and smoke alarms.


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

```sql
SELECT country, ROUND(SUM(productQuantity*productPrice) / (SELECT SUM(productQuantity*productPrice) FROM all_sessions), 2) AS proportion_of_all_revenues
FROM all_sessions
GROUP BY country
ORDER BY proportion_of_all_revenues DESC, country
```


```sql
SELECT country, ROUND(SUM(productQuantity*productPrice) / (SELECT SUM(productQuantity*productPrice) FROM all_sessions), 2) AS proportion_of_all_revenues
FROM all_sessions
GROUP BY city
ORDER BY proportion_of_all_revenues DESC, city
```

Answer: USA brings 93% of revenue, 2% Argentina, Ireland, Spain and 1% - Canada. 
        Mountain View brings 13%, Salem - 11%, New York - 10%.







