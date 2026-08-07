# Canadian Flight Warehouse

##### Project objective
Build a data engineering platform that collects, transforms, archives, and analyzes flight data for major Canadian airports to identify operational trends such as:

departure and arrival volumes;
average delays by airport;
delays by airline;
cancellation rates;
periods of high activity;
airport comparisons;
daily, weekly, or monthly operational trends.

##### Repository Organisation
canadian-flight-warehouse/

│
├── ingestion/
│   ├── flights_api.py
│   ├── upload_to_s3.py
│
├── databricks/
│   ├── bronze/
│   │   ├── bronze_flights.py
│   │   ├── bronze_weather.py
│   │
│   ├── silver/
│   │   ├── silver_flights.py
│   │   ├── silver_weather.py
│   │
│   ├── gold/
│   │   ├── dim_airport.py
│   │   ├── dim_airline.py
│   │   ├── fact_flights.py
│
├── sql/
│   ├── top_delayed_airlines.sql
│   ├── airport_performance.sql
│
├── tests/
│
├── docs/
│
└── README.md

##### High-Level Architecture
                ┌─────────────────────┐
                │  AviationStack API  │
                └──────────┬──────────┘
                           │
                Python API Ingestion
                           │
                           ▼
                  AWS S3 (Raw Layer)
                  JSON / CSV / Parquet
                           │
                           ▼
                  Databricks Notebook
                     Bronze ETL
                           │
                    Delta Tables
                           │
                           ▼
                  Databricks Notebook
                     Silver ETL
                           │
                    Delta Tables
                           │
                           ▼
                  Databricks Notebook
                     Gold ETL
                           │
                    Delta Tables
                           │
                           ▼
                   SQL Analysis


##### Analytical questions to address:
Which airport has the highest daily flight volume?
Which airport has the highest delay rate?
Which airlines have the longest average delays?
Are delays more frequent in the morning, afternoon, or evening?
Which days of the week have the highest volume of operations?
What is the cancellation rate by airport?
What is the trend in delays over the last 30 or 90 days?
Are arrivals delayed more often than departures?
Which corridors are the busiest (e.g., YYZ-YUL or YVR-YYC)?
Can unusual operational peaks be detected?
