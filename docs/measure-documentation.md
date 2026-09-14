# Measure Documentation

This document summarizes the DAX measures used in the report, including their organization, dependencies, and direct or indirect visual usage.

## Measure Summary

| Measure | Folder Path | Depends On | Direct Visuals | Indirect Visuals |
|---|---|---|---|---|
| **Active Customers** | `Measure / Others` | Order Count | — | Average Sales per Customer |
| **Average Order Value** | `Measure / Base` | Order Count; Total Sales | Average Order Value, Sales Growth Drivers, Customer Industry Performance Detail, Country Performance Detail, Customer Performance Detail | — |
| **Average Order Value PY** | `Measure / Previous Year` | Average Order Value; Has Full Same Period PY | — | Average Order Value |
| **Average Order Value YoY** | `Measure / YoY` | Average Order Value; Average Order Value PY | — | Average Order Value |
| **Average Order Value YoY Color** | `Measure / YoY` | Average Order Value YoY; Has Full Same Period PY | Average Order Value | — |
| **Average Order Value YoY Display** | `Measure / YoY` | Average Order Value YoY; Has Full Same Period PY | Average Order Value | — |
| **Average Sales per Customer** | `Measure / Others` | Active Customers; Total Sales | Average Sales per Customer | — |
| **Country Chart Title** | `Country Metric Selector / (root)` | Selected Country Metric Name | Top 5 Country Contributors | — |
| **Customer Chart Title** | `Customer Metric Selector / (root)` | Selected Customer Metric Name | Top 5 Customer Contributors | — |
| **Customer Quintile Sales** | `Customer Sales Quintile / (root)` | Total Sales | — | Customer Sales Distribution |
| **Customer Quintile Sales Share** | `Customer Sales Quintile / (root)` | Customer Quintile Sales; Total Sales | Customer Sales Distribution | — |
| **Customer X Axis Max** | `Measure / Customer Performance Profile` | Order Count | Customer Performance Profiles | — |
| **Customer Y Axis Max** | `Measure / Customer Performance Profile` | Max Customer PM; Min Customer PM | Customer Performance Profiles | — |
| **Customer Y Axis Min** | `Measure / Customer Performance Profile` | Max Customer PM; Min Customer PM | Customer Performance Profiles | — |
| **Customers Count in Quintile** | `Customer Sales Quintile / (root)` | Total Sales | Customer Sales Distribution | — |
| **Customers to 80% Sales** | `Measure / Others` | Total Sales | Customers to 80% Sales | — |
| **Has Full Same Period PY** | `Measure / Previous Year` | — | — | Total Sales, Total Profit, Profit Margin, Order Count, Average Order Value |
| **Industry X Axis Max** | `Measure / Industry Performance Profile` | Order Count | Customer Industry Performance Profiles | — |
| **Industry Y Axis Max** | `Measure / Industry Performance Profile` | Profit Margin | Customer Industry Performance Profiles | — |
| **Industry Y Axis Min** | `Measure / Industry Performance Profile` | Profit Margin | Customer Industry Performance Profiles | — |
| **Loss-Making Customer Share** | `Measure / Others` | Total Profit; Total Sales | Loss-Making Customer Share | — |
| **Max Customer PM** | `Measure / Customer Performance Profile` | Order Count; Profit Margin | — | Customer Performance Profiles |
| **Min Customer PM** | `Measure / Customer Performance Profile` | Order Count; Profit Margin | — | Customer Performance Profiles |
| **Order Count** | `Measure / Base` | — | Order Count, Sales Growth Drivers, Customer Industry Performance Profiles, Customer Industry Performance Detail, Country Performance Detail, Customer Performance Profiles, Customer Performance Detail | Average Order Value, Average Sales per Customer |
| **Order Count PY** | `Measure / Previous Year` | Has Full Same Period PY; Order Count | — | Order Count |
| **Order Count YoY** | `Measure / YoY` | Order Count; Order Count PY | — | Order Count |
| **Order Count YoY Color** | `Measure / YoY` | Has Full Same Period PY; Order Count YoY | Order Count | — |
| **Order Count YoY Display** | `Measure / YoY` | Has Full Same Period PY; Order Count YoY | Order Count | — |
| **Profit Margin** | `Measure / Base` | Total Profit; Total Sales | Profit Margin, Sales and Profitability Trend, Customer Industry Performance Profiles, Customer Industry Performance Detail, Country Performance Detail, Customer Performance Profiles, Customer Performance Detail | — |
| **Profit Margin PY** | `Measure / Previous Year` | Has Full Same Period PY; Profit Margin | — | Profit Margin |
| **Profit Margin YoY** | `Measure / YoY` | Has Full Same Period PY; Profit Margin; Profit Margin PY | — | Profit Margin |
| **Profit Margin YoY Color** | `Measure / YoY` | Has Full Same Period PY; Profit Margin YoY | Profit Margin | — |
| **Profit Margin YoY Display** | `Measure / YoY` | Has Full Same Period PY; Profit Margin YoY | Profit Margin | — |
| **Profit PY** | `Measure / Previous Year` | Has Full Same Period PY; Total Profit | — | Total Profit |
| **Profit YoY Color** | `Measure / YoY` | Has Full Same Period PY; Profit PY; Total Profit | Total Profit | — |
| **Profit YoY Display** | `Measure / YoY` | Has Full Same Period PY; Profit PY; Total Profit | Total Profit | — |
| **Sales PY** | `Measure / Previous Year` | Has Full Same Period PY; Total Sales | — | Total Sales |
| **Sales YoY** | `Measure / YoY` | Sales PY; Total Sales | — | Total Sales |
| **Sales YoY Color** | `Measure / YoY` | Has Full Same Period PY; Sales YoY | Total Sales | — |
| **Sales YoY Display** | `Measure / YoY` | Has Full Same Period PY; Sales YoY | Total Sales | — |
| **Selected Country Metric** | `Country Metric Selector / (root)` | Total Profit; Total Sales | Top 5 Country Contributors | — |
| **Selected Country Metric Name** | `Country Metric Selector / (root)` | — | — | Top 5 Country Contributors |
| **Selected Customer Metric** | `Customer Metric Selector / (root)` | Total Profit; Total Sales | Top 5 Customer Contributors | — |
| **Selected Customer Metric Name** | `Customer Metric Selector / (root)` | — | — | Top 5 Customer Contributors |
| **Top 10 Customer Sales Share** | `Measure / Others` | Total Sales | Top 10 Customer Sales Share | — |
| **Total Profit** | `Measure / Base` | — | Total Profit, Customer Industry Performance Detail, Country Performance Detail, Customer Performance Detail | Top 5 Customer Contributors, Top 5 Country Contributors, Profit Margin, Sales and Profitability Trend, Customer Industry Performance Profiles, Customer Performance Profiles, Loss-Making Customer Share |
| **Total Sales** | `Measure / Base` | — | Total Sales, Sales and Profitability Trend, Customer Industry Performance Profiles, Customer Industry Performance Detail, Country Performance Detail, Country Sales Concentration, Customer Performance Profiles, Customer Performance Detail | Top 5 Customer Contributors, Top 5 Country Contributors, Profit Margin, Average Order Value, Sales Growth Drivers, Top 10 Customer Sales Share, Customers to 80% Sales, Loss-Making Customer Share, Average Sales per Customer, Customer Sales Distribution |
| **Treemap Color Sales** | `Measure / Others` | Total Sales | Country Sales Concentration | — |
