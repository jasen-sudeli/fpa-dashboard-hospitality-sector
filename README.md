# Financial Planning & Analysis (FP&A) Dashboard - Hospitality Sector

## Executive Summary
This project represents a dynamic FP&A dashboard built to execute Budget vs. Actual variance analysis, operating margin tracking, and financial modeling for a hospitality business (2016-2022). The model automates the ingestion of raw accounting data to provide C-level executives with immediate visibility into liquidity and operational efficiency.

## Data Architecture & ETL (Extract, Transform, Load)
* **Data Cleaning & Transformation:** Executed via Power Query (M). Transformed raw, two-dimensional YTD (Year-To-Date) financial statements into a strict Tidy Data format using Unpivot methodologies.
* **Relational Modeling:** Built a robust Star Schema (1:*) connecting a massive Fact Table containing transactional records with a dimensional dictionary (Dim_Cuentas) to standardize financial taxonomy.
* **Date Normalization:** Applied conditional transformations to convert string quarters into precise end-of-period dates for Time Intelligence processing.

## DAX Engineering (Data Analysis Expressions)
Engineered a scalable measure hierarchy. The model calculates base transactions, isolates top-line revenue via cross-filtering, and computes the EBIT Margin while shielding the engine against divide-by-zero errors.

*Code implementation for Measure Lineage:*
```dax
// 1. Base Transactional Engine
1_Total_Soles = SUM(Data[Monto])

// 2. Top-Line Isolation (Cross-filter injection)
2_Ventas_Netas = CALCULATE([1_Total_Soles], Dim_Cuentas[Categoria_Analisis] = "Ingresos")

// 3. Core Operating Profitability KPI
3_Margen_Operativo_Pct = 
DIVIDE( 
    CALCULATE([1_Total_Soles], Dim_Cuentas[Categoria_Analisis] IN {"Ingresos", "Costos Directos", "Gasto Operativo", "Otros Operativos"}), 
    [2_Ventas_Netas], 
    0 
)
```

## Financial Insights & Business Impact

* **Pre-pandemic Margin Compression:** The operation exhibited structural fragility prior to the 2020 crisis, with the EBIT margin compressing steadily from 11.2% in 2016 down to 4.7% in 2019, indicating an escalation in fixed costs that outpaced revenue growth.
* **High-Density Cost Structure (Waterfall Analysis):** Direct Cost of Goods Sold (COGS) devours approximately 71% of total gross revenue ($0.70M out of $0.99M). The residual gross margin is subsequently absorbed by heavy Administrative OPEX ($0.24M).
* **The Profitability Bottleneck:** Despite generating close to $1M in cumulative top-line revenue, the business retains a dismal 1.5% ($15K) in operating profit (EBIT). This highlights a severe pricing inefficiency and unoptimized fixed-cost framework, requiring an immediate operational audit to prevent structural cash burn.
