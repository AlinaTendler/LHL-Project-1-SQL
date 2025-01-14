# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
This is a training project. The main goal is to perform the following actions using new skills and knowledge:
  - create database and tables;
  - import data from CSV to the newly created database and tables;
  - clean data;
  - answer questions;
  - find new insights from the data;
  - QA data;
  - create ERD.


## Process
  1. Creating a database and tables. I created a database and tables using the pgAdmin interface, where I specified formats and constraints. 
  2. Downloading and examining CSV files: Before importing data into the database, I "pre-cleaned" it (trimmed, removed duplicates, etc.) in Google Spreadsheets. For more details, please take a look at cleaning_data.md.
  3. Cleaning and normalization (3NF): removing duplicated rows and columns, identifying Primary and Foreign keys, formatting, verifying data, and deleting improper data. Please take a look at cleaning_data.md.
  4. Answering questions:
     
     1) Which cities and countries have the highest level of transaction revenues on the site?
     3) What is the average number of products ordered from visitors in each city and country?
     4) Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?
     5) What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?
     6) Can we summarize the impact of revenue generated from each city/country?

  5. Looking for new insights from the data
## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)

## Challenges 
There was no documentation, so it was hard to understand what each column represented only by its name. Logically, visitorID should be a unique value in all_sessions and analytics, but analytics, for example, had over 4 000 000 duplicates by visitorID. 
 
## Future Goals
If it were a real task, I would doubt the data's reliability and thoroughly check how it was gained and processed. All the results I received are not reliable. Also, if I had more time, I would check "full-duplicates" instead of relying on logic and deleting duplicates by visitID. 

