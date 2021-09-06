# mlpp-etl

## Data Collection & ETL Assignment Discussion
jandre

### Part 1: Get Data
*"Pick a US state and (write a script to) download the most recent ACS data (at the block group level) for every block group in that state."*

For this step, I first decided which data I wanted to retrieve. Though this is a standalone exercise, I imagined that I was doing these tasks within a broader project context. I decided to obtain ACS 2019 5-year estimates (2015-2019) because these are the [most recent](https://www.census.gov/newsroom/press-kits/2020/acs-5-year.html) 5-year estimates available. Though these data points are less recent than 1- or 3-year estimates, there are larger sample sizes allowing for greater [precision](https://www.census.gov/programs-surveys/acs/guidance/estimates.html). Additionally, data for areas of [all population sizes](https://www.census.gov/programs-surveys/acs/guidance/estimates.html) are available with these estimates. I chose to look at the block-group level in my current state, Pennsylvania.

I used Census documentation to decide which [variables](https://api.census.gov/data/2019/acs/acs5/variables.html) to include. I selected 7 variables covering population-level educational attainment, and household-level poverty status and access to broadband internet. I can imagine a study analyzing the associations between these attributes and geographic variation across Pennsylvania block groups. For example, potential use cases could be to analyze the relationship between educational attainment and poverty status, or to identify hotspots of high poverty status or low access to broadband for policy intervention. For this exercise, I did not include margins of error.

Next, I decided how to retrieve the data. For the sake of practicing baseline python skills, I decided to retrieve the data by querying the Census API directly. I utilized Census [documentation](https://www.census.gov/programs-surveys/acs/guidance/handbooks/api.html) to determine how to format my query. The documentation revealed that an API key is not necessary for small/infrequent requests, so I did not obtain a key for this exercise. I then used the requests and json packages to query the API and format the results. To make my code generalizable, the URL is constructed using flexible parameters. I can change the year, estimates, state, and variables in one section of the code, and these changes are automatically applied to the query and resulting data.

### Part 2: Transform/Prep
*"Once you've downloaded the data, get it ready (with python code) so that it can be loaded into a postgres database table and is in a usable format for downstream applications."*

In this step, I used pandas to transform the data into a more usable format. First, I added the data to a data frame and set the headers. Next, I renamed the variables from codes to descriptive text using the variables parameter I included in part 1, and rearranged the order of the columns. Finally, I converted data types as necessary (from objects to integers in this case). Along the way, I ran sanity checks by viewing and describing the data.

### Part 3: Load
*"[Load] the data you've downloaded into a postgres database table."*

For this step, I employed the sqlalchemy (create_engine), psycopg2, ohio.ext.pandas, and os packages to connect to the database and load the data.
First, I stored the database connection keys (user, password, etc.) within environment variables on my system for security purposes. I could then load them directly into the connection syntax. Next, I created a SQL query to first drop my table if it already exists, and then create my table with the variables as defined. After creating an engine and connecting to the database, the code executes the table creation query and then copies my data frame to the table. I chose this method using [sources](https://dssg.github.io/hitchhikers-guide/curriculum/1_getting_and_keeping_data/csv-to-db/) that [indicate](https://github.com/dssg/ohio) these methods are much faster than other writing methods. Finally, I queried my newly-created table to print out a few rows in order to check that the data are properly stored.

