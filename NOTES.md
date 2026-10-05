# Copilot DAX Development Notes

## Project

**BrewMetrics BI – Version-Controlled Business Intelligence Solution**

GitHub Copilot in VS Code was used to assist with creating DAX measures for the Power BI semantic model. The generated DAX was reviewed to ensure that the table and column names matched the BrewMetrics data model before adding the measures to the PBIP/TMDL project.

---

## 1. Total Sales

### Purpose

Calculates the total sales amount from the `Fact_sales` table.

### Copilot-Generated DAX

```DAX
Total Sales =
SUM(Fact_sales[sales_amount])
Review

The measure was reviewed against the Fact_sales[sales_amount] column and found to be appropriate for calculating total sales.

2. MoM Sales Growth %
Purpose

Calculates the percentage change in sales compared with the previous month.

Copilot-Generated DAX
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
Review

The measure was reviewed to ensure that it uses the existing Total Sales measure and the Dim_date[date] column. The calculation returns the month-over-month percentage change.

3. Running Total Sales
Purpose

Calculates cumulative sales from the beginning of the available date range up to the current date.

Copilot-Generated DAX
Running Total Sales =
CALCULATE(
    [Total Sales],
    FILTER(
        ALL(Dim_date[date]),
        Dim_date[date] <= MAX(Dim_date[date])
    )
)
Review

The measure was reviewed to ensure that it uses the existing Total Sales measure and the date column from Dim_date. The filter allows sales to accumulate over time.

4. City Sales Rank
Purpose

Ranks cities according to their total sales, with the highest-selling city receiving rank 1.

Copilot-Generated DAX
City Sales Rank =
RANKX(
    ALL(Dim_city[city]),
    [Total Sales],
    ,
    DESC,
    DENSE
)
Review

The measure was reviewed to ensure that the ranking is based on Total Sales and that all cities are considered. Descending order and dense ranking were used.

5. Total Quantity
Purpose

Calculates the total quantity of products sold.

Copilot-Generated DAX
Total Quantity =
SUM(Fact_sales[quantity])
Review

The measure was reviewed against the Fact_sales[quantity] column and is used to calculate the overall quantity sold.

Copilot Usage Summary

GitHub Copilot was used in VS Code to generate and assist with the DAX expressions. The generated code was reviewed before being added to the Power BI PBIP/TMDL semantic model.

The table and column names were checked against the project schema to ensure that the measures matched the implemented star schema.

The measures were added and committed separately to maintain a clear version-control history.

Validation

The DAX measures were reviewed for:

Correct table and column names
Correct use of existing measures
Appropriate DAX functions
Compatibility with the implemented star schema
Suitability for use in Power BI dashboard visuals

