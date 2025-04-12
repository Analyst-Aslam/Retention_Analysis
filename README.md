# Retention Analysis


## Overview

This project involves conducting a Cohort Analysis using a real world dataset. The aim is create Cohort Table showing customer retention which helps one to understand customer loyalty and stickiness.

---

## Dataset Information

- **Source**: Sales data is available in the repository in a compressed form. Access and decompress it to use.
  (Access the dataset from [GitHub Repository Link](https://github.com/Analyst-Aslam/Retention_Analysis))


---

## How to Use the Dashboard

### Requirements:

- **Power BI Desktop**  
  (Download from [here](https://www.microsoft.com/en-us/download/details.aspx?id=58494)).

- **Dataset File**  
  Download the dataset in Compressed form [Data](https://github.com/Analyst-Aslam/Retention_Analysis/blob/main/data.csv.zip).

- **Power BI Dashboard File**
  Download the dashboard [Retention Analysis](https://github.com/Analyst-Aslam/Retention_Analysis/blob/main/Retention%20Analysis.pbix)
---

### Steps to Load the Dashboard:

1. Open **Power BI Desktop**.
2. Click on `File` > `Open` and select the `Retention Analysis.pbix` file for the dashboard.
3. If necessary, load the dataset by selecting **Transform Data** > **Apply Changes**.
4. Interact with the visualizations to explore the key insights.

---

## STEPS

### Step 1: Create a Calendar Table

Use the following DAX code to create a calendar table with rich date attributes:

```dax
Calendar = 
ADDCOLUMNS(
    CALENDAR(DATE(2010,1,1), DATE(2011,12,31)),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMM"),
    "Year-Month", FORMAT([Date], "YYYY-MMM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Weekday", FORMAT([Date], "dddd"),
    "Weekday Short", FORMAT([Date], "ddd")
)
```

> **Model View**: After creating this table, connect  
> `Calendar[Date]` ➝ `data[InvoiceDate]` using a one-to-many relationship.

---

### Step 2: Calculate Cohort Date for Each Customer

This calculated column returns the **end of the month of each customer's first purchase**.

```dax
CohortDate = 
CALCULATE(
    EOMONTH(MIN(data[InvoiceDate]), 0),
    ALLEXCEPT(data, data[CustomerID])
)
```

---

### Step 3: Create a Measure to Count Distinct Customers

This measure returns the number of unique customers:

```dax
CustomerCount = DISTINCTCOUNT(data[CustomerID])
```

---

### Step 4: Create a Cohort Column to Show Month Difference

This column shows the number of months between the cohort date and the transaction invoice date. It's useful for building **cohort matrices**.

```dax
CohortColumn = 
DATEDIFF(
    data[CohortDate],
    EOMONTH(data[InvoiceDate], 0),
    MONTH
)
```
### Step 5: Create a Cohort Table

- **Select Matrix Visualization**
- **Add CohortColumn to Column**
- **Add CohortDate to Rows**
- **Add CustomerCount to Values**

### Step 6: Create a Retention Table

Create a Measure as

```dax
Cohort = 
    VAR _FirstMonth=CALCULATE(data[CustomerCount],FILTER(ALL(data[CohortColumn]),data[CohortColumn]=0))
    VAR _Cohort =DIVIDE(data[CustomerCount],_FirstMonth)
RETURN _Cohort
```

- **Select Matrix Visualization**
- **Add CohortColumn to Column**
- **Add CohortDate to Rows**
- **Add Cohort to Values**


---

## Potential Use Cases

- **User Engagement Tracking**  

- **Revenue/LTV Analysis**  

- **Marketing Campaign Performance**  

- **Churn Behaviour**  

---


## Contact

For any questions or inquiries, feel free to reach out!
