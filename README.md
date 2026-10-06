# Medical Insurance Data Engineering Pipeline

A data engineering and SQL analytics project that builds a complete local ETL pipeline for synthetic medical insurance data.

The project generates large-scale healthcare data using Python and Faker, stores the generated datasets in Parquet format, transforms and loads the data through a Python-based ETL pipeline using SQLAlchemy, and organizes it into a normalized MySQL relational database for downstream analysis.

## Project Overview

The objective of this project is to simulate a production-style healthcare data pipeline covering the complete flow:

```text
Synthetic Data Generation
        ↓
      Parquet
        ↓
   Python ETL Pipeline
        ↓
    Transformation
        ↓
     SQLAlchemy
        ↓
   MySQL Database
        ↓
   SQL Analytics
```

The project focuses on:

* Large-scale synthetic healthcare data generation
* Columnar data storage using Parquet
* Python-based ETL
* SQLAlchemy database connectivity
* Relational database design
* Primary and foreign key constraints
* Referential integrity
* Multi-table SQL analysis

## Tech Stack

| Technology | Purpose                              |
| ---------- | ------------------------------------ |
| Python     | Data generation and ETL              |
| Faker      | Synthetic healthcare data generation |
| Pandas     | Data processing                      |
| PyArrow    | Parquet file handling                |
| SQLAlchemy | Database connection and data loading |
| MySQL      | Relational database                  |
| SQL        | Data analysis                        |
| Parquet    | Intermediate data storage            |

## Key Project Metrics

* **750,000+ rows** processed
* **6 normalized MySQL tables**
* **8+ synthetic healthcare attributes**
* **5+ table joins** used for analysis
* **Parquet-based storage**
* **Primary and foreign key constraints**
* **Python + SQLAlchemy ETL pipeline**

## Data Generation

The healthcare dataset is synthetically generated using **Faker**, allowing the project to work with realistic-looking healthcare records without exposing real patient information.

The generated data covers multiple patient and insurance-related attributes and is designed to provide enough relational complexity for downstream SQL analysis.

### Why Synthetic Data?

Healthcare data is highly sensitive and difficult to use for portfolio projects because of privacy restrictions.

Synthetic data provides a practical alternative:

* No real patient information is required
* Large datasets can be generated locally
* Data distributions can be controlled
* Relational relationships can be deliberately designed
* The resulting dataset can be safely used for development and analysis

## Parquet vs CSV

The generated datasets are stored in **Parquet** rather than relying exclusively on CSV.

Parquet is a columnar storage format that is particularly useful for analytical workloads.

In this project, storing the generated data in Parquet resulted in approximately **60–70% lower storage requirements compared with CSV**.

```text
CSV
 └── Row-oriented storage

Parquet
 └── Column-oriented storage
       ├── Compression
       ├── Efficient analytical reads
       └── Reduced storage footprint
```

## ETL Pipeline

The project uses a modular Python-based ETL pipeline.

### 1. Extract

The pipeline reads the generated Parquet datasets from the local data directory.

```text
Parquet Files
     ↓
Python
     ↓
DataFrames
```

### 2. Transform

The extracted data is prepared for relational storage.

Transformation includes preparing the data according to the target database schema and maintaining the relationships required between the six tables.

### 3. Load

SQLAlchemy is used to connect the Python pipeline to MySQL and load the processed datasets into the relational database.

```text
Python Data
     ↓
SQLAlchemy
     ↓
MySQL
```

This separates the data-generation layer from the database-loading layer and makes the pipeline easier to maintain.

## Database Design

The final dataset is organized into **six normalized MySQL tables**.

The schema uses:

* Primary keys
* Foreign keys
* Referential integrity
* Normalized relational structures

This design allows healthcare and insurance entities to be stored independently while maintaining relationships between them.

### Relational Structure

```text
                ┌──────────────────┐
                │   Patient Data   │
                └────────┬─────────┘
                         │
                         ▼
                ┌──────────────────┐
                │ Insurance Data   │
                └────────┬─────────┘
                         │
              ┌──────────┴──────────┐
              ▼                     ▼
       ┌──────────────┐      ┌──────────────┐
       │ Claims Data  │      │ Other Entity │
       └──────────────┘      └──────────────┘
```

The exact table relationships and schema definitions are available in `setup.sql`.

## Referential Integrity

Foreign key constraints are used to ensure that relationships between tables remain valid.

For example:

```text
Parent Record
     │
     │ Primary Key
     ▼
Child Record
     │
     │ Foreign Key
     ▼
Referenced Entity
```

This prevents orphaned records and simulates the data-integrity requirements of a production relational database.

## SQL Analysis

After loading the data into MySQL, SQL queries are used to analyze the healthcare and insurance data.

The repository contains:

```text
Analysis.sql
```

The analysis includes queries requiring joins across multiple relational tables, with some analyses involving **five or more table joins**.

This demonstrates the ability to work with normalized schemas rather than relying on a single denormalized dataset.

## Repository Structure

```text
Medical_Insurance/
│
├── Data/
│   └── Parquet datasets
│
├── ETL/
│   └── Python ETL pipeline
│
├── Analysis.sql
│
├── setup.sql
│
├── ETL.zip
│
├── LICENSE
│
└── README.md
```

## How to Run

### Prerequisites

Install:

* Python 3.x
* MySQL
* Required Python packages

### Install Dependencies

```bash
pip install pandas faker pyarrow sqlalchemy mysql-connector-python
```

### 1. Create the Database

Run:

```text
setup.sql
```

This creates the required MySQL database structure and constraints.

### 2. Configure the ETL Pipeline

Update the database connection configuration in the ETL scripts with your local MySQL credentials.

Example:

```python
from sqlalchemy import create_engine

engine = create_engine(
    "mysql+mysqlconnector://username:password@localhost/database_name"
)
```

### 3. Run the ETL Pipeline

Execute the Python ETL scripts inside the `ETL` directory.

The pipeline reads the Parquet data and loads it into MySQL through SQLAlchemy.

### 4. Run the Analysis

After the data has been loaded, execute:

```text
Analysis.sql
```

to perform the analytical queries.

## Engineering Concepts Demonstrated

### Data Generation

Large-scale synthetic datasets generated programmatically rather than manually downloaded from a static Kaggle dataset.

### Columnar Storage

Parquet is used as an intermediate storage format to reduce storage requirements and support analytical workloads.

### ETL

The project implements the complete:

```text
Extract → Transform → Load
```

workflow.

### Database Connectivity

SQLAlchemy provides the interface between the Python pipeline and MySQL.

### Relational Modeling

The dataset is split across six normalized tables rather than stored as one large flat file.

### Data Integrity

Primary and foreign key constraints enforce valid relationships between entities.

### SQL Analytics

Complex analytical queries use multiple joins across the relational schema.

## Why This Project Matters

This project goes beyond simply performing analysis on a pre-existing dataset.

It demonstrates the complete process of building the data layer required **before** analytics can happen:

```text
Generate Data
      ↓
Choose Storage Format
      ↓
Build ETL Pipeline
      ↓
Design Relational Schema
      ↓
Load Database
      ↓
Enforce Data Integrity
      ↓
Perform SQL Analysis
```

This makes the project relevant to **Data Analyst, Analytics Engineer, and Junior Data Engineer** roles where understanding how data moves from raw sources into an analytical database is important.

## Future Improvements

Potential extensions include:

* Incremental ETL instead of full data loads
* Pipeline logging and error handling
* Data quality validation before loading
* Configuration through environment variables
* Automated ETL scheduling
* Dockerized MySQL and ETL environment
* Query performance benchmarking
* Index optimization
* Data warehouse/star-schema implementation
* Dashboard integration using Power BI
* Automated pipeline testing

## License

This project is licensed under the **Apache License 2.0**.
