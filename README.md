## AIM

The aim of this project was to extract, load and transform a pipeline that used data from CSV files and an API. The results are then loaded into a Google Cloud Bucket. 

## Datasets

Transactions.csv



column name     ->  Type

txn_id          ->  String

user_id         ->  INT

amount          ->  INT

currency        ->  INT

txn_time        -> timestamp

I have not found any null values or duplicates within this data


clickstream.csv

column name     ->  Type

user_id         ->  INT

session_id      ->  INT

page_url        ->  INT

click_time      -> timestamp

device          -> string

location        ->  string

I have not found any null values or duplicates within this data



## Extraction

The csv files were read in chunks of 50,000 rows.

The currency rates are fetched from https://v6.exchangerate-api.com/v6/YOUR_API_KEY/latest/USD and is stored as a json file in data/raw/api_currency/YYYY-MM-DD/response.json



## Transformations


For both datasets, all of the timestamps were converted into UTC times. For the transactions dataset, the columns txn_id and txn_time were converted into transaction_id and transaction_time respectively. Additionally a new column  was added which converted the amount into USD using the rates found in the API.

## Loading

The transformed data was loaded into a Google Cloud Storage Bucket as a CSV file. Each of the datasets were loaded to separate files in chunks of 50000 rows.
They were saved as:
gs://week_1_bucke/clickstream/ingest_date==YYYY-MM-DD/clickstreampartX.csv
gs://week_1_bucke/transactions/ingest_date=YYYY-MM-DD/transactionspartX.csv


