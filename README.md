# Business Intelligence Tool Test
How to set up a locally hosted Postgres DB populated with useful public data, then connect a BI tool to your DB.

## Business Context
*Skip to the **Dataset** section if you are only interested in the setup process.*

### The problem
We need a BI tool in order to make our data reporting more robust and to remove our engineering team from analytics altogether. 

### Our current process
We run a PostgreSQL database. For analytics & data requests, we generally:
1. Write SQL queries using Navicat
2. Export the results of those queries to CSV
3. Manipulate the data and creating charts using Excel
4. Create a PowerPoint deck with those charts and sending it to clients
5. Repeat this process manually each week/month

Little to no forecasting and modeling is actually done, given that analyst time is spent on data reporting (which could be 95% automated).

### Ideal process
*This process informs our criteria for selecting a BI tool.*
1. Write SQL queries from within BI tool
2. Create charts & graphics within BI tool
3. Queries auto-run on given cadence, charts & graphics auto-refresh
4. Models & forecasts are created from within the BI tool, using the results of our queries as datasets
5. Analysis can be shared with clients and/or team members through 
    1. a link to the BI tool's reports and 
    2. PDF downloads

This allows analysts to spend their time creating models (i.e. higher-level thinking), rather than simply reporting on events that have already occurred. These insights can then be productized and **sold** to clients.

## Dataset
Ideally, we would link our exisiting databases to a variety of BI tools and test them directly in the workplace with real data.

However, because our data is sensitive and we need to remain HIPAA compliant, this was not an option for testing. (Once a BI tool is implemented, the provider will sign a BAA.)

I wanted a dataset that was large enough that we would need to link tables, but small enough to easily understand and run on a local machine.

[This is the dataset I used](https://www.kaggle.com/hugomathien/soccer). It is a SQLite database of European Professional Football (i.e. soccer) matches, players, and teams.

### Database setup

#### The easy way to do this
Because I've manipulated all of this data already, you can import my dump of the Postgres DB. 

Make sure Postgres is running, then download the *`data.zip`* file from this repo. Navigate to the file directory and run: 

```psql your_created_DB_name < soccer_db.sql```

#### From scratch
I downloaded the data from Kaggle, which gave me a *`.sqlite`* file. 

I then used [DB Browser for SQLite](http://sqlitebrowser.org/) to visualize the data structures. I also used this tool to export all of the tables to CSV - convenient if you'd rather explore the data in Excel.

I ensured that I had [Postgres installed and running locally](https://postgresapp.com/).
* Create a new Postgres DB for your data: `createdb soccer`

From there, I used [pgloader](https://github.com/dimitri/pgloader) to migrate the data from my SQLite DB to Postgres.
* Install pgloader: `brew install pgloader`
* Export your SQLite DB to Postgres: `pgloader ./path/to/sqlite.db postgresql:///soccer`

pgloader played nice with all of the DB tables except 'player'. For this table, I went into the *`Player.csv`* file. I changed the data in the *'height'* column to be integers and deleted the top row (headers) of the CSV.

I then used [this gist](https://gist.github.com/nepsilon/f2937fe10fe8b0efc0cc) to import the edited *`Player.csv`* file into the blank table in my Postgres DB.

Once my Postgres DB was built, I ran `pg_dump` [to dump the results into a *`.sql`* file](https://www.postgresql.org/docs/9.1/static/app-pgdump.html).

The SQL dump, SQLite DB, and all of the CSV files are included in *`data.zip`*.

### Linking to your BI tool
*Note that some of these instructions may be specific to [Mode Analytics](https://modeanalytics.com/), the BI tool I was testing.*

I connected my DB to Mode using a [bridge connection](https://help.modeanalytics.com/articles/connect-with-bridge/).

When I first attempted to connect, I got an error that SSL was not enabled on my DB. 

#### Enable SSL in Postgres
1. Generate a passphrase protected certificate: 
    ```
    openssl req -new -text -out cert.req
    ```
2. Remove the passphrase: 
    ```
    openssl rsa -in privkey.pem -out cert.pem
    ```
3. Convert the certificate into a self-signed certificate: 
    ```
    openssl req -x509 -in cert.req -text -key cert.pem -out cert.cert
    ```
4. Move your *`cert.pem`* and *`cert.cert`* files to the Postgres data directory. You can find the Data Directory by clicking "Server Settings" in the Postgres GUI.

5. Navigate to your data directory and change permissions:
    ```
    chmod 600 cert.pem
    chmod 600 cert.cert
    ```
6. Open your *`posgresql.conf`* file (again found in "Server Settings") and uncomment and change these 3 lines of code:
    ```
    ssl=on
    ssl_cert_file = 'cert.cert'
    ssl_key_file = 'cert.pem'
    ```
7. Restart Postgres

Your database now should be set up to connect using Mode's instructions!

## Testing the BI tool's capabilities
Now that you have your BI tool layered over your Postgres DB, you're ready to play with the data! Some sample tests you could run to explore your BI tool's capabilities:

##### SQL Query & Data Visualization Ideas
* Rank the teams by goals scored by league by season
* Rank the teams by goals allowed by league by season
* Rank the leagues on total goals scored each season and explore how this changed over time

##### Modeling Ideas
* Develop a model to predict a player's overall rating based on their attributes
* Predict the outcome of games!
* More information and ideas can be found on the [dataset's Kaggle page](https://www.kaggle.com/hugomathien/soccer)
  
## To-do
Add the queries & analysis that I used to this repo.
  
## Notes
I created this guide because implementing this process took a lot of Googling and trial & error. I figured it would be useful to compile what I learned into a step-by-step guide. If it is helpful to you, drop me a line!
