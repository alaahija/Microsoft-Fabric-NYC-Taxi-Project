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
