# 📊 Daily Transaction Analytics: Streamlined ETL with AWS S3 & Microsoft Fabric

This project delivers a robust, end-to-end data pipeline designed for businesses that handle large volumes of daily customer transaction summaries in JSON format. Leveraging the combined power of **AWS S3** for scalable raw data storage, **Microsoft Fabric** as a unified analytics platform, **PySpark** for efficient data processing, and **Power BI** for dynamic business intelligence, this solution addresses critical data management challenges.

---

## ❗ The Challenge: Data Duplication & Inefficient Full Refreshes

Traditional data ingestion methods often lead to redundant data storage and inefficient batch processing. Relying on full data truncates and refreshes for daily transactions consumes excessive computational resources, lengthens processing times, and increases the risk of data inconsistencies or loss during pipeline failures. This bottleneck severely impacts the ability to derive timely and accurate business insights.

---

## ✅ Our Solution: Incremental Loading & Optimized Transformation

This project implements a sophisticated incremental data loading mechanism. By meticulously tracking previously processed files, the pipeline intelligently identifies and ingests only new or updated transaction data from AWS S3 into Microsoft Fabric.

### Key Components:

* **Data Ingestion from AWS S3**: Files are uploaded daily, partitioned by folders.
* **Shortcut Connection in Fabric**: Using AWS Access Key to link S3 into Fabric's Lakehouse.
* **Incremental Load Tracking**: Ensures only new files are processed.
* **Data Transformation with PySpark**: Parses and flattens JSONs into a star schema:

  * `SalesFact`
  * `CustomerDim`, `ProductDim`, `PaymentDim`, `AccountDim`, `LocationDim`, `TimeDim`
* **Geo Enrichment**: Extracts country and state from `LaptopIP_Address`.
* **Export to Silver Layer**: Tables saved as Parquet and Delta.
* **Power BI Integration**: Clean semantic model built for reporting.

---

## 🧠 Key Benefits

| Benefit                     | Description                                                              |
| --------------------------- | ------------------------------------------------------------------------ |
| ⚡ Optimized Performance     | Incremental loading cuts processing time and compute cost                |
| 🛡️ Enhanced Data Integrity | Minimizes data duplication and avoids full-refresh failures              |
| ☁️ Scalable Architecture    | Cloud-native stack scales with growing data volumes                      |
| 📊 Actionable Insights      | Structured, clean data fuels rich Power BI dashboards                    |
| 💰 Resource Efficiency      | Avoids redundant transformations and storage by tracking processed files |

---

## 🚀 Project Workflow Summary

### 1. **Data Generation and Ingestion**

* Daily transaction data generated via Python script
* Uploaded to AWS S3 bucket: `microsoft-fabrics-0619`
* Group `Microsoft_Fabrics_Engineer` created with full access

### 2. **Connect Microsoft Fabric to S3**

* Fabric Lakehouse shortcut configured to S3 bucket
* `awss3_raw` (bronze) Lakehouse reflects the shortcut

### 3. **Data Transformation in Notebooks**

* Daily JSONs incrementally read
* Records merged and enriched
* Converted to star schema using PySpark

### 4. **Store Output in Silver Layer**

* Transformed tables written to `awss3_transformed`
* Formats: Parquet and Delta

### 5. **Semantic Model and Table Load**

* Tables loaded and modeled in Fabric
* Date table created and marked
* Formatting and relationships configured

### 6. **Power BI Reporting**

* KPIs, charts, and slicers built
* Visual logic includes product parsing, location breakdowns, etc.

---

## 🔄 Fabric Pipeline Automation

* Notebook execution, semantic model refresh, and dashboard updates handled via Fabric pipeline
* Latest files dynamically loaded using Spark APIs (no hardcoded paths)
* Final dashboard auto-refreshes daily

---

## 📈 Dashboard Measures (DAX)

```DAX
Total Revenue = SUM(SalesFact[OrderTotalPrice])
Total Orders = COUNT(SalesFact[OrderId])
Average Order Value (AOV) = DIVIDE([Total Revenue], [Total Orders])
Unique Customers = DISTINCTCOUNT(SalesFact[CustomerId])
Payment Success Rate = DIVIDE(CALCULATE(COUNT(PaymentDim[PaymentId]), PaymentDim[PaymentStatus] = "Success"), COUNT(PaymentDim[PaymentId]))
```

---

## 📊 Recommended Visuals

* Revenue Trend (Line Chart)
* AOV by Segment (Column)
* Customer Distribution (Donut)
* Payment Source Split (Stacked Column)
* Revenue by Country (Map)
* Order Status Trend (Area Chart)

---

## 🎯 Business Impact

* 📉 Reduced infrastructure cost via compute efficiency
* ⚡ Faster data refresh → real-time insights
* 👥 Deeper customer intelligence
* 🧩 Increased focus on analysis, not data wrangling

---

## 📁 Key Artifacts

* Raw + Transformed files in AWS S3
* PySpark Notebooks
* Delta/Parquet tables
* Semantic model in Fabric
* Power BI dashboard (connected live)
* Fabric Data Pipeline

---

MIT License © 2025
