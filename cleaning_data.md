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

1. Table contained 4 000 000+ rows. I assumed that there are a lof of duplicates

#number of rows and number of unique values for analytics

```sql
select count(*), count(distinct fullvisitorId), count(distinct userid), count(distinct visitId)
from public.analytics
```




2. UserID didn’t have any values, I dropped this column 

ALTER TABLE public.analytics DROP COLUMN userid


3. VisitStartTime column was duplicated VisitId column in all_sessions table. I dropped it.

4. socialEngagementType column contained only 1 value for all rows, I dropped it

select count(socialEngagementType) from public.analytics
where socialEngagementType='Not Socially Engaged'
