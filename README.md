# MIST-4610-Group-7 - Project 2

## Team Name:
61608 - Group 7
## Team Members:
1. Jamie Kim - [@Jamie-S-Kim](https://github.com/Jamie-S-Kim)
2. Sienna Wong - [@s1ennawong](https://github.com/s1ennawong)
3. Shaehaan Khaja - [@shaehaankhaja](https://github.com/shaehaankhaja)
4. Seth White - [@sethwhite444](https://github.com/sethwhite444)
5. Owen Verlander - [@owenver](https://github.com/owenver)

## Dataset Description
- We selected the COVID-19 Epidemiological Data because it contains a wide range of real-world datasets from multiple sources such as Policy Measures from the CDC and Demographics from the Databank. Having multiple sources allows for a deeper multi-dimensional analysis of the data. This allows us to go beyond simple case tracking and analyze relationships between policy, behavior, healthcare capacity, and COVID outcomes.

- However, we decided to focus on two tables SCS_BE_DETAILED_MORTALITY and JHU_COVID_19

- SCS_BE_DETAILED_MORALITY contains 1 table with approximately 11,413 rows. Key columns include DATE (string, YYYY-MM-DD format) serving as the join key for time, REGION (string) as the geographic identifier mapping to country/state, AGEGROUP (string) enabling demographic breakdowns, SEX (string) for optional deeper breakdowns, and DEATHS (integer) acting as the numerator in mortality rate calculations.
- JHU_COVID_19 has 9738292 rows and key columns include DATE (string, YYYY-MM-DD format) serving as the join key for time, COUNTRY_REGION (string) as the geographic identifier, CASE_TYPE (string) indicating the classification of cases such as confirmed or deaths, and CASES (integer) representing the count of cases.


## Questions and Justification
Question 1. How did COVID-19 mortality rates vary across age groups in Belgium over time?

Which columns are relevant?

From JHU_COVID_19
- Date - allowing alignment of time between both datasets
- Country region - Belgium
- Case type - used to filter for “confirmed” cases (total cases)
- Cases - used as a denominator in mortality rate

From SCS_BE_DETAILED_MORTALITY
- Date- join key for time
- Region - Belgium
- Age Group - allows the demographic breakdown
- Sex - could be an optional deeper breakdown of data
- Deaths - numerator in mortality rate

What makes this question non-trivial?

- Filtering; Filters JHU_COVID_19 by case type with confirmed cases
- Joining tables; Join on date, Join on geographic field (country region <> region) Belgium
- Aggregation; Group by date, country region, age group
- Creating a new metric; Calculating mortality rate; deaths / cases
- Comparing over time and demographics; Analyze how mortality rates differs: across age groups, countries, time

Why this is interesting / meaningful?
This question is meaningful from a public health and social perspective because it identifies which age groups were most vulnerable to COVID-19. Understanding how mortality differs by age can help inform healthcare resource allocation, vaccination strategies, and policy decisions aimed at protecting high-risk populations. It also provides insight into the broader societal impact of the pandemic on different demographic groups.

Question 2: How did the growth rate of COVID-19 cases relate to changes in mortality across Belgium over time?

Which columns are relevant?

From SCS_BE_DETAILED_MORTALITY
- Date - join key
- Region - Belgium
- Deaths - deaths over time

From JHU_COVID_19
- Date - used for time series analysis
- Country region - belgium
- Case type - filtered for "confirmed"
- Cases - total case count
- Difference - growth rate / new cases

What makes this question non-trivial?

- Filtering; Extract confirmed cases from case type
- Use a derived metric; Use difference to measure case growth
- Joining tables; Join cases and deaths using date and geographic identifiers
- Aggregation; Group by date and country
- Comparing trends over time; Analyze spikes in case growth, corresponding changes in deaths

Why is this interesting / meaningful?
This question is meaningful from an operational and public health perspective because it helps determine whether increases in COVID-19 cases led to increases in deaths, and how quickly those changes occurred. Understanding this relationship can provide insight into the severity of different waves of the pandemic and the effectiveness of interventions such as lockdowns or healthcare responses. It also helps evaluate how well systems handled rising case loads.

## Data Manipulations
Query 1:
<img width="624" height="218" alt="Screenshot 2026-04-27 at 3 12 57 PM" src="https://github.com/user-attachments/assets/32b55459-16e6-4be5-a8e5-762de42592b7" />

What it does: This query pulls from the mortality table and aggregates total deaths by date, age group, and region. The SUM(DEATHS) collapses any duplicate or sub-grouped rows into a single death count per combination of those three fields.

Why each piece matters:

- WHERE AGEGROUP IS NOT NULL — filters out rows that lack an age group label, which would skew the demographic breakdown or create an unclassified category in the visualization
- GROUP BY DATE, AGEGROUP, REGION — ensures deaths are bucketed at the right granularity for both the time series and the age group comparison
- ORDER BY DATE — sorts chronologically so the data is ready for time series charting without additional transformation

Why this is non-trivial: The raw mortality table contains multiple rows per date due to sex-level breakdowns (male/female). Without the aggregation, deaths would be double-counted in the chart. The SUM with the correct GROUP BY collapses those into a single meaningful total per age group per date.

Query 2:
<img width="668" height="361" alt="Screenshot 2026-04-27 at 3 13 58 PM" src="https://github.com/user-attachments/assets/67c0436e-05da-468e-a9e6-5573cf283f64" />
What it does: This query joins the two tables on DATE to compare new COVID-19 case growth against total deaths in Belgium over time. It uses DIFFERENCE from the JHU table as a proxy for daily new cases, and aggregates deaths from the mortality table in a subquery before joining.

Why each piece matters:

- WHERE CASE_TYPE = 'Confirmed' — filters out death-type rows from the JHU table so only confirmed case counts are used, avoiding double-counting cases and deaths from the same table
- WHERE DIFFERENCE >= 0 — removes negative values that appear due to retroactive data corrections in the JHU dataset, which would distort the case growth trend
- LEFT JOIN with the subquery — pre-aggregates the mortality table by date before joining, which prevents row multiplication that would occur if you joined on the raw table directly
- COALESCE(..., 0) — handles dates where Belgium has case data but no corresponding mortality record, replacing NULL with 0 so the chart renders cleanly

Why this is non-trivial: This query requires a multi-table join across two datasets from different sources, a subquery to pre-aggregate deaths, geographic and case-type filtering, and data cleaning to handle negative difference values. Simply pulling from one table would not answer the question — the relationship between cases and deaths only emerges by combining both sources on a shared date key.

## Analysis and Results
Dashboard:
<img width="1214" height="426" alt="Screenshot 2026-04-26 at 2 51 05 PM" src="https://github.com/user-attachments/assets/e61d6ba6-0d44-4c32-a4c8-47e9b82e2c37" />
1. How did COVID-19 death tolls vary across age groups and regions in Belgium during the pandemic? (Chart on Left)
- This chart displays the monthly deaths from COVID-19 in Belgium, broken down by age group, spanning monthly from March 2020 to April 2023. Deaths are overwhelmingly intense among the older populations, with the 85+ group recording the highest mortality across all age groups, shortly followed by the 75-84 group, while those under the age of 64 remained closer to zero throughout the pandemic. The two most deadly periods were in Spring and Fall of 2020, and after which deaths fell substantially, which likely reflects Belgium’s rollout of the vaccination.

2. How did the growth rate of COVID-19 cases relate to changes in mortality across Belgium over time? (Chart on Right)
- This chart compares new COVID-19 cases against total deaths in Belgium from 2020 to 2023 on a monthly basis. The January to February 2022 Omicron wave led to Belgium’s largest surge in cases by far, exceeding over one million monthly cases, yet deaths would stay relatively low, which suggests immunity improvement and less severity of the pandemic. In contrast, the wave during Fall of 2020 shows a higher proportional relationship between cases and deaths, reflecting the higher fatality rate earlier in the pandemic, before vaccines were ready.

## Streamlit App
<img width="1812" height="1300" alt="image" src="https://github.com/user-attachments/assets/d3063d93-c1cb-44ce-8ea2-9af05710257f" />
<img width="1774" height="320" alt="image" src="https://github.com/user-attachments/assets/4c65fb37-8e67-41c6-aaa0-ca702ea73e09" />
<img width="1802" height="1294" alt="image" src="https://github.com/user-attachments/assets/ad744a6a-7d6e-4e3b-b0d0-2d9c7448a25d" />

Interactive Element:
<img width="574" height="416" alt="image" src="https://github.com/user-attachments/assets/3754fcbe-ab88-482b-8e32-0c1f724d0a80" />
- There is a multi-select filter featured on the siddebar that allows users to show data for Flanders, Wallonia, Brussels, or any combination of the three areas. This is analytically meaningful because the patterns of mortality significantly differed across Belgium's three regions, as Flanders recorded higher death totals consistently, due to its larger population and early care home outbreaks. Filtering these metrics by region lets users compare how each area was effected. 

AI Assistance:

- What was prompted: Claude was asked to build a Streamlit in Snowflake app that reproduced the two dashboard charts from Component 2, with a region multi-select filter as the interactive element.
  
- What Claude generated: The full Streamlit app structure including sidebar filter, SQL queries, chart configurations, and written interpretations. Initially used Plotly Express for charting

- What was changed/rejected:
1. The first version of the code used plotly.express, which threw a ModuleNotFoundError because Plotly is not pre-installed in Snowflake's Streamlit environment and there was no accessible Packages installer in the interface.
2. Claude replaced Plotly with Altair, which comes pre-installed in Snowflake's Streamlit runtime, requiring no additional package installation
3. The bar chart for Chart 2 was implemented using alt.Chart.mark_bar() with df.melt() to reshape the two metrics into a format Altair could plot side by side
4. All SQL queries, filter logic, and interpretations from the original generated code were kept as-is with no modifications needed

- What was accepted as-is
1. Region multi-select sidebar filter
2. Both SQL queries with dynamic region filtering
3. Chart titles, axis labels, and written interpretations
4. Overall app layout and structure
