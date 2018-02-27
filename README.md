# Business Intelligence Tool Test

## Business Context
*Skip to the ***Dataset*** section if you are only interested in the setup process.*

### The problem
We need a BI tool in order to make our data reporting more robust and to remove our engineering team from analytics altogether. 

### Our current process
We currently run a Postgresql database. For analytics & data requests, we generally:
1) Write SQL queries using Navicat
2) Export the results of those queries to CSV
3) Manipulate the data and creating charts using Excel
4) Create a PowerPoint deck with those charts and sending it to clients
5) Repeat this process manually each week/month

Little-to-no forecasting and modeling is actually done, given that analyst time is spent on data reporting (which could be 95% automated).

### Ideal process
*This process informs our criteria for selecting a BI tool.*
1) Write SQL queries from within BI tool
2) Create charts & graphics within BI tool
3) Queries auto-run on given cadence, charts & graphics auto-refresh
4) Models & forecasts are created from within the BI tool, using the results of our queries as datasets
5) Analysis can be shared with clients and/or team members through 
  * a link to the BI tool's reports and 
  * PDF downloads

This allows analysts to spend their time creating models (i.e. higher-level thinking), rather than simply reporting on events that have already occurred. These insights can then be productized and ***sold*** to clients.

## Dataset
Ideally, we would link our exisiting databases to a variety of BI tools and test them directly in the workplace with real data.

However, because our data is sensitive and we need to remain HIPAA compliant, I had to grab some fake data to test.

I wanted a dataset that was large enough that we would need to link tables, 

download from Kaggle https://www.kaggle.com/hugomathien/soccer

### Dataset manipulation

SQLite visualization tool: DB Browser for SQLite --> Export to CSV

Postgres running locally

Homebrew pgloader

psql 

pgloader export sqlite db to soccer

SQLite DB

pgloder played nice with all of the tables except 'players'

changed csv data in height column to be an integer, deleted top row of csv

this gist for last table https://gist.github.com/nepsilon/f2937fe10fe8b0efc0cc

actual command

pg dump https://www.postgresql.org/docs/9.1/static/app-pgdump.html

pg_dump soccer > soccer_db.sql

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
