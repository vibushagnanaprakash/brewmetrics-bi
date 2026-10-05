# BrewMetrics BI

A version-controlled Business Intelligence solution for BrewMetrics Coffee Co. developed using Power BI, Git, GitHub, and GitHub Copilot.

## 1. Project Overview

The BrewMetrics BI project analyses coffee sales data to understand sales performance across cities, store formats, product categories, individual items, and time.

The original sales dataset was transformed into a structured star schema using Power Query. DAX measures were then created to support sales, quantity, growth, running-total, and ranking analysis.

An interactive Power BI dashboard was developed to help analyse business performance and identify important sales trends.

The project also uses Git and GitHub for version control and GitHub Copilot to assist with DAX development.

## 2. Dataset

The project uses the `brewmetrics_sales.csv` dataset.

The dataset contains 15,482 sales transactions with the following main fields:

- `sale_id` – unique identifier for each sale
- `date` – date of the transaction
- `city` – city where the sale occurred
- `store_format` – format of the store
- `category` – product category
- `item` – individual product/item
- `quantity` – quantity sold
- `unit_price` – price per unit
- `sales_amount` – total sales amount

## 3. Data Model

The Power BI model follows a star schema with `Fact_sales` as the central fact table connected to four dimension tables.

### Fact Table

**Fact_sales**

The `Fact_sales` table contains transaction-level sales information, including sale ID, date, city, store format, product information, quantity, unit price, and sales amount.

### Dimension Tables

**Dim_date**

Contains date information used for time-based analysis:

- `date`
- `Year`
- `Month`

**Dim_city**

Contains the unique cities and is used for city-level analysis and filtering.

**Dim_store**

Contains the different store formats and supports store-format analysis.

**Dim_product**

Contains product information:

- `category`
- `item`

The dimension tables are connected to the `Fact_sales` table using one-to-many relationships.

## 4. DAX Measures

The following DAX measures were created.

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

The Power BI dashboard provides an interactive view of BrewMetrics sales performance.

The dashboard contains:

Total Sales KPI
Total Quantity KPI
MoM Sales Growth % KPI
Sales by City column chart
Sales Trend line chart
Total Sales by Category donut chart
City → Store Format drill-down chart
Total Sales by Item bar chart
Cold Brew Sales by Month line chart
City slicer
Cold Brew Seasonal Analysis

A separate monthly sales chart is used to analyse Cold Brew performance.

The item field is filtered to Cold Brew at the visual level, so the chart displays the monthly sales trend for Cold Brew only.

The rest of the dashboard continues to analyse the overall sales dataset.

6. Key Dashboard Insights
1. Bengaluru has the highest city sales

The Sales by City chart shows that Bengaluru has the highest total sales among the four cities, followed by Chennai, Hyderabad, and Coimbatore.

The approximate sales values are:

Bengaluru – ₹1.12M
Chennai – ₹1.05M
Hyderabad – ₹0.97M
Coimbatore – ₹0.82M

This indicates that Bengaluru is the strongest-performing city in the dataset.

2. Coffee is the largest sales category

The Total Sales by Category chart shows that Coffee contributes the largest share of total sales, at approximately 45.49%.

Merchandise contributes approximately 38.81%, while Bakery contributes approximately 15.69%.

This indicates that Coffee is the main contributor to overall sales.

3. Cold Brew shows a seasonal sales pattern

The Cold Brew Sales by Month chart uses a visual-level filter for the item field, selecting Cold Brew only.

The monthly trend can therefore be used to observe changes in Cold Brew sales across the displayed months, particularly the increase during the April–May period.

7. GitHub Copilot

GitHub Copilot was used through Visual Studio Code to assist with the development of DAX measures and project documentation.

Copilot-generated suggestions were reviewed against the Power BI data model before being incorporated into the project.

The Copilot-assisted development process and observations are documented in NOTES.md.

8. Version Control

Git and GitHub were used to maintain a version-controlled history of the project.

Different development stages were committed separately, including:

Initial project setup
Star schema creation
Total Sales measure
Month-over-Month Sales Growth measure
Running Total Sales measure
City Sales Ranking measure
Total Quantity measure
Dashboard development
Project documentation

This provides a clear history of the project development process.

9. Project Files

The repository contains:

README.md – project documentation
NOTES.md – GitHub Copilot and DAX development notes
REFLECTION.md – project reflection
Power BI .pbip project files
Semantic model files
Source dataset
10. Conclusion

The BrewMetrics BI solution provides an interactive way to analyse sales performance across cities, categories, products, store formats, and time.

The Cold Brew monthly analysis provides an additional view of product seasonality, while the city slicer and drill-down functionality allow users to explore the data at different levels.

The project demonstrates the use of Power BI, DAX, Git, GitHub, and GitHub Copilot together to develop a structured and version-controlled Business Intelligence solution.