# canadian-flight-warehouse
Personal Portfolio Data Engineering project aiming to analyze flight operations trends in Canada's four majour airports

Objectif du projet

Construire une plateforme Data Engineering qui collecte, transforme, historise et analyse les données de vols pour les grands aéroports canadiens afin d’identifier les tendances opérationnelles comme :

volume de départs et d’arrivées;
retards moyens par aéroport;
retards par compagnie aérienne;
taux d’annulation;
périodes de forte activité;
comparaison entre les aéroports;
évolution quotidienne, hebdomadaire ou mensuelle des opérations.

Repository Organisation
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

Questions analytiques à répondre :
Quel aéroport a le plus grand volume de vols par jour?
Quel aéroport a le plus fort taux de retard?
Quelles compagnies aériennes ont les retards moyens les plus élevés?
Les retards sont-ils plus fréquents le matin, l’après-midi ou le soir?
Quels jours de la semaine ont le plus d’opérations?
Quel est le taux d’annulation par aéroport?
Quelle est la tendance des retards sur les 30 ou 90 derniers jours?
Les arrivées sont-elles plus souvent retardées que les départs?
Quels corridors sont les plus actifs, par exemple YYZ-YUL ou YVR-YYC?
Peut-on détecter des pics opérationnels inhabituels


High-Level Architecture
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
