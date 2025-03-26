
# Electric Vehicles EV-Dashboard

### Dashboard Link : https://app.powerbi.com/view?r=eyJrIjoiY2I3NDk0ZGMtNzBlZS00YjBlLWE2MTgtNGFjM2M4OTAzYTA2IiwidCI6ImNlOGY3MTI3LWQzMDMtNDgxOC05NWJiLTJiNmExNTZiZDQ5NSIsImMiOjF9

## Problem Statement

AtliQ Motors is an automotive giant from the USA specializing in electric vehicles (EV). In the last 5 years, their market share rose to 25% in electric and hybrid vehicles segment in North America. As a part of their expansion plans, they wanted to launch their bestselling models in India where their market share is less than 2%. Bruce, the chief of AtliQ Motors India wanted to do a detailed market study of existing EV/Hybrid market in India before proceeding further.


### Steps followed 

- Step 1 : AtliQ Motors uses PostgreSQL as its standard database management system for its analytical processes. Import the provided CSV files into PostgreSQL, creating and structuring tables to ensure data is efficiently stored and properly related (e.g., sales, dates, revenues).
- Step 2 : Establish a connection to access the database from Power BI and build a data model that reflects the relationships between tables.
- Step 3 : Also creating a new csv File and table by the name Revenue, assuming an average unit price of 2-wheeler and 4-wheelers EVs as Rs85000 and Rs1500000 respectively and importing into PostgreSQL and connecting to PowerBI.

## Primary Analysis
1. List the top 3 and bottom 3 makers for the fiscal years 2023 and 2024 in terms of the number of 2-wheelers sold
Calculated_Columns: Total EV sales per makers = sum(electric_vehicle_sales_by_makers[electric_vehicles_sold])
Filters and slicers for this visual:-
fiscal_year is 2023 or 2024
Maker-top 3 by Total EV sales per makers

2. Identify the top 5 states with the highest penetration rate in 2-wheeler and 4-wheeler EV sales in FY 2024
Peneration Rate = DIVIDE(sum(electric_vehicle_sales_by_state[electric_vehicles_sold]),sum(electric_vehicle_sales_by_state[total_vehicles_sold]))
   Peneration Rate 2024 = CALCULATE([Peneration Rate],dim_date[fiscal_year]=2024)


3. List the states with negative penetration (decline) in EV sales from 2022 to 2024?
Peneration Rate 2022 = CALCULATE([Peneration Rate],dim_date[fiscal_year]=2022)
Peneration Rate 2024 = CALCULATE([Peneration Rate],dim_date[fiscal_year]=2024)
Overall Peneration Rate Change(22-24) = [Peneration Rate Change(22-23)]+[Peneration Rate Change(23-24)]


4. What are the quarterly trends based on sales volume for the top 5 EV makers (4-wheelers) from 2022 to 2024?





5. How do the EV sales and penetration rates in Delhi compare to Karnataka for 2024?





6. List down the compounded annual growth rate (CAGR) in 4-wheeler units for the top 5 makers from 2022 to 2024
CAGR = DIVIDE(CALCULATE([Total EV sales per makers], dim_date[fiscal_year] = 2024), CALCULATE([Total EV sales per makers], dim_date[fiscal_year] = 2022),BLANK()) ^ (1 / 2) - 1
CAGR For Makers = 
VAR EndValue = CALCULATE([Total EV sales per makers], dim_date[fiscal_year] = 2024)
VAR StartValue = CALCULATE([Total EV sales per makers], dim_date[fiscal_year] = 2022)
VAR Periods = 2
RETURN
IF(
    StartValue > 0,
    ( (EndValue / StartValue) ^ (1 / Periods) ) - 1,
    BLANK()
)


7. List down the top 10 states that had the highest compounded annual growth rate (CAGR) from 2022 to 2024 in total vehicles sold
CAGR For States = 
VAR EndValue = CALCULATE([Total sales per state], dim_date[fiscal_year] = 2024)
VAR StartValue = CALCULATE([Total sales per state], dim_date[fiscal_year] = 2022)
VAR Periods = 2
RETURN
IF(
    StartValue > 0,
    ( (EndValue / StartValue) ^ (1 / Periods) ) - 1,
    BLANK()
)


8. What are the peak and low season months for EV sales based on the data from 2022 to 2024?



9. What is the projected number of EV sales (including 2-wheelers and 4-wheelers) for the top 10 states by penetration rate in 2030, based on the
compounded annual growth rate (CAGR) from previous years?
Projected EV Sales 2030 = 
VAR CurrentYear = 2024
VAR FutureYear = 2030
VAR YearsToProject = FutureYear - CurrentYear
VAR CurrentEVSales = CALCULATE([Total EV Sales per State], dim_date[fiscal_year] = CurrentYear)
VAR CAGREVSales = [CAGR EV Sales For States]
RETURN
IF(
    NOT ISBLANK(CAGREVSales),
    CurrentEVSales * (1 + CAGREVSales) ^ YearsToProject,
    BLANK()
)

10. Estimate the revenue growth rate of 4-wheeler and 2-wheelers EVs in India for 2022 vs 2024 and 2023 vs 2024, assuming an average unit price of 2-wheeler and 4-wheelers EVs as Rs85000 and Rs1500000 respectively
Revenue Growth Rate 2022-2023 = 
DIVIDE(
    [2023 Revenue] - [2022 Revenue], 
    [2022 Revenue]
)
Revenue Growth Rate 2022-2024 = 
DIVIDE(
    [2024 Revenue] - [2022 Revenue], 
    [2022 Revenue]
)
Revenue Growth Rate 2023-2024 = 
DIVIDE(
    [2024 Revenue] - [2023 Revenue], 
    [2023 Revenue]
)

11. Two Wheeler EVs sold= 1913168
    2 Wheelers sold = CALCULATE(SUM(electric_vehicle_sales_by_makers[electric_vehicles_sold]),electric_vehicle_sales_by_makers[vehicle_category] = "2-Wheelers")

12. Four Wheeler EVs sold= 152943
    4 Wheelers sold = CALCULATE(SUM(electric_vehicle_sales_by_makers[electric_vehicles_sold]),electric_vehicle_sales_by_makers[vehicle_category] = "4-Wheelers")

