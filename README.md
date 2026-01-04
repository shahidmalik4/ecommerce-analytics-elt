# E-commerce Analytics ELT (Learning Project)

This repository is a learning and demonstration project that shows how an ELT / ETL-style analytics pipeline can be built using plain Python, without relying on heavy data engineering frameworks.

It is not production-ready and not intended for real-world deployment.

Instead, the goal is to:
- Help data analysts understand how ELT/ETL pipelines work
- Demonstrate data cleaning, validation, and testing concepts
- Show how analytics-ready tables can be built using Python, Pandas, and SQL

---

## What This Project Demonstrates

✔ Extracting data from a relational database
✔ Working with messy, imperfect source data
✔ Cleaning, deduplicating, and transforming data
✔ Schema validation using Pydantic
✔ Simple feature engineering for analytics
✔ Loading clean data into an analytics table
✔ Writing tests for data pipelines using pytest
✔ Using Docker for a reproducible local database

All logic is written in plain Python, intentionally avoiding complex frameworks.

---

## Project Structure

```bash
.
├── generate_source_data.py   # Generates fake, messy source data
├── extractor.py              # Extracts raw data from PostgreSQL
├── cleaner.py                # Cleans and transforms data
├── schemas.py                # Data validation schemas (Pydantic)
├── loader.py                 # Loads cleaned data back into the database
├── tests/
│   ├── test_cleaner.py       # Tests transformation logic
│   └── test_pipeline.py      # End-to-end pipeline tests
├── docker-compose.yml        # Local PostgreSQL setup
└── README.md
```

### **Environment and Source Data Generation**

| File(s)               | Function |
|----------------------|----------|
| `docker-compose.yml` | Defines the PostgreSQL service container for a repeatable local database environment.
| `generate_source_data.py` | Populates `raw_customers` and `raw_orders` with 1,000 intentionally messy records.

---

### **Extraction and Observability**

| File(s)       | Function |
|---------------|----------|
| `extractor.py` | Performs the Extract (E) step by connecting to PostgreSQL and pulling raw data into Pandas DataFrames.
| `pipeline.log` | Logs events to both terminal and file for observability.

---

### **Transformation and Pydantic Validation (Complete)**

This phase implemented the core data cleaning and quality enforcement layer, addressing all known data issues.

| File(s)       | Function |
|---------------|----------|
| `schemas.py`  | Defines the output data structure using a Pydantic `CleanedOrder` model with custom validators.
| `cleaner.py`  | Merges data, deduplicates records, imputes missing regions, fixes negative prices, derives `total_sale`, and validates each record via Pydantic.

---

### **Loading and Automated Testing**

This phase completes the ELT loop by loading the clean data into the final reporting table and running an automated test suite to guarantee data integrity.

| File(s)        | Function |
|----------------|----------|
| `loader.py`    | Orchestrates the full E-T-L pipeline and loads validated data into the `analytics_sales` table.
| `test_pipeline.py` | Runs Pytest checks: no duplicates, no negative values, valid foreign keys, correct row counts.
| `test_cleaner.py` | Unit Test: Uses Pytest with mock Pandas DataFrames to perform fast, isolated tests on the core logic in cleaner.py, ensuring functions work reliably without database access.

---

## Getting Started (Local Setup)

To run this project locally:

---

### **1. Clone the Repository**
```bash
git clone https://github.com/shahidmalik4/ecommerce-analytics-elt.git
cd ecommerce-analytics-elt
```

### **2. Ensure Docker is Running**

### **3. Start the Database**
```bash
docker compose up -d
```

### **4. Generate Source Data**
```bash
python generate_source_data.py
```

### **5. Run the Full Pipeline (E → T → L)**
```bash
python loader.py
```

### **6. Run Automated Data Validation (QA)**
```bash
pytest
```





