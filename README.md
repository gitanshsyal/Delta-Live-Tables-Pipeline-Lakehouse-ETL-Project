# Delta-Live-Tables-Pipeline-Lakehouse-ETL-Project
This project demonstrates a complete ETL pipeline using **Lakeflow Declarative Pipelines / Delta Live Tables** on **Azure Databricks**.
## Data Flow Architecture

Landing (Azure ADLS Gen2)  
⬇  
**Bronze Layer** – Raw ingestion  
⬇  
**Silver Layer** – Data Quality + Validation + Standardization  
⬇  
**Gold Layer** – Business Analytics + Aggregations

---

## Datasets Used

| Table | Description |
|-------|-------------|
| customers | Base customer master data |
| customer_updates | Incremental customer changes for SCD2 |
| products | Product catalog |
| orders | Sales transactions |

---

## Silver Layer Cleaning Rules

### Customers & Customer Updates
- Remove duplicates → `APPLY CHANGES INTO`
- Enforce NOT NULL on key fields
- Email pattern validation
- Phone number → standardized 10-digit numeric format
- Case normalization → email lowercase, city title-case, state uppercase
- Timestamp formatting

### Orders
- Validate order_id, customer_id, product_id not null
- `order_amount > 0` enforced via **DLT constraints**
- Correct timestamp format
- Remove duplicates on order_id

### Products
- Enforce valid product_id, product_name, category
- Price > 0 validation

---

## SCD Type-2 CDC

Maintains full historical view of changing customer information  
with fields:
- `__START_AT`
- `__END_AT`
- `__CURRENT__`

---

## Gold Layer Outputs

| Table | Description |
|-------|-------------|
| `customer_360_gold` | Lifetime revenue + total orders per active customer |
| `category_sales_gold` | Daily revenue by category with demand badge |
| `top_customers_gold` | Top 10 spenders ranked using DENSE_RANK |

---

##  Technologies Used

- Databricks Delta Live Tables (Lakeflow)
- Azure Data Lake Storage Gen2
- Delta Lake
- SQL transformations with streaming ingestion
- SCD-2 history tracking


---

 Open for collaboration and feedback!

