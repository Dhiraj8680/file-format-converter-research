# Exploratory Analysis - File Format Converter

## Author

**Dhiraj Chauhan**  

## Overview

This directory contains exploratory Jupyter notebooks documenting the incremental development of a CSV to JSON conversion pipeline for a retail database.

## Notebooks

### 1. Get Column Names using Schemas File.ipynb
**Purpose:** Extract column metadata from JSON schema

**Key Learning:**
- Parse JSON schema files
- Sort data using lambda functions
- Extract column names dynamically

**Main Function:**
```python
def get_column_names(schemas, ds_name, sorting_key='column_position'):
    columns_details = schemas[ds_name]
    columns = sorted(columns_details, key=lambda col: col[sorting_key])
    return [col['column_name'] for col in columns]
```

---

### 2. Get File Names to be processed.ipynb
**Purpose:** Discover and validate source CSV files

**Key Learning:**
- Use `glob` for pattern-based file discovery
- Validate data files before processing

**Datasets Discovered:**
- categories: 58 records
- customers: 12,435 records
- departments: 6 records
- orders: 68,883 records
- order_items: 172,198 records
- products: 1,345 records

---

### 3. Read CSV Data into Pandas Dataframe with schema.ipynb
**Purpose:** Combine schema reading with CSV parsing

**Key Learning:**
- Extract dataset names from file paths using `pathlib`
- Apply schemas dynamically during CSV reading
- Validate data with DataFrame preview

**Key Technique:**
```python
file_path_list = Path(file).parts
ds_name = file_path_list[-2]  # Extract dataset name
columns = get_columns_names(schemas, ds_name)
df = pd.read_csv(file, names=columns)
```

---

### 4. Write Pandas Dataframe to JSON Files.ipynb
**Purpose:** Complete the conversion pipeline

**Key Learning:**
- Create directory structures programmatically
- Convert DataFrames to JSONL format
- Preserve file organization

**Key Operations:**
```python
# Create output directory
os.makedirs(f'{tgt_base_dir}/{ds_name}', exist_ok=True)

# Write JSONL format
df.to_json(json_file_path, orient='records', lines=True)
```

**Output Format (JSONL):**
```json
{"order_id":1,"order_date":"2013-07-25 00:00:00.0","order_customer_id":11599,"order_status":"CLOSED"}
{"order_id":2,"order_date":"2013-07-25 00:00:00.0","order_customer_id":256,"order_status":"PENDING_PAYMENT"}
```

---

## Development Progression

```
Schema Understanding → File Discovery → CSV Reading → JSON Writing → Complete Pipeline.
```

## Technologies Used

- **pandas** - Data manipulation
- **pathlib** - Path operations
- **glob** - File pattern matching
- **json** - Schema parsing
- **os** - Directory management

## Running the Notebooks

### Prerequisites
```bash
pip install pandas
```

### Execution Order
Run notebooks sequentially (1 → 2 → 3 → 4) to follow the learning path.

### Data Requirements
- Source data in `data/retail_db/` directory
- `schemas.json` file present

## Key Takeaways

✅ Schema-driven processing for flexibility  
✅ Path-based dataset detection  
✅ JSONL format for big data compatibility  
✅ Incremental development approach  
✅ Data validation at each step  

## What's Next?

## What's Next?

This exploratory work served as the foundation for building a production-ready implementation. The refined, modular version is maintained as a separate project with improved error handling and reusable functions.
