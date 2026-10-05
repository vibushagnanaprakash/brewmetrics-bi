# BrewMetrics BI

A version-controlled Business Intelligence solution for BrewMetrics Coffee Co.

## 1. Project Overview

This project develops a Business Intelligence solution for BrewMetrics Coffee Co. using Power BI.

The project uses sales transaction data to analyse sales performance across cities, store formats, products and time periods.

The project demonstrates:

- Data cleaning and transformation using Power Query
- Star schema data modelling
- DAX measures
- Interactive Power BI dashboard development
- Git and GitHub version control
- GitHub Copilot assistance for DAX development

## 2. Dataset

The dataset used for this project is:

`brewmetrics_sales.csv`

The dataset contains 15,482 sales transactions.

The main columns are:

- `sale_id`
- `date`
- `city`
- `store_format`
- `category`
- `item`
- `quantity`
- `unit_price`
- `sales_amount`

## 3. Data Model

The flat sales dataset was transformed into a **star schema** using Power Query.

### Fact Table

**Fact_sales**

Contains the transaction-level sales data:

- sale_id
- date
- city
- store_format
- category
- item
- quantity
- unit_price
- sales_amount

### Dimension Tables

**Dim_date**
- date
- Year
- Month

**Dim_city**
- city

**Dim_store**
- store_format

**Dim_product**
- category
- item

### Relationships

The following relationships were created:

- Dim_date → Fact_sales
- Dim_city → Fact_sales
- Dim_store → Fact_sales
- Dim_product → Fact_sales

The dimension tables have a one-to-many relationship with the Fact_sales table.

## 4. DAX Measures

The following DAX measures were created for analysis.

### Total Sales

```DAX
Total Sales =
SUM(Fact_sales[sales_amount])
MoM Sales Growth %
MoM Sales Growth % =
VAR CurrentMonthSales = [Total Sales]
VAR PreviousMonthSales =
    CALCULATE(
        [Total Sales],
        DATEADD(Dim_date[date], -1, MONTH)
    )
RETURN
    DIVIDE(
        CurrentMonthSales - PreviousMonthSales,
        PreviousMonthSales
    )
Running Total Sales
Running Total Sales =
CALCULATE(
    [Total Sales],
    FILTER(
        ALL(Dim_date[date]),
        Dim_date[date] <= MAX(Dim_date[date])
    )
)
City Sales Rank
City Sales Rank =
RANKX(
    ALL(Dim_city[city]),
    [Total Sales],
    ,
    DESC,
    DENSE
)
Total Quantity
Total Quantity =
SUM(Fact_sales[quantity])
5. Power BI Dashboard

The dashboard was created to provide an interactive view of BrewMetrics sales performance.

The dashboard includes:

Total Sales KPI
Total Quantity KPI
Month-over-Month Sales Growth KPI
Sales trend over time
Sales by City
Cold Brew seasonal analysis
Sales by Category
Top 10 Products
Quantity by City
Sales by Store Format
City slicer
City → Store Format drill-down

The drill-down allows users to move from a city level to the store-format level for more detailed analysis.

6. Key Business Insights
City Performance

Bengaluru has the highest total sales among the four cities, followed by Chennai, Hyderabad and Coimbatore.

Cold Brew Seasonality

Cold Brew sales show a seasonal increase during the April–May period.

Sales Analysis

The dashboard combines overall sales, quantity, city, product and time-based analysis to help identify sales trends and performance differences.

7. GitHub Copilot

GitHub Copilot was used in Visual Studio Code to assist with creating DAX measures.

Copilot suggestions were reviewed against the Power BI semantic model before being added to the project.

The Copilot-assisted work and observations are documented in:

NOTES.md

8. Version Control

Git and GitHub were used to maintain the project history.

The project was developed through separate commits for different stages, including:

Initial project setup
Star schema creation
Total Sales measure
Month-over-Month Growth measure
Running Total Sales measure
City Sales Ranking measure
Total Quantity measure
Dashboard development
Documentation

This provides a clear version history of the development process.

9. Project Files

The repository contains:

README.md – Project documentation
NOTES.md – GitHub Copilot and DAX documentation
REFLECTION.md – Project reflection
Power BI .pbip project files
Semantic model files
Source dataset