# 💾 Data Architecture

> **Navigation**: [⬅️ Back to Lesson Overview](../README.md) | [Previous: API Design](../04-api-design/README.md)

---

## 📊 5.1 Data Modeling

**🔄 Approaches**:
- **🎯 Conceptual Model**: High-level business entities
- **📋 Logical Model**: Detailed structure without implementation
- **💾 Physical Model**: Database-specific implementation

## 🔄 5.2 ETL vs ELT

| Aspect | 🔧 ETL | ⚡ ELT |
|--------|-----|-----|
| **Process** | Extract → Transform → Load | Extract → Load → Transform |
| **Transformation** | Outside data warehouse | Inside data warehouse |
| **Best For** | Structured data, smaller volumes | Large volumes, cloud DW |
| **Tools** | Informatica, Talend | dbt, Spark |

## 🏢 5.3 Data Warehouse vs Data Lake

| Aspect | 🏢 Data Warehouse | 🌊 Data Lake |
|--------|----------------|-----------|
| **Data Type** | Structured | Raw (any format) |
| **Schema** | Schema-on-write | Schema-on-read |
| **Processing** | ETL | ELT |
| **Users** | 📊 Business analysts | 🔬 Data scientists |
| **Cost** | 💰 Higher storage cost | 💵 Lower storage cost |

---

## 📊 Diagrams in This Section

- [5.1-data-modeling-levels.drawio](./5.1-data-modeling-levels.drawio)
- [5.2-etl-vs-elt.drawio](./5.2-etl-vs-elt.drawio)
- [5.3-data-warehouse-vs-lake.drawio](./5.3-data-warehouse-vs-lake.drawio)
