# Business Intelligence Tool Test

## Context
Skip to the [Dataset](#Dataset) BOLD section if you are only interested in running your own sample tests.

### Our current process
currently running a Postgresql database, most of the analytics is done by:
1) Writing queries using Navicat
2) Exporting the results of those queries to CSV
3) Manipulating the data and creating charts using Excel
4) Creating a PowerPoint deck with those charts and sending it to clients

### Ideal process

This process informs our criteria for a BI tool.

## Dataset
Ideally, we would link our exisiting databases to a variety of BI tools and test them directly in the workplace with real data.

However, because our data is sensitive and we need to remain HIPAA compliant, I had to grab some fake data to test.

I wanted a dataset that was large enough that we would need to link tables, 

download from Kaggle

### Dataset manipulation

SQLite visualization tool: DB Browser for SQLite --> Export to CSV

Postgres running locally

Homebrew pgloader

psql 

pgloader export sqlite db to soccer

SQLite DB

all of the tables worked except 'players'

changed csv data in height column to be an integer, deleted top row of csv

this gist for last table https://gist.github.com/nepsilon/f2937fe10fe8b0efc0cc

actual command

pg dump https://www.postgresql.org/docs/9.1/static/app-pgdump.html

now the soccer postgres db was running on my local machine

included all of these files in the zipped 'data' directory

### Linking to Mode

## Testing the BI tool's capabilities

Things we're testing:
- SQL querying ease and speed
  1. query 1
  2. query 2
  3. query 3
- Visualizations and dashboards built from simple SQL queries
  - build viz's based on the previous queries
  - goals by league by year
- Forecasting 
  - Regression analysis / Python, etc.
  
## Results 
Note that these are from Mode:
- time from queries
- paste visualizations
- etc.
