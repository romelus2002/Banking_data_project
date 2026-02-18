🚀 Azure-Native Real-Time Banking Data Platform
📌 Project Overview

This project implements a fully Azure-native modern data platform that simulates a real-world banking system (customers, accounts, transactions) and processes both real-time and batch data using scalable Microsoft Azure services.

The platform demonstrates:

Change Data Capture (CDC) from Azure SQL

Real-time and incremental ingestion

Bronze → Silver → Gold Lakehouse architecture

SCD Type-2 historical tracking

Enterprise orchestration and monitoring

CI/CD automation

Secure, governed analytics delivery

🏗️ Azure Architecture
Data Generator (Python + Faker)
        ↓
Azure SQL Database (OLTP, CDC Enabled)
        ↓
Azure Data Factory (Incremental / CDC Extraction)
        ↓
Azure Data Lake Storage Gen2 (Bronze - Delta)
        ↓
Azure Databricks (Spark + Delta Lake)
        ↓
Bronze → Silver → Gold Transformations
        ↓
Azure Synapse Analytics / Databricks SQL
        ↓
Power BI Dashboards
        ↓
Azure DevOps (CI/CD)

⚡ Azure Tech Stack
Layer	Azure Service
OLTP Source	Azure SQL Database
CDC / Ingestion	Azure Data Factory
Storage	Azure Data Lake Storage Gen2
Processing	Azure Databricks
Data Format	Delta Lake
Orchestration	Azure Data Factory
Data Warehouse	Azure Synapse Analytics
BI	Power BI
CI/CD	Azure DevOps
Security	Microsoft Entra ID + Managed Identity
Governance	Microsoft Purview
🔄 End-to-End Pipeline Flow
1️⃣ Data Simulation

Python + Faker generates:

Customers

Accounts

Transactions

Data inserted into Azure SQL Database.

Azure SQL acts as the transactional OLTP system with ACID guarantees.

2️⃣ Change Data Capture (CDC)

CDC is enabled on Azure SQL:

EXEC sys.sp_cdc_enable_db;

EXEC sys.sp_cdc_enable_table
    @source_schema = 'dbo',
    @source_name   = 'transactions',
    @role_name     = NULL;


Azure Data Factory reads from CDC tables and extracts incremental changes.

3️⃣ Bronze Layer (Raw Data)

ADF loads raw incremental data into:

Azure Data Lake Storage Gen2 (Delta format)


Bronze layer characteristics:

Immutable raw records

Metadata columns (load timestamp, batch ID)

Partitioned by ingestion date

4️⃣ Silver Layer (Cleaned & Validated)

Azure Databricks performs:

Schema validation

Deduplication

Null handling

Data quality checks

Standardization

Stored as optimized Delta tables.

5️⃣ Gold Layer (Business Models)

Business-ready tables include:

dim_customers (SCD Type-2)

dim_accounts

fact_transactions

SCD Type-2 implemented using Delta MERGE:

MERGE INTO dim_customer AS target
USING updates AS source
ON target.customer_id = source.customer_id
WHEN MATCHED AND target.hash <> source.hash THEN
  UPDATE SET current_flag = false
WHEN NOT MATCHED THEN
  INSERT (...)


Gold layer supports analytics, reporting, and regulatory audit requirements.

📊 Analytics Layer

Gold tables exposed via:

Azure Synapse Dedicated SQL Pool
OR

Databricks SQL Warehouse

Connected to Power BI for:

Customer lifetime value analysis

Account balance trends

Transaction volume monitoring

Historical account tracking

Fraud detection indicators

🔐 Security & Governance

Microsoft Entra ID authentication

Managed Identity for service-to-service communication

Azure Key Vault for secret management

Row-Level Security (RLS)

Dynamic Data Masking

Transparent Data Encryption (TDE)

Microsoft Purview for lineage and catalog

Azure SQL auditing enabled

🔁 CI/CD with Azure DevOps
Continuous Integration (CI)

Lint notebooks

Validate Spark transformations

Run unit tests

Validate SQL scripts

Continuous Deployment (CD)

Deploy Databricks notebooks

Deploy ADF pipelines

Deploy Synapse objects

Promote Dev → Test → Prod

📂 Repository Structure
azure-banking-data-platform/
├── data-generator/
│   └── faker_generator.py
├── databricks/
│   ├── bronze_ingestion.py
│   ├── silver_transformations.py
│   ├── gold_models.py
│   └── scd2_logic.py
├── adf/
│   └── azure_sql_cdc_pipeline.json
├── synapse/
│   └── warehouse_models.sql
├── devops/
│   ├── ci-pipeline.yml
│   └── cd-pipeline.yml
├── infrastructure/
│   └── bicep_or_terraform_templates/
└── README.md

🏆 Key Capabilities Demonstrated

Azure SQL CDC implementation

Incremental data processing

Enterprise Lakehouse design

Delta Lake performance optimization

Historical data tracking (SCD Type-2)

Secure cloud-native architecture

Automated DevOps pipeline

Governed and auditable analytics platform

🎯 Business Value

This platform demonstrates the ability to:

Architect secure Azure-native data solutions

Handle high-volume transactional systems

Implement reliable CDC pipelines

Build scalable Lakehouse architectures

Deliver analytics-ready datasets

Apply enterprise-grade security and governance controls
