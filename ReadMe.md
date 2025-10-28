# DAMG Food Inspection Data Engineering Project

A comprehensive data engineering solution for analyzing food inspection data using Azure Data Factory, Python, Snowflake, and visualization tools. This project implements an end-to-end ETL pipeline with dimensional modeling to deliver insights on food safety and inspection trends.

## 📋 Project Overview

This project builds a scalable data pipeline that processes food inspection records through multiple stages - from data profiling to interactive dashboards. The solution leverages cloud-native Azure services, Python for data profiling, and Snowflake for data warehousing.

### Key Features

- **Automated ETL Pipeline**: Azure Data Factory orchestration
- **Advanced Data Profiling**: Python-based exploratory data analysis
- **Data Quality Management**: Alteryx-powered data cleansing
- **Cloud Data Warehouse**: Snowflake dimensional modeling
- **Star Schema Design**: Optimized for analytical queries
- **Interactive Dashboards**: Business intelligence visualizations

## 🏗️ Architecture

```
Data Sources (Food Inspection Records)
            ↓
Python (ydata-profiling - EDA & Profiling)
            ↓
    Alteryx (Data Cleaning)
            ↓
Azure Data Factory (ETL Orchestration)
            ↓
  Snowflake (Star Schema DW)
            ↓
   Dashboard (BI Visualization)
```

## 🎯 Project Stages

### 1. Data Profiling (Python - ydata-profiling)
**Purpose**: Comprehensive exploratory data analysis and data quality assessment

**Tools Used**:
- **ydata-profiling** (formerly pandas-profiling): Automated profiling library
- **Python**: Data analysis and statistics

**Key Activities**:
- Generate comprehensive HTML profiling reports
- Analyze data distributions and patterns
- Identify missing values and outliers
- Assess data quality metrics
- Detect correlations and relationships
- Evaluate data types and formats

**Sample Code**:
```python
from ydata_profiling import ProfileReport
import pandas as pd

# Load food inspection data
df = pd.read_csv('food_inspections.csv')

# Generate profiling report
profile = ProfileReport(df, title="Food Inspection Data Profile")
profile.to_file("food_inspection_profile.html")
```

**Profiling Outputs**:
- Overview statistics (row count, columns, missing data %)
- Variable distributions and histograms
- Correlation matrices
- Missing value patterns
- Duplicate detection
- Data quality warnings

### 2. Data Cleaning (Alteryx)
**Purpose**: Cleanse and standardize food inspection data

**Cleaning Operations**:
- **Duplicate Removal**: Remove redundant inspection records
- **Missing Value Handling**: Impute or filter null values
- **Date Standardization**: Normalize inspection dates
- **Text Cleaning**: Clean establishment names and addresses
- **Category Standardization**: Standardize violation types
- **Data Type Conversion**: Ensure correct data types
- **Validation Rules**: Apply business logic validation

**Data Quality Rules**:
- Valid inspection dates (not future dates)
- Required fields must be populated
- Violation codes must match lookup tables
- Establishment addresses properly formatted
- Inspection results categorized correctly

### 3. Data Loading (Azure Data Factory & Snowflake)
**Purpose**: Load cleaned data into Snowflake cloud data warehouse

**Components**:
- **Azure Data Factory**: Orchestration and data movement
- **Snowflake**: Cloud data warehouse
- **Linked Services**: Secure connections to data sources
- **Datasets**: Schema definitions for sources and sinks
- **Pipelines**: Automated data flow logic

**Loading Strategy**:
- Stage tables for initial data landing
- Transformation logic for business rules
- Incremental loading for new records
- Error handling and retry mechanisms
- Data validation checkpoints

### 4. Star Schema Design (ER Studio)
**Purpose**: Design optimized dimensional model for analytics

**Dimensional Model**:

**Fact Table**:
- **FactInspection**: Core inspection events and metrics
  - InspectionKey (PK)
  - EstablishmentKey (FK)
  - DateKey (FK)
  - InspectorKey (FK)
  - ViolationKey (FK)
  - InspectionResult
  - RiskLevel
  - ViolationCount
  - InspectionScore

**Dimension Tables**:
- **DimEstablishment**: Restaurant/facility details
  - EstablishmentKey (PK)
  - EstablishmentID
  - Name
  - Address
  - City, State, ZIP
  - FacilityType
  - LicenseNumber
  
- **DimDate**: Time dimension
  - DateKey (PK)
  - FullDate
  - Year, Quarter, Month, Day
  - DayOfWeek, WeekOfYear
  - FiscalYear, FiscalQuarter
  
- **DimInspector**: Inspector information
  - InspectorKey (PK)
  - InspectorID
  - InspectorName
  - Certification
  - Region
  
- **DimViolation**: Violation classifications
  - ViolationKey (PK)
  - ViolationCode
  - ViolationDescription
  - ViolationCategory
  - Severity
  - ComplianceType

- **DimLocation**: Geographic hierarchy
  - LocationKey (PK)
  - City
  - County
  - State
  - Region
  - ZIPCode

**Design Principles**:
- Surrogate keys for all dimensions
- Slowly Changing Dimensions (SCD Type 2) where needed
- Conformed dimensions across fact tables
- Degenerative dimensions where appropriate
- Fact table at atomic grain level

### 5. Dashboard Design
**Purpose**: Create interactive visualizations for food safety insights

**Key Metrics & KPIs**:
- Total inspections conducted
- Pass/fail rates by establishment type
- Average inspection scores
- Most common violations
- Trend analysis over time
- Geographic distribution of violations
- Inspector performance metrics

**Dashboard Components**:
- **Executive Summary**: High-level KPIs and trends
- **Inspection Analysis**: Detailed inspection patterns
- **Violation Tracker**: Violation types and frequencies
- **Establishment Performance**: Individual facility scores
- **Geographic View**: Map-based violation distribution
- **Temporal Analysis**: Time-series trends
- **Risk Assessment**: Risk level distributions

**Visualizations**:
- Bar charts for violation categories
- Line charts for trends over time
- Heat maps for geographic patterns
- Scorecards for key metrics
- Tables for detailed drill-down
- Filters for interactive exploration

## 📁 Project Structure

```
DAMGFoodInspection/
├── dataflow/                    # ADF data transformation flows
│   └── factdataflowy2
├── dataset/                     # Dataset definitions
│   └── SF_Fact_v2
├── factory/                     # Factory configuration
│   └── ADF settings and metadata
├── linkedService/               # Connection configurations
│   ├── AzureDataLakeStorage_LS
│   └── DataLakeStgFoodInspection
├── pipeline/                    # Orchestration pipelines
│   └── foodinspectionv2
├── publish_config.json          # Publishing configuration
└── ReadMe                       # Project documentation
```

## 🚀 Getting Started

### Prerequisites

- **Azure Subscription** with Data Factory access
- **Snowflake Account** (Standard edition or higher)
- **Python 3.8+** with ydata-profiling library
- **Alteryx Designer** (for data cleaning workflows)
- **ER Studio** or similar modeling tool
- **BI Tool** (Power BI, Tableau, or similar)
- **Azure Data Lake Storage Gen2**

### Installation

#### 1. Clone Repository
```bash
git clone https://github.com/yourusername/DAMGFoodInspection.git
cd DAMGFoodInspection
```

#### 2. Set Up Python Environment
```bash
# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install pandas
pip install ydata-profiling
pip install numpy
pip install matplotlib
pip install seaborn
```

#### 3. Configure Azure Resources

**Create Azure Data Factory**:
```bash
az datafactory factory create \
  --resource-group <resource-group> \
  --factory-name <factory-name> \
  --location <location>
```

**Create Azure Data Lake Storage**:
```bash
az storage account create \
  --name <storage-account> \
  --resource-group <resource-group> \
  --location <location> \
  --sku Standard_LRS \
  --kind StorageV2 \
  --hierarchical-namespace true
```

#### 4. Set Up Snowflake

```sql
-- Create database and schemas
CREATE DATABASE FOOD_INSPECTION_DW;
CREATE SCHEMA FOOD_INSPECTION_DW.STAGING;
CREATE SCHEMA FOOD_INSPECTION_DW.DIMENSIONS;
CREATE SCHEMA FOOD_INSPECTION_DW.FACTS;

-- Create warehouse
CREATE WAREHOUSE FOOD_INSPECTION_WH
  WITH WAREHOUSE_SIZE = 'MEDIUM'
  AUTO_SUSPEND = 300
  AUTO_RESUME = TRUE;

-- Create stage for data loading
CREATE STAGE FOOD_INSPECTION_STAGE
  URL = 'azure://youraccount.blob.core.windows.net/data/'
  CREDENTIALS = (AZURE_SAS_TOKEN='...');
```

#### 5. Deploy ADF Pipelines

1. Open Azure Data Factory Studio
2. Import ARM templates or JSON definitions
3. Configure linked services
4. Update pipeline parameters
5. Validate connections
6. Publish changes

## 🔄 Pipeline Execution

### Data Flow Process

1. **Extract**: Pull raw food inspection data from source systems
2. **Profile**: Generate data quality reports using ydata-profiling
3. **Clean**: Apply cleansing rules in Alteryx
4. **Stage**: Load to Snowflake staging area
5. **Transform**: Apply business logic and create dimensions
6. **Load**: Populate fact and dimension tables
7. **Validate**: Run data quality checks
8. **Visualize**: Refresh BI dashboards

### Running the Pipeline

**Manual Trigger**:
```bash
az datafactory pipeline create-run \
  --resource-group <resource-group> \
  --factory-name <factory-name> \
  --pipeline-name foodinspectionv2
```

**Scheduled Trigger**:
- Configure schedule trigger in ADF
- Set frequency (daily, weekly, monthly)
- Define execution time windows
- Configure retry policies

## 📊 Data Quality Monitoring

### Python Profiling Checks
```python
# Data quality validation
def validate_data_quality(df):
    quality_checks = {
        'total_records': len(df),
        'missing_percentage': df.isnull().sum() / len(df) * 100,
        'duplicate_count': df.duplicated().sum(),
        'date_range': (df['inspection_date'].min(), df['inspection_date'].max())
    }
    return quality_checks
```

### Snowflake Monitoring Queries
```sql
-- Check fact table loading
SELECT 
    COUNT(*) as total_inspections,
    COUNT(DISTINCT establishment_key) as unique_establishments,
    MIN(inspection_date) as earliest_date,
    MAX(inspection_date) as latest_date
FROM FACTS.FactInspection;

-- Validate dimension integrity
SELECT 
    d.dimension_name,
    COUNT(*) as record_count,
    COUNT(DISTINCT surrogate_key) as unique_keys
FROM information_schema.tables t
WHERE table_schema = 'DIMENSIONS'
GROUP BY dimension_name;
```

## 🛠️ Development Guidelines

### Data Profiling Best Practices
- Run profiling on sample data first for large datasets
- Schedule regular profiling to monitor data drift
- Document data quality issues discovered
- Share profiling reports with stakeholders

### ETL Best Practices
- Implement idempotent pipeline design
- Use parameterized queries
- Log all transformations
- Implement error handling
- Create unit tests for transformations

### Star Schema Best Practices
- Use surrogate keys for all dimensions
- Implement SCD Type 2 for historical tracking
- Keep fact tables at atomic grain
- Denormalize dimensions when appropriate
- Create aggregate fact tables for performance

## 📈 Performance Optimization

### ADF Optimization
- Optimize data flow partitioning
- Use appropriate integration runtime size
- Enable staging for large datasets
- Implement parallel execution where possible
- Monitor and tune DIU (Data Integration Units)


## 🔒 Security & Compliance

### Data Security
- Store credentials in Azure Key Vault
- Use Managed Identity for authentication
- Implement RBAC in Snowflake
- Encrypt data at rest and in transit
- Mask PII in non-production environments


### Integration Tests
- End-to-end pipeline validation
- Data reconciliation checks
- Performance benchmarks
- Dashboard functionality tests

## 📚 Documentation

### Additional Resources
- [Azure Data Factory Documentation](https://docs.microsoft.com/azure/data-factory/)
- [Snowflake Documentation](https://docs.snowflake.com/)
- [ydata-profiling Documentation](https://docs.profiling.ydata.ai/)
- [Dimensional Modeling (Kimball)](https://www.kimballgroup.com/)

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create feature branch (`git checkout -b feature/enhancement`)
3. Commit changes (`git commit -m 'Add feature'`)
4. Push to branch (`git push origin feature/enhancement`)
5. Open Pull Request

---

**Built with Azure Data Factory, Python, Snowflake** | **Food Safety Analytics Pipeline**
