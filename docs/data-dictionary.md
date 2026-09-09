# Data Dictionary

This document describes the source fields and the main derived metrics used in the **SaaS Commercial Analytics** Power BI dashboard.

The original dataset provides short field descriptions but does not fully document the accounting meaning of some financial fields. To avoid overstating what the data represents, this project keeps the source terminology (`Sales`, `Profit`) and explicitly documents the interpretation used in the dashboard.

## Dataset Grain and Key Notes

- **Grain:** one row represents one sales transaction line within an Order.
- **Order ID** identifies an order and can repeat across multiple transaction lines.
- **Row ID** is unique across all 9,994 source rows.
- **License** is also unique across all 9,994 source rows in the current dataset and can therefore support row-level uniqueness checks.
- Geography fields are interpreted as **order geography** (where the order was recorded as placed), not as the customer's headquarters or permanent location.
- `Industry` behaves as a customer-level attribute in the dataset.
- `Segment` is not treated as a fixed customer attribute in this project because the same customer can appear under different Segment values.

## Source Fields

| Field | Data type | Definition used in this project | Interpretation / notes |
|---|---|---|---|
| **Row ID** | Integer | Unique identifier for a source transaction line. | Identifies the row, not the whole Order. |
| **Order ID** | Text | Unique identifier for an Order. | One Order can contain multiple transaction lines. Used to calculate distinct **Order Count**. |
| **Order Date** | Date | Date when the Order was placed. | Drives the dashboard Time Window and time-series analysis. |
| **Date Key** | Integer | Numeric representation of Order Date in `YYYYMMDD` format. | Used as a model key; not displayed as a business metric. |
| **Contact Name** | Text | Name of the person recorded as placing the Order. | Source-defined field; not used in the final dashboard. |
| **Country** | Text | Country recorded as where the Order was placed. | Treated as **order geography** in this project, not customer headquarters. |
| **City** | Text | City recorded as where the Order was placed. | Treated as order geography. Not used in the final dashboard. |
| **Region** | Text | Geographic region associated with the Order. | Values include AMER, EMEA, and APJ. Used by the **Order Region** slicer. |
| **Subregion** | Text | Geographic subregion associated with the Order. | More granular order geography below Region. |
| **Customer** | Text | Name of the company that placed the Order. | Used for customer-level contribution, concentration, and portfolio analysis. |
| **Customer ID** | Integer | Unique identifier for a Customer. | Used to identify customers consistently across Orders and support customer-level calculations. |
| **Industry** | Text | Industry to which the Customer belongs. | Used as **Customer Industry** in the dashboard. |
| **Segment** | Text | Segment label recorded for the transaction/order context (SMB, Strategic, Enterprise). | The source describes this as a customer segment, but it is not stable by Customer in the current dataset, so it is not treated as a fixed customer attribute in the dashboard model. |
| **Product** | Text | Product recorded on the transaction line. | A source product attribute; not used in the final dashboard pages. |
| **License** | Text | License key associated with the product transaction line. | Unique across the current source rows. Used as a row-uniqueness/data-quality reference rather than as a business KPI. |
| **Sales** | Decimal / currency | Source-defined monetary **Sales amount** for the transaction line. | Kept as `Sales`. The source does not document accounting revenue-recognition rules, so this project does **not** relabel it as recognized Revenue, ARR, or MRR. |
| **Quantity** | Integer | Number of items recorded for the transaction line. | The source does not specify that this represents seats, users, or licenses, so no more specific SaaS interpretation is assumed. |
| **Discount** | Decimal / percentage | Discount rate applied to the transaction line. | Stored as a decimal fraction (for example, `0.20` = 20%). |
| **Profit** | Decimal / currency | Source-defined **Profit amount** for the transaction line. | Can be negative. The source does not specify whether this is gross, operating, or net profit, or the exact cost basis used, so the project keeps the generic `Profit` label. |

## Derived Dashboard Metrics

These metrics are calculated from the source fields and form the common commercial measures used across the three dashboard pages.

| Metric | Definition | Interpretation |
|---|---|---|
| **Total Sales** | `SUM(Sales)` | Total source-defined Sales in the current filter context. |
| **Total Profit** | `SUM(Profit)` | Total source-defined Profit in the current filter context. |
| **Profit Margin** | `Total Profit / Total Sales` | Dataset-defined profitability ratio. It should not be interpreted as a specific accounting margin such as gross margin, operating margin, or net margin because the source does not define the underlying Profit type. |
| **Order Count** | Distinct count of `Order ID` | Number of unique Orders in the current filter context. |
| **Average Order Value (AOV)** | `Total Sales / Order Count` | Average Sales amount per Order. A change in AOV may reflect product mix, quantity, discounting, pricing, or other order-composition effects; it is not interpreted as price alone. |
| **Average Sales per Customer** | `Total Sales / distinct Customer count` | Average Sales generated per Customer in the current filter context. |
| **Top 10 Customer Sales Share** | Sales generated by the 10 highest-Sales Customers / Total Sales | Measures customer concentration at the top of the portfolio. |
| **Customers to 80% Sales** | Minimum number of Customers, ranked by Sales from highest to lowest, required to cumulatively reach at least 80% of Total Sales | Measures how broadly or narrowly Sales are distributed across the customer base. |
| **Loss-Making Customer Share** | Customers whose aggregated Profit is below zero / Customers in the analysis population | Measures the prevalence of loss-making Customers in the current analysis context. |
| **Customer Quintile Sales Share** | Sales generated by each dynamically ranked 20% Customer group / Total Sales | Customers are ranked by Sales within the current filter context and divided into five similarly sized groups. |

## Source

Original public dataset: [AWS SaaS Sales — Kaggle](https://www.kaggle.com/datasets/nnthanh101/aws-saas-sales)
