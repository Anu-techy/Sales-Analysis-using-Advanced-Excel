# Sales-Analysis-using-Advanced-Excel
Analysing Sales of a Hardware company based on
1. Customer Performance Report
2. Market Performance Report
3. KPI (Key Performance Indicators)

**Aim**
To Analyze sales of the company to unveal hidden insights and give actionable recommendations

=========================================================================================

**Objective**
To Create a Customer Performance Report

**Description:**

Atliq is a hardware company which manufactures electronic hardware items like PC, Laptop, Hard Drive, mouse, Keyboards, pendrives etc
It has operations all over the globe.

It sells to consumers throught the following **distribution channels:**
1. Direct Channel (Atliq E-stroe, Atliq Exclusive)
2. Retailer (Croma, amazon)
3. Distributer( eg Neptune in China)

It has **customers** of two categories:
1. Brick and Mortar (like Croma, BestBuy)
2. E-commerce ( like Amazon, Flipkart)

**ETL (Extract Transform Load) Process**

Data is given in various excel sheets.
Extracted Data through various sources

**Dimension tables :** dim_customer, dim_product, dim_market

**Fact table/Transaction table :** fact_sales_monthly

**Data Transformations in Power Query**

1. Removed duplicate values
2. Removed data with missing values (only 3 rows)
3. Identified, replaced spelling mistakes in customer names
4. Replaced nan category in market table to NA (North American Region) ater confirming with business owners
5. Few rows have negative quantity , Replaced with positive after clear confirmation
6. Named all the data transformation steps
7. Load the data to data model
8. Created dim_date table with date, month, FY (fiscal_year) columns.

The **Dim_Date** table helps in analysis by providing a structured way to categorize and group data based on 
time (e.g., by day, month, quarter, or year), enabling easier and more flexible time-based analysis, 
such as trend analysis, period-over-period comparisons, and time-based calculations (like YTD or MTD).
Fiscal year of Atliq hardware is from september through August.

**Data Modelling**

dim_custommer(customer_code)        --->     fact_sales_monthly(customer_code)   one to many

dim_product(product_code)           --->     fact_sales_monthly(product_code)    one to many

We cannot connect dim_market directly to fact_sales_monthly as there is no common column between dim_market and fact_sales_monthly table.
Hence we connect dim_market to dim_customer

dim_market(market)                  --->     dim_customer(market)         one to many

dim_date(date)                      --->     fact_sales_monthy(date)      one to many

**DAX Measures created for Customer Performance Report**

In the Power Pivot, Created the following DAX measures in the fact_sales_monthly table

**Measure 1:**            Net Sales = SUM(fact_sales_monthly[net_sales_amount])

To create net sales measure for 2019,2020 and 2021, got FY column of dim_date to fact_sales_monthly table
using the formula =RELATED(dim_date[FY])

**Measure 2:**            Net Sales 19 = CALCULATE ( [Net Sales], dim_date[FY]="2019")

**Measure 3:**            Net Sales 20 = CALCULATE ( [Net Sales], dim_date[FY]="2020")

**Measure 4:**            Net Sales 21 = CALCULATE ( [Net Sales], dim_date[FY]="2021")

**Measure 5**:            21 vs 20 = DIVIDE([Net Sales 21],[Net Sales 20],0)

**Final Customer Performance Report Designing**

In the Power Pivot: 

**Row   :**             Customer (dim_customer)

**Values:**             Net Sales 19

                        Net Sales 20
          
                        Net Sales 21

                        21 vs 20
          
**Filters:**            Region  (dim_market)

                        Market  (dim_market)
          
                        Division (dim_product)

Added conditional formatting and improved aesthetics

==========================================================================================

**Objective**
To Create a Market Performance Report

**Note:**
Here market is the country name where the sales are analyzed.

**ETL (Extract Transform Load) Process**

Separate ns_target_2021 table in .csv file is provided by the business owner.
Extracted the same and loaded to the Data model after transformation in power query.

**Data Modelling**


dim_market(market)                  --->     ns_targets_2021(market)         one to many

dim_date(date)                      --->     ns_targets_2021(date)           one to many

**DAX Measures created for Market Performance Report**

In the Power Pivot, Created the following DAX measures in the fact_sales_monthly table

**Measure 1:**            Target 21 = SUM(ns_targets_2021[ns_target])

**Measure 2:**            2021 target = [Net Sales 21] - [Target 21]

**Measure 3:**            % Change = DIVIDE([2021 target],[Net Sales 21],0)


**Final Market Performance Report Designing**

In the Power Pivot: 

**Row   :**             market (dim_market)

**Values:**             Net Sales 19

                        Net Sales 20
          
                        Net Sales 21

                        2021 target

          
**Filters:**            Region  (dim_market)
          
                        Division (dim_product)

Added conditional formatting and improved aesthetics


**Recommendations** 

Use Report to 

1. Understand customers and market performance over the time
   
2. Slice and dice data to drill down the hidden insights

3. Determine discounts, helps to negotiate with consumers, and identify potential business expansion opportunities in promising countries.

