# Austin Animal Center – Data Warehousing and BI Solution

This project implements a full-stack Data Warehousing and Business Intelligence (BI) solution for the **Austin Animal Center** dataset. The process includes source data preparation, ETL development, data warehouse design using a Snowflake schema, and the creation of accumulating fact tables for process duration analysis.

---

## 📁 Dataset Overview

The dataset originates from the Austin Animal Center and covers animal intake, sheltering, and outcomes from **2013 to 2021**. It includes data about:

- Animal details
- Intake and outcome events
- Breed and color information
- Sex types
- Stray locations

### Source Files
- **Animal.csv**
- **Intakes.csv**
- **Outcomes.csv**
- **Stray_Map.csv**
- **Colors.csv** (CSV)
- **Sex_Type.txt** (TXT)

---

## ⚙️ Architecture

The architecture follows a typical DW/BI pipeline with the following layers:

1. **Data Sources**  
   - Source DB: `Austin_Animal_CenterSourceDB`  
   - Flat Files: CSV (`Colors.csv`), TXT (`Sex_Type.txt`)

2. **Staging Area**  
   - Database: `Austin_Animal_Center_Staging`

3. **Data Warehouse**  
   - Schema: `Austin_Animal_Center_DW`
   - Snowflake schema with normalized dimensions

4. **ETL Layer**  
   - Tool: SSIS (SQL Server Integration Services)

5. **BI Layer**  
   - Designed for downstream reporting and analytics (e.g., adoption trends, breed analysis)

---

## 🧱 Data Warehouse Design

### Schema: Snowflake Schema

- **Fact Table**
  - `FactAnimalEvent`: Stores transactional data such as stay duration and outcome info.

- **Dimension Tables**
  - `DimAnimal`
  - `DimColor`
  - `DimBreed`
  - `DimSexType`
  - `DimStrayMap` (Slowly Changing Dimension)
  - `DimDate`

### Slowly Changing Dimension
- **DimStrayMap** manages changing and historical attributes:
  - *Changing:* `Age`, `At_AAC`, `Image_Link`
  - *Historical:* `Intake_Date`

---

## 🔄 ETL Process (SSIS)

### 1. **Extraction**
- Source → Staging DB
- Handled multiple file formats (DB, CSV, TXT)

### 2. **Transformation**
- Null handling
- Data profiling and quality checks
- Lookup transformations for surrogate key generation
- Column filtering and data cleaning
- Derived attributes (e.g., Stay Duration, Insert/Modified Dates)

### 3. **Loading**
- Data loaded in dimension tables in dependency order
- Fact table populated last with all FK references resolved

---

## ⏱ Accumulating Fact Table Logic

- A separate SQL table is created to store **completion time**
- Fact table (`FactAnimalEvent`) is updated with **calculated process times**
- Used for duration-based performance analysis

---

## 🚀 Execution Flow

1. Extract → Load to Staging
2. Transform → Load to DW Dimensions
3. Load → Fact Table
4. Update → Process Time in Fact Table

---

## 🧪 BI Use Cases

- Analyze adoption trends over time
- Identify common intake types per breed
- Determine average stay duration by breed or sex type
- Visualize stray map locations and re-intake frequency

---

## 📌 Tools Used

- **SQL Server** (Database, SSIS, SSMS)
- **SSIS** (ETL Package Design)
- **MS Excel** (Data Profiling)
- **ERD Tools** (Schema Design)
