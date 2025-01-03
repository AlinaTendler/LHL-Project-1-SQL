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
SELECT SUM(units_sold*unit_price), city
FROM public.analytics an
JOIN public.all_sessions al ON an.visitId=al.visitId
GROUP BY city
HAVING SUM(units_sold*unit_price) IS NOT NULL
```

### Answer: 

**United States and Pittsburgh.**

The query is very  simple, but the problem is that we don't have enough data to provide the answer, *transactionRevenue* column in *all_sessions* table has only 4 values, and *totalTransactionRevenue* has only 80 values, which is more, but yet not enough to provide the answer. We could simply multiply *productQuantity* by *productPrice*, but *productQuantity* has only 53 values. 

The most complete information we probably can get is from the analytics table, by multiplying *units_sold* by *unit_price* (it has 2549 values). And it will give us United States with 1501.71 and Pittsburgh with 239.84 results.


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

| AVG_Number    | Country       |
| ------------- |:-------------:|
| 1.67          | United States |
| 1.00          | Egypt         |
| 1.00          | India         |




**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:



Answer:





**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:



Answer:





**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:



Answer:







