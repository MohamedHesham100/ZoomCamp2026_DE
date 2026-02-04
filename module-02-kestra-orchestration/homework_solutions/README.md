Module 2 Homework: Workflow Orchestration (Kestra)

This folder contains my detailed solutions and documentation for the Module 2 homework of the Data Engineering Zoomcamp 2026.
🛠️ Prepare the Data (Q3–Q5)

The goal of this module was to orchestrate data pipelines using Kestra to load NYC Taxi datasets (Yellow and Green) for the years 2020 and 2021 into an external storage (Azure/GCS) and then into a Data Warehouse.
Step 1: Set up Kestra & Flows

I used the provided Kestra flows to handle the ETL process. The flows were configured to:

    Extract data from the NYC TLC website.

    Upload the raw CSV/Parquet files to Azure Blob Storage.

    Load the data into a database for querying.

Step 2: Running Backfills

To get the data for the entire year of 2020, I utilized Kestra's Backfill feature:

    Yellow Taxi 2020: Backfill from 2020-01-01 to 2020-12-31.

    Green Taxi 2020: Backfill from 2020-01-01 to 2020-12-31.

📊 Homework Questions & Solutions
Question 1. Data Size

Within the execution engine, what is the value of the file output after the extract task?

    12.8 MB

    128 MB ✅

    1.28 GB

    128 KB

Solution: 128 MB By checking the Outputs tab in the Kestra UI for the Yellow Taxi flow execution, the file size for the extracted CSV is clearly shown as 128 MB.
Question 2. Rendered Value

What is the rendered value of the variable file when the inputs taxi is green, year is 2020, and month is 04?

    {{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv

    green_tripdata_2020-04.csv ✅

    green_tripdata_04-2020.csv

    green_tripdata_2020-04.csv.gz

Solution: green_tripdata_2020-04.csv Kestra uses Jinja templating. The expression {{inputs.taxi}}_tripdata_{{inputs.year}}-{{inputs.month}}.csv resolves by replacing the variables with their input values: green + _tripdata_ + 2020 + - + 04 + .csv.
Question 3. Yellow Taxi 2020 Data

How many rows are there for the Yellow Taxi data for all of 2020?

    13,537,299

    24,648,499 ✅

    18,324,219

    19,251,252

Solution: 24,648,499 Step: Run a SQL query on the table after completing the backfill for all 12 months of 2020.
SQL

SELECT COUNT(*) FROM yellow_tripdata WHERE filename LIKE 'yellow_tripdata_2020%';

Output: 24,648,499
Question 4. Green Taxi 2020 Data

How many rows are there for the Green Taxi data for all of 2020?

    5,327,301

    936,199

    1,734,051 ✅

    1,342,034

Solution: 1,734,051 Step: Similar to the previous question, query the Green Taxi table after the 2020 backfill.
SQL

SELECT COUNT(*) FROM green_tripdata WHERE filename LIKE 'green_tripdata_2020%';

Output: 1,734,051
Question 5. Yellow Taxi March 2021

How many rows are there for the Yellow Taxi data for March 2021?

    1,925,152 ✅

    1,428,092

    1,647,325

    2,093,031

Solution: 1,925,152 Step: Filter the count for the specific month of March 2021.
SQL

SELECT COUNT(*) FROM yellow_tripdata WHERE filename = 'yellow_tripdata_2021-03.csv';

Output: 1,925,152
Question 6. Schedule Trigger Configuration

How would you configure the timezone for a Schedule trigger?

    Add a timezone property set to America/New_York in the Schedule trigger configuration ✅

    Add a timezone property set to America/New_York in the Flow configuration

    Add a timezone property set to America/New_York in the core configuration

    Add a timezone property set to America/New_York in the task configuration

Solution Explanation: According to Kestra documentation, the timezone property is specifically part of the Schedule trigger plugin configuration to ensure the cron expression evaluates against the correct local time.