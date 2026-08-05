# canadian-flight-warehouse
Personal Portfolio Data Engineering project aiming to analyze flight operations trends in Canada's four majour airports


canadian-flight-warehouse/

│
├── ingestion/
│   ├── flights_api.py
│   ├── weather_api.py
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
│   ├── weather_analysis.sql
│   ├── airport_performance.sql
│
├── tests/
│
├── docs/
│
└── README.md
