# Data Preparation and Modeling

This document summarizes how the flat SaaS sales source was prepared in Power Query and transformed into the dimensional model used by the Power BI dashboard.

## 1. Source Data and Grain

### Dataset overview

The source contains:

- **9,994 sales transaction lines**
- **5,009 Orders**
- **99 Customers**
- **14 Products**
- **48 Countries**
- Data covering **2020–2023**

The main analytical entities are **Order, Customer, Product, Geography, and Date**. Core numerical fields include **Sales, Quantity, Discount, and Profit**.

### Business grain

**One row represents one sales transaction line within an Order.**

`Order ID` is not unique because one Order can contain multiple transaction lines.

### Row identification and uniqueness checks

- `Row ID` is the documented unique identifier for each transaction.
- `Row ID` is unique across all **9,994 rows**.
- No exact duplicate rows were found.
- `License` is also unique in the current dataset, although the data dictionary does not define it as the transaction primary key.
- `Order ID` repeats as expected because an Order can contain multiple transaction lines.

These checks were used to confirm that the data is consistent with the expected transaction-line grain before building the dimensional model.

---

## 2. Power Query ETL

The source was prepared in Power Query using the following workflow:

```text
Raw source
→ Promote headers
→ Inspect values and data dictionary
→ Assign data types deliberately
→ Check conversion errors
→ Trim/Clean text columns
→ Check missing values
→ Validate duplicates and grain
→ Check business validity
→ Create staging layer
```

### Data-quality checks

**Missing values**

- Column profiling was performed on the full dataset.
- All columns were assigned their intended data types.
- Numerical columns contained no empty values or conversion errors.
- Text columns were trimmed and checked for both null and empty-string values.
- No missing values were identified.

**Duplicates and grain**

- No exact duplicate rows were found.
- `Row ID` remained unique.
- Repeated `Order ID` values were retained because they are expected at transaction-line grain.
- No duplicate transaction records were identified.

**Numerical validity**

Numerical fields were reviewed for invalid or suspicious values rather than automatically removing extreme observations.

- `Sales` contained no zero or negative values.
- `Quantity` contained no zero or negative values.
- `Discount` ranged from `0` to `0.8`.
- Negative `Profit` values were retained because they represented valid loss-making transactions.
- No transaction had `Profit` greater than `Sales`.

### Staging approach

A staging query was created from the cleaned source and used as the input for the dimensional model.

The staging layer is used to build the Dimensions and `FactSales`; it is not used directly for reporting.

---

## 3. Build A Star-Schema Model

The cleaned flat source was transformed into a star-schema model consisting of one Fact table and four main Dimensions.

### FactSales

**Grain:** one sales transaction line within an Order.

Contains:

- Foreign keys:
  - `DateKey`
  - `CustomerKey`
  - `ProductKey`
  - `CityKey`
- Numerical fields:
  - `Sales`
  - `Profit`
  - `Quantity`
  - `Discount`
- Business identifiers:
  - `Order ID`
  - `License`

`Order ID` and `License` are retained in `FactSales` as business / degenerate identifiers rather than being separated into standalone Dimensions with no additional descriptive attributes.

### DimCustomer

**Grain:** one row per Customer.

Contains:

- `CustomerID` - customer identifier
- `Customer`
- `Industry`

Design decisions:

- `Contact Name` was excluded because one Customer can have multiple contacts.
- `Segment` was excluded because the same Customer can appear under different Segment values. Without additional business context, it was not treated as a stable Customer attribute.

### DimProduct

**Grain:** one row per Product.

Contains:

- `ProductKey`
- `Product`

`License` was not included in `DimProduct` because one Product can have many Licenses.

### DimGeography

**Grain:** one row per unique city-location.

Contains:

- `CityKey`
- `Region`
- `Subregion`
- `Country`
- `City`

The geographic hierarchy is:

```text
Region → Subregion → Country → City
```

### DimDate

**Grain:** one row per calendar date.

A complete consecutive calendar is generated from the beginning of the first year containing Fact data through the end of the last year containing Fact data.

Typical attributes include:

- `DateKey`
- `Date`
- `Year`
- `Quarter`
- `Month`
- `Year-Month`
- `Month Name`
- `Day Name`

This supports consistent time hierarchies and time-based comparisons such as YoY analysis.

### Star-Schema Model structure

![Star Schema model](visuals/star-schema-model.PNG)

---

## 4. The Star-Schema Model Validation

After transforming the flat source into the dimensional model, validation checks were performed to ensure that the transformation did not change the underlying business data.

- **Fact row count preserved** - the Fact table remains consistent with the original transaction-line grain.
- **Dimension primary keys unique** - For each Dimension, the total row count was compared with the distinct count of its primary key; matching counts confirm one unique row per Dimension key.
- **No missing foreign keys** - Fact rows successfully map to the relevant Dimensions.
- **Totals unchanged** - key totals such as Sales and Profit remain consistent with the cleaned source.

## 5. Build Semantic Model
The semantic model was then configured with these steps:
- Configure relationships, cardinality and filter direction: connect each Dimension to FactSales using one-to-many relationships with single-direction filtering from Dimension to Fact.
- Mark DimDate as Date table
- Configure Sort by Column for some text fields
- Create hierarchies: Date hierarchy, Geography hierarchy
- Configure Data Categories: DimGeography[City] has Data category = City and DimGeography[Country] has Data category = Country/Region
- Hide technical fields



