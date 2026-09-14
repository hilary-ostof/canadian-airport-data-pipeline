# ✈️ Canadian Airport Operations Data Engineering Pipeline

An end-to-end AWS data engineering project that collects, processes, models, and analyzes flight operations data from major Canadian airports.

The project demonstrates the design and implementation of a cloud-based ETL/ELT pipeline using **Python, Amazon S3, AWS Lambda, Amazon EventBridge, AWS Step Functions, Amazon Redshift, and SQL**, with a focus on data warehousing, dimensional modeling, historical tracking, and analytics-ready datasets.

---

# 👤 Author

### Hilary Fotso

**DataOps Engineer | Aspiring Data Engineer**

I started this project as part of my transition and continued development toward a career in **Data Engineering**. I wanted to move beyond learning individual tools and concepts in isolation and build a project that reflects how data engineering problems are approached in a real-world environment.

I chose Canadian airport and flight operations data because it provides an interesting engineering challenge: data comes from multiple sources, flight information changes throughout the day, historical changes can be valuable for analysis, and the resulting data naturally lends itself to dimensional modeling and analytical workloads.

Through this project, I wanted to strengthen and demonstrate my ability to:

* Design an end-to-end data pipeline rather than only write standalone scripts
* Use **Python and SQL** for data extraction, transformation, validation, and analysis
* Build cloud-based data infrastructure using **AWS**
* Design a **data lake and data warehouse architecture**
* Apply concepts such as **Medallion Architecture, dimensional modeling, and SCD Type 2**
* Make engineering decisions based on trade-offs between **cost, complexity, scalability, and business requirements**
* Document my technical decisions and explain not only **what** I built, but **why** I built it that way

Most importantly, this project represents my approach to learning Data Engineering: **building practical systems, encountering real constraints, making architectural decisions, and improving the solution as my skills develop.**

📍 Montréal QC, Canada
💼 **LinkedIn:** https://www.linkedin.com/in/hilary-fotso-2a9180193/
💻 **GitHub:** https://github.com/hilary-ostof
---

## 📌 Project Overview

Canadian airports process hundreds of departures and arrivals every day, while flight information continuously changes as flights are delayed, cancelled, rescheduled, or completed.

This project builds a data pipeline to collect and analyze operational flight data from four of Canada's busiest airports:

* **YYZ** — Toronto Pearson International Airport
* **YVR** — Vancouver International Airport
* **YUL** — Montréal–Trudeau International Airport
* **YYC** — Calgary International Airport

Flight information is extracted several times per day to capture changes in operational status.

The pipeline ingests raw flight data into an Amazon S3 data lake, transforms and validates it using Python and AWS Lambda, and loads analytics-ready data into Amazon Redshift using a **Medallion Architecture**.

Historical changes to flight information are preserved using **Slowly Changing Dimension Type 2 (SCD2)** concepts where appropriate.

---

# 🎯 Project Objectives

The main objective of this project is to demonstrate practical data engineering skills through a realistic cloud-based pipeline.

The project focuses on:

* API data ingestion with Python
* Cloud data lake design with Amazon S3
* Serverless data processing with AWS Lambda
* Pipeline scheduling with Amazon EventBridge
* Workflow orchestration with AWS Step Functions
* Data warehousing with Amazon Redshift
* Medallion Architecture implementation
* Dimensional data modeling
* Slowly Changing Dimension Type 2 implementation
* Data quality and transformation with Python and SQL
* Building analytics-ready datasets for airport operations analysis
---

# 🚧 Project Status

**Currently in development.**

Current implementation focuses on:

```text
✓ Flight API exploration
✓ Python extraction
✓ Canadian airport selection
✓ Data source identification
□ S3 ingestion pipeline
□ EventBridge scheduling
□ Step Functions orchestration
□ Redshift Bronze layer
□ Redshift Silver layer
□ SCD Type 2 implementation
□ Dimensional model
□ Gold analytics layer
□ Data quality tests
□ Architecture documentation
```

The README and architecture will evolve as implementation progresses.

---

# 🏗️ Architecture

```text
                         ┌─────────────────────┐
                         │   Flight Data API   │
                         └──────────┬──────────┘
                                    │
                             Python Extraction
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │      Amazon S3      │
                         │     Raw Landing     │
                         └──────────┬──────────┘
                                    │
                              AWS Lambda
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   Amazon Redshift   │
                         │                     │
                         │   Bronze Layer      │
                         │        ↓            │
                         │   Silver Layer      │
                         │        ↓            │
                         │    Gold Layer       │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         Analytics / SQL Queries


       EventBridge ─────► Scheduled Pipeline Execution

       Step Functions ──► Workflow Orchestration
```

### Data Flow

```text
Flight API
    ↓
Python Extractor
    ↓
Amazon S3
    ↓
AWS Lambda
    ↓
Amazon Redshift Bronze
    ↓
Data Cleaning / Transformation
    ↓
Amazon Redshift Silver
    ↓
Dimensional Models + Business Logic
    ↓
Amazon Redshift Gold
    ↓
Analytics
```

---

# ☁️ AWS Services

| Service                | Purpose                                        |
| ---------------------- | ---------------------------------------------- |
| **Amazon S3**          | Raw data lake and landing zone                 |
| **AWS Lambda**         | Serverless extraction and transformation logic |
| **Amazon EventBridge** | Scheduled execution of flight API ingestion    |
| **AWS Step Functions** | Pipeline workflow orchestration                |
| **Amazon Redshift**    | Cloud data warehouse                           |
| **AWS IAM**            | Access control between AWS services            |
| **Amazon CloudWatch**  | Pipeline logging and monitoring                |

The architecture intentionally uses serverless AWS components where possible to keep infrastructure relatively simple and cost-efficient.

---

# 📥 Data Sources

## Flight Operations Data

Flight information is collected from airport/API endpoints for:

```text
YYZ
YVR
YUL
YYC
```

Both **departures and arrivals** are collected.

The extraction process focuses on flights occurring during the current operational day:

```text
00:00 → 23:59
```

Rather than continuously polling the API, the pipeline runs periodically throughout the day.

This allows the project to capture operational changes while limiting unnecessary API calls and storage costs.

Typical flight attributes include:

```text
flight_number
airline
origin_airport
destination_airport
scheduled_departure
actual_departure
scheduled_arrival
actual_arrival
flight_status
terminal
gate
aircraft
```

The exact attributes available depend on the source API.

---

# 🌎 Reference Data

Static or slowly changing reference datasets are also incorporated into the pipeline.

These datasets provide additional context for flight records.

### Airports

Contains airport-level information such as:

```text
airport_code
airport_name
city
country
latitude
longitude
timezone
```

### Airlines

Contains airline information such as:

```text
airline_code
airline_name
country
```

### Countries

Provides standardized country information used by airport and airline dimensions.

### Cities / Regions

Geographical reference data can be used to enrich airport information and support regional analysis.

These datasets are loaded separately from the frequently changing flight data because they change much less often.

---

# 🪣 S3 Data Lake

Amazon S3 acts as the **raw landing zone** for the project.

Raw API responses are stored before transformation so that the original source data remains available for debugging, reprocessing, and auditing.

Example structure:

```text
s3://canadian-airport-data/

├── flights/
│   ├── year=2026/
│   │   ├── month=09/
│   │   │   ├── day=14/
│   │   │   │   ├── airport=YUL/
│   │   │   │   │   ├── departures/
│   │   │   │   │   └── arrivals/
│   │   │   │   ├── airport=YYZ/
│   │   │   │   ├── airport=YVR/
│   │   │   │   └── airport=YYC/
│
├── reference/
│   ├── airports/
│   ├── airlines/
│   ├── countries/
│   └── regions/
```

Partitioning the S3 keys by date and airport makes the raw data easier to navigate and process.

---

# 🥉🥈🥇 Medallion Architecture

The Redshift warehouse follows a **Bronze → Silver → Gold** architecture.

## 🥉 Bronze Layer — Raw Warehouse Data

The Bronze layer contains data loaded from S3 with minimal transformation.

Its purpose is to preserve the source structure while making the data accessible through SQL.

Typical operations include:

* Loading raw flight records
* Adding ingestion timestamps
* Recording source airport
* Recording extraction timestamp
* Preserving source values

Example metadata fields:

```text
ingestion_timestamp
source_airport
source_file
extraction_timestamp
```

---

## 🥈 Silver Layer — Cleaned & Standardized Data

The Silver layer contains validated and standardized records.

Transformations may include:

* Data type conversion
* Timestamp standardization
* Airport code normalization
* Airline code normalization
* Duplicate removal
* Null handling
* Invalid record filtering
* Flight identifier generation
* Status standardization
* Reference-data enrichment

The Silver layer becomes the trusted source for downstream modeling.

---

## 🥇 Gold Layer — Analytics Models

The Gold layer contains business-ready dimensional models optimized for analytical queries.

The warehouse follows a **star-schema-oriented design**.

Potential models include:

```text
fact_flights

dim_airport
dim_airline
dim_date
dim_time
dim_flight_status
dim_location
```

The Gold layer supports analysis such as:

* Flight volume by airport
* Departure vs. arrival volume
* Airline market share
* On-time performance
* Average departure delay
* Average arrival delay
* Cancellation rates
* Airport traffic patterns
* Peak operating hours
* Domestic vs. international traffic

---

# 🔄 Handling Flight Status Changes with SCD Type 2

Flight information is not static.

For example, the same flight may progress through several states:

```text
Scheduled
    ↓
Delayed
    ↓
Boarding
    ↓
Departed
    ↓
Landed
```

Simply overwriting the previous record would remove valuable historical information.

For this reason, the project implements **Slowly Changing Dimension Type 2 (SCD2)** logic for attributes where maintaining history is analytically useful.

Example:

| Flight | Status    | Valid From | Valid To | Current |
| ------ | --------- | ---------- | -------- | ------- |
| AC123  | Scheduled | 08:00      | 09:15    | False   |
| AC123  | Delayed   | 09:15      | 11:02    | False   |
| AC123  | Departed  | 11:02      | NULL     | True    |

Typical SCD2 fields include:

```text
effective_from
effective_to
is_current
```

This design makes it possible to reconstruct how a flight's operational information changed throughout the day.

---

# ⏱️ Data Extraction Strategy

Flight data is extracted multiple times during the day rather than every few minutes.

Example schedule:

```text
00:00
06:00
12:00
18:00
```

The exact schedule can be adjusted depending on API limits and project requirements.

Each execution retrieves the current day's flights for:

```text
YUL
YYZ
YVR
YYC
```

including:

```text
Departures
Arrivals
```

Only the relevant daily flight collection is retained from API responses when the source endpoint also returns unnecessary previous- or next-day information.

This reduces:

* API payload processing
* S3 storage
* Lambda execution time
* unnecessary downstream data volume

---

# ⚙️ Pipeline Orchestration

## Amazon EventBridge

EventBridge is responsible for triggering the pipeline on a schedule.

```text
EventBridge
      ↓
Step Functions
      ↓
Lambda Extraction
      ↓
S3
      ↓
Transformation / Loading
      ↓
Redshift
```

## AWS Step Functions

Step Functions coordinates the different stages of the pipeline and provides visibility into execution state.

A workflow may include:

```text
Start
  ↓
Extract Flight Data
  ↓
Validate API Response
  ↓
Write Raw Data to S3
  ↓
Load Bronze
  ↓
Transform Silver
  ↓
Apply SCD2 Logic
  ↓
Build / Refresh Gold Models
  ↓
Success
```

Failure states can be routed to logging and monitoring mechanisms for troubleshooting.

---

# 🐍 Python

Python is primarily used for extraction and data-processing logic.

Example responsibilities include:

```text
API requests
JSON parsing
CSV processing
Data validation
Data normalization
S3 interaction
Error handling
Logging
```

Libraries used may include:

```text
requests
json
pandas
boto3
datetime
```

---

# 🗄️ SQL

SQL is used extensively inside Amazon Redshift for:

* Data transformation
* Data validation
* Deduplication
* SCD Type 2 processing
* Dimensional modeling
* Fact table creation
* Aggregation
* Analytical queries

Example analytical question:

```sql
SELECT
    airport_code,
    COUNT(*) AS total_departures,
    AVG(departure_delay_minutes) AS avg_delay_minutes
FROM gold.fact_flights
WHERE flight_date BETWEEN '2026-06-01' AND '2026-09-01'
GROUP BY airport_code
ORDER BY total_departures DESC;
```

---

# 📁 Repository Structure

```text
canadian-airport-data-pipeline/
│
├── README.md
│
├── architecture/
│   └── architecture_diagram.png
│
├── src/
│   ├── extraction/
│   │   ├── flight_extractor.py
│   │   └── reference_data_loader.py
│   │
│   ├── transformation/
│   │   ├── clean_flights.py
│   │   ├── validate_flights.py
│   │   └── scd2.py
│   │
│   └── utils/
│       ├── aws_utils.py
│       └── logging_utils.py
│
├── sql/
│   ├── bronze/
│   ├── silver/
│   ├── gold/
│   └── analytics/
│
├── step_functions/
│   └── pipeline_definition.json
│
├── sample_data/
│   └── README.md
│
├── tests/
│
└── docs/
    ├── data_dictionary.md
    └── data_model.md
```

---

# 📊 Example Analytics

Once the Gold layer is populated, the dataset can answer questions such as:

**Which airport handles the most flights?**

```text
YYZ vs YVR vs YUL vs YYC
```

**Which airlines operate the most flights from major Canadian airports?**

**Which airports experience the highest average departure delays?**

**What percentage of flights are delayed or cancelled?**

**Which hours of the day experience the highest flight volume?**

**Are arrival delays or departure delays more significant?**

**How does operational performance differ across airlines?**

**How frequently does a flight's status change before departure?**

These analyses demonstrate how raw operational data can be transformed into business-relevant information.

---

# 🔐 Security & Configuration

Credentials and secrets are **not stored in the repository**.

API keys and AWS configuration should be handled through secure mechanisms such as:

* Environment variables
* AWS IAM roles
* AWS Secrets Manager

Example:

```text
FLIGHT_API_KEY
AWS_REGION
S3_BUCKET_NAME
REDSHIFT_HOST
```

Sensitive files should be excluded through `.gitignore`.

---

# 📈 Future Improvements

This project intentionally focuses on a relatively simple AWS data engineering architecture.

Potential future improvements include:

**dbt**

Move warehouse transformation logic into modular, tested, documented dbt models.

**Apache Airflow**

Introduce more advanced DAG-based orchestration.

**AWS Glue**

Support larger-scale distributed ETL workloads as data volume increases.

**Apache Kafka / Amazon Kinesis**

Move from periodic batch ingestion toward real-time flight event processing.

**Databricks / Apache Spark**

Introduce distributed processing for larger datasets.

**Weather Data**

Combine airport operations with historical or real-time weather information to investigate relationships between weather conditions and delays.

**CI/CD**

Add automated testing and deployment through GitHub Actions.

**Data Quality Framework**

Introduce automated validation and data-quality checks throughout the pipeline.

---

# 🧠 Engineering Decisions

Several design decisions were intentionally made to keep the project realistic while avoiding unnecessary complexity.

### Batch instead of real-time ingestion

Flight data is collected periodically rather than every few minutes.

For the current scale of four airports, real-time streaming infrastructure would add complexity without providing enough additional value.

### Lambda instead of Spark / Glue

AWS Lambda is sufficient for the expected data volume and allows transformations to remain lightweight and serverless.

AWS Glue or Spark would become more appropriate if the pipeline expanded significantly in data volume or transformation complexity.

### S3 as the raw source of truth

Raw API responses are retained in S3 so data can be replayed if transformation logic changes.

### Redshift for analytical workloads

Amazon Redshift provides the analytical warehouse used to implement the Medallion Architecture and dimensional models.

### Historical flight tracking

Because flight attributes can change throughout the operational day, historical versions are retained rather than simply overwriting existing values.

This enables analysis not only of the final state of a flight, but also of **how that flight changed over time**.

---

# 🛠️ Tech Stack

```text
Language
├── Python
└── SQL

AWS
├── Amazon S3
├── AWS Lambda
├── Amazon EventBridge
├── AWS Step Functions
├── Amazon Redshift
├── AWS IAM
└── Amazon CloudWatch

Data Engineering
├── ETL / ELT
├── Data Lake
├── Data Warehouse
├── Medallion Architecture
├── Dimensional Modeling
├── Star Schema
├── SCD Type 2
├── Batch Processing
└── Data Quality

Development
├── Git
└── GitHub
```

---

# 🎓 Project Purpose

This project was built as a portfolio data engineering project to demonstrate practical experience designing and implementing an end-to-end cloud data pipeline.

Rather than focusing only on individual scripts, the project emphasizes the complete data lifecycle:

```text
Source
  ↓
Ingestion
  ↓
Raw Storage
  ↓
Transformation
  ↓
Data Quality
  ↓
Data Modeling
  ↓
Data Warehousing
  ↓
Analytics
```

The goal is to demonstrate the ability to make appropriate engineering decisions based on **data volume, cost, complexity, scalability, and analytical requirements**.


