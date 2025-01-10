## Data Pre-cleaning

Before importing all the data I checked the raw data. 

### all_sessions.csv

1. Two columns potentially could be keys: fullVisitorId and visitId. Both columns contained duplicates and NULLs. At this stage, for both columns I deleted NULLs.
2. All the raws with (not set) for both Country and City were deleted because it’s either a bot or improperly collected data. All other (not set) were replaced with NULL.
3. The cities with “not available in demo” were replaced with NULL; the same for (not set) in the City column.
4. ${escCatTitle} and (not set) for product category replaced by NULL.
5. Time format converted to proper format =INT(C2/100)/24+MOD(C2,100)/1440
6. Date format converted to proper format =DATE(LEFT(J2,4),MID(J2,5,2),RIGHT(J2,2))
7. Columns without headers and data deleted.
8. Columns with headers and without data deleted for instance Search Keyword and productRefundAmount. 


### products.csv 

1. Name was trimmed 
2. Format for quantity was fixed
3. Empty columns and rows are deleted
