# Fintech SQL Data Migration into Azure Portal

## Overview
This project focuses on migrating financial data from an SQL database into the Azure cloud ecosystem. The migration is structured into multiple layers to ensure data integrity and scalability using Azure services.

## Tech Stack
- **Azure**  Synapse Analytics, ADLS, Logic Apps
- **Azure SQL Database**
- **PySpark**
- **SQL**
- **Delta Tables**

## Infrastructure Setup
### Resource Group
- `fintech-data-migration`

### Storage Account
- Created `fintechdatamigration` with a `fintech` container
- Three folders: `bronze`, `silver`, and `gold`
![Alt text](snaps/containers-folders.PNG)

### SQL Database & Server
- **Database**: `fintechsqldb`
- **Server**: `fintechsqlserver`
![Alt text](snaps/sql-tables.PNG)

### Synapse Analytics
- **Workspace**: `synapse-ws-fintech`
- **ADLS Account**: `fintechdatamigration`
- **File System**: `synapse-data`
- **Spark Pool**: `fintechsparkpool`

## Data Pipeline Implementation
![Alt text](snaps/complete-pipeline.PNG)

### Linked Services
1. **LS_SQLdatabase**
2. **LS-fintechdatalake** (Permission: Storage Blob Data Contributor)

### Datasets
1. **SQLSource**: Placeholder dataset for SQL tables
![Alt text](snaps/sql-source-dataset.PNG)

3. **BronzeLayer**: Parquet storage dataset
![Alt text](snaps/bronze-layer-dataset-1.PNG)

![Alt text](snaps/bronze-layer-dataset-2.PNG)

### Data Pipeline: `FintechPipeline`
1. **Lookup Activity** (`GetTableList`)
   - Retrieves a list of tables from `fintechsqldb`
![Alt text](snaps/lookup-activity.PNG)

2. **ForEach Activity**
   - Iterates over tables and copies data to the Bronze layer
![Alt text](snaps/for-each-activity.PNG)
![Alt text](snaps/for-each-activity-expression.PNG)

3. **Copy Activity** (`CopyToBronzeLayer`)
   - Extracts data from SQL tables and writes to ADLS in Parquet format
![Alt text](snaps/copy-activity-1.PNG)
![Alt text](snaps/copy-activity-source.PNG)
![Alt text](snaps/copy-activity-sink.PNG)

### Data Processing in Synapse (Bronze to Silver)
- **Notebook Activity** (`BronzeToSilverDataProcess`)
  - Uses PySpark for data transformations
  - Runs on `fintechsparkpool`
- **Notebook Activity** (`SilverToGoldDataProcess`)
  - Uses PySpark to create Business-Ready Data
  - Runs on `fintechsparkpool`

## Notification System
- **Web Activity** for sending execution notifications
  - **SuccessNotification**
  - **FailedNotification**
![Alt text](snaps/web-activity-1.PNG)
![Alt text](snaps/web-activity-success.PNG)
![Alt text](snaps/web-activity-failure.PNG)

- **Logic App Integration**
  - HTTP Trigger to send emails via Outlook
![Alt text](snaps/logic-app-2.PNG)
![Alt text](snaps/logic-app-1.PNG)


## Conclusion
This project provides an end-to-end Azure-based SQL data migration and transformation pipeline, ensuring scalability, security, and efficient data processing.
