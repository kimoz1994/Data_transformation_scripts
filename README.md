# BOM Transformation ETL Pipeline

## Overview
This project is an end-to-end **ETL pipeline** developed to transform complex Bill of Materials (BOM) and supplier datasets into a standardized structure ready for integration with a SaaS platform. 

The pipeline processes manufacturing, supplier, and composition datasets exported from Snowflake views, enriches them with geographic and chemical composition information, reconstructs multi-level BOM hierarchies, and produces a clean, scalable output dataset suitable for downstream ingestion and analytics.

**The solution reduced a previously manual workflow from approximately 4–6 months of effort to 4–6 hours of automated processing.**

---

## Project Focus
The source data was received as multiple relational tables exposed through Snowflake views. Initial data exploration, validation, and screening were performed using SQL, while the transformation and enrichment pipeline was implemented in Python using Pandas.

* **BOM hierarchy reconstruction**
* **Recursive component traversal**
* **Quantity scaling across BOM levels**
* **Supplier and geographic enrichment**
* **Chemical composition enrichment**
* **Data normalization and cleansing**
* **Automated export generation**

---

## Key Features
* **Automated multi-level BOM expansion** using DFS traversal.
* **Recursive hierarchy processing** for deep component nesting.
* **Quantity propagation** and scaling across nested BOM levels.
* **Supplier and geographic enrichment** for supply chain visibility.
* **Chemical composition mapping** using CDMS reference data.
* **Unit conversion and normalization** (e.g., LB to KG).
* **High-volume processing** of over 1.3M+ BOM records.
* **Memory optimization** via chunked export processing.

---

## Data Sources
The pipeline consumes multiple datasets including:

| Dataset | Purpose |
| :--- | :--- |
| **Monthly BOM Data** | Core BOM relationships and parent-child links |
| **Plant Data** | Manufacturing site metadata |
| **Supplier Location Data** | Supplier geographic enrichment |
| **CDMS Composition Data** | Chemical composition and CAS identifier mapping |

---

## Tech Stack
### SQL
Used for data exploration, validation, filtering, data quality screening, and Snowflake view analysis.

### Python
Used for ETL orchestration, recursive hierarchy traversal, data transformation, enrichment, and export generation.
* **Main Libraries:** `pandas`, `numpy`, `tqdm`

---

## Pipeline Architecture

### 1. Data Loading & Preparation
The pipeline loads BOM and enrichment datasets, applies datatype normalization, filters invalid records, and restricts data to the target year.
* Removal of zero-output rows.
* Material ID standardization.
* Aggregation of quantities.

### 2. Adjacency List Construction
An adjacency list is built from BOM relationships to represent parent-child component dependencies. This enables efficient recursive traversal and hierarchical mapping.

### 3. Recursive BOM Traversal
A **Depth-First Search (DFS)** algorithm recursively traverses the BOM structure to expand nested components and track hierarchy levels, preserving component lineage and depth.

### 4. Quantity Scaling
The pipeline calculates scaled component quantities across hierarchy levels by normalizing input/output ratios and propagating quantities recursively. This ensures accurate calculations for deeply nested structures.

### 5. Supplier & Geographic Enrichment
Supplier metadata is added using plant reference data and supplier location datasets. Enrichment includes supplier identification and geographic coordinates (with address-based fallback).

### 6. Chemical Composition Enrichment
Leaf-level BOM components are matched against **CDMS reference data** to identify chemical compositions and extract CAS identifiers. Chemical rows are dynamically expanded into separate BOM entries.

### 7. Data Cleansing & Standardization
Final processing includes non-printable character removal, unit normalization, and field standardization to meet SaaS integration requirements.

### 8. Batch Processing & Export
The pipeline processes BOMs incrementally using chunked execution to optimize memory usage. Final outputs are exported to **CSV** and **Excel**.

---

## Performance Impact

| Metric | Before Automation | After Automation |
| :--- | :--- | :--- |
| **Processing Time** | 4–6 Months | 4–6 Hours |
| **Workflow** | Manual reconciliation | Fully automated ETL |
| **Operational Overhead** | High | Minimal |

