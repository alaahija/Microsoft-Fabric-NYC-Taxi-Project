# 🚖 Microsoft Fabric - NYC Taxi Data Project

## 📖 Overview
The **NYC Taxi Data Project** is a comprehensive data analytics solution built using **Microsoft Fabric**.

It captures and processes taxi ride data across New York City to deliver interactive visualizations and valuable insights into:
- Passenger trends
- Trip characteristics
- Operational efficiency

---

## 🏛️ Project Architecture

The following diagram outlines the end-to-end data flow—from ingestion to visualization:

![Architecture Diagram](https://github.com/user-attachments/assets/052aadce-0da0-4541-a659-88f3c6186650)

### 🔹 Fabric Architecture - Key Components
- **Fabric Data Pipeline**: Manages ingestion, transformation, and loading into the Lakehouse.
- **Fabric Lakehouse**: Centralized storage for raw and curated data.
- **Power BI**: Leverages *Direct Lake* connectivity for near real-time reporting and dynamic dashboards.

---

## 📋 General Project Description

This solution provides an interactive dashboard for analyzing NYC taxi rides over a selected date range.

Key capabilities:
- 📈 Monitor ride trends and passenger behavior
- 🚦 Detect operational inefficiencies
- 🧠 Enable informed, data-driven decision-making

---

## 🔄 ETL Process (Data Pipelines)

### 1. **Pipeline: `Pipeline_stg_processing_nyctaxi`**

![Pipeline 1 Diagram](https://github.com/user-attachments/assets/ea3079da-9972-4337-8273-f39a7e730e25)

**Steps:**

#### a. Retrieve the Latest Processed Data
```sql
SELECT TOP 1 latest_processed_pickup
FROM metadata.processing_log
WHERE table_processed = 'staging_nyctaxi_yellow'
ORDER BY latest_processed_pickup DESC;
#### b. Date Handling (Setting Date Variables)
```sql
v_date = @formatDateTime(
    addToTime(
        activity('Latest Processed Date').output.resultSets[0].rows[0].latest_processed_pickup, 
        1, 
        'Month'
    ), 
    'yyyy-MM'
);

v_end_date = @addToTime(
    concat(variables('v_date'), '-01'), 
    1, 
    'Month'
);
#### c. Copy to Staging (Clear Existing Records)
```sql
DELETE FROM stg.nyc_taxi_yellow;
#### d. d. Remove Outlier Dates (Data Cleaning Procedure)
```sql
CREATE PROCEDURE stg.data_cleaning_stg
    @end_date DATETIME2,
    @start_date DATETIME2
AS
BEGIN
    DELETE FROM stg.nyc_taxi_yellow 
    WHERE tpep_pickup_datetime < @start_date 
       OR tpep_pickup_datetime > @end_date;
END
### 2. Pipeline: Pipeline_presentation_processing_nyctaxi
#### a. Create the Presentation Table
```sql
CREATE TABLE dbo.nyctaxi_yellow (
    vendor VARCHAR(50),
    tpep_pickup_datetime DATE,
    tpep_dropoff_datetime DATE,
    pu_borough VARCHAR(100),
    pu_zone VARCHAR(100),
    do_borough VARCHAR(100),
    do_zone VARCHAR(100),
    payment_method VARCHAR(50),
    passenger_count INT,
    trip_distance FLOAT,
    total_amount FLOAT
);
#### b. Insert Presentation Metadata
```sql
CREATE PROCEDURE metadata.insert_presentation_metadata
    @pipeline_run_id VARCHAR(255),
    @table_name VARCHAR(255),
    @processed_date DATETIME2
AS
INSERT INTO metadata.processing_log
SELECT 
    @pipeline_run_id,
    @table_name,
    COUNT(*),
    MAX(tpep_pickup_datetime),
    @processed_date
FROM dbo.nyctaxi_yellow;
## 🗂️ Metadata Management

### Schema and Table Creation

```sql
CREATE SCHEMA metadata;

CREATE TABLE metadata.processing_log (
  pipeline_run_id VARCHAR(255), 
  table_processed VARCHAR(255), 
  rows_processed INT, 
  latest_processed_pickup DATETIME2(6),
  processed_datetime DATETIME2(6)
);



