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

- The dataset contains 1 table with approximately 11,413 rows. Key columns include DATE (string, YYYY-MM-DD format) serving as the join key for time, REGION (string) as the geographic identifier mapping to country/state, AGEGROUP (string) enabling demographic breakdowns, SEX (string) for optional deeper breakdowns, and DEATHS (integer) acting as the numerator in mortality rate calculations.
- JHU_COVID_19 has 9738292 rows and key columns include DATE (string, YYYY-MM-DD format) serving as the join key for time, COUNTRY_REGION (string) as the geographic identifier, CASE_TYPE (string) indicating the classification of cases such as confirmed or deaths, and CASES (integer) representing the count of cases.


## Questions and Justification

## Data Manipulations

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
