## Canadian Airport Operations Data Engineering Pipeline
### 📌 Project Overview

Canadian airports process hundreds of departures and arrivals every day, while flight information continuously changes as flights are delayed, cancelled, rescheduled, or completed.

This project builds an end-to-end AWS data engineering project that collects, processes, models, and analyzes flight data and operations from four of Canada's busiest airports.

The project demonstrates the design and implementation of a cloud-based ETL/ELT pipeline using **Python, Amazon S3, AWS Lambda, Amazon EventBridge, AWS Step Functions, Amazon Redshift, and SQL**, with a focus on data warehousing, dimensional modeling, historical tracking, and analytics-ready datasets.

* **YYZ** — Toronto Pearson International Airport
* **YVR** — Vancouver International Airport
* **YUL** — Montréal–Trudeau International Airport
* **YYC** — Calgary International Airport

Flight information is extracted several times per day to capture changes in operational status.

The pipeline ingests raw flight data into an Amazon S3 data lake, transforms and validates it using Python and AWS Lambda, and loads analytics-ready data into Amazon Redshift using a **Medallion Architecture**.

Historical changes to flight information are preserved using **Slowly Changing Dimension Type 2 (SCD2)** concepts where appropriate.

### 👤 Why I Built This Project

I'm **Hilary Fotso**, currently working as **DataOps Engineer** and building toward a career in Data Engineering.

I started this project to move beyond learning data engineering tools individually and apply them to a realistic, end-to-end problem. Flight operations were particularly interesting to me because the data is continuously changing, comes from multiple sources, and creates real challenges around ingestion, historical tracking, data modeling, and analytics.

My goal is not simply to use as many technologies as possible, but to understand **why and when they should be used**. As the project evolves, I document the architectural decisions, trade-offs, and improvements I make along the way.

###  🚧 Project Status

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

## Wiki table of contents
- [Home](https://github.com/hilary-ostof/canadian-flight-warehouse/wiki)
- [Architecture & Data Flow](https://github.com/hilary-ostof/canadian-flight-warehouse/wiki/Architecture)
- [Data Sources](https://github.com/hilary-ostof/canadian-flight-warehouse/wiki/Data-Sources)
- [Tech Stack](https://github.com/hilary-ostof/canadian-flight-warehouse/wiki/Tech-Stack)
- [Miscellaneous](https://github.com/hilary-ostof/canadian-flight-warehouse/wiki/Miscellaneous)
- [Strategy](https://github.com/hilary-ostof/canadian-flight-warehouse/wiki/Strategy)




