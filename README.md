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
- Retrieve the latest processed data:
  ```sql
  SELECT TOP 1 latest_processed_pickup
  FROM metadata.processing_log
  WHERE table_processed = 'staging_nyctaxi_yellow'
  ORDER BY latest_processed_pickup DESC;
  ## 2. Pipeline: Pipeline_presentation_processing_nyctaxi

This pipeline focuses on transforming and loading the processed data from the staging layer into the presentation layer for analysis and visualization.

**Steps:**

**Create the Presentation Table:**

This SQL statement creates the main table (`dbo.nyctaxi_yellow`) in the presentation layer to store the cleaned and transformed taxi ride data:

```sql
CREATE TABLE dbo.nyctaxi_yellow
(
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
