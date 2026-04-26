# MIST-4610-Group-7---Project-2

## Team Name:
61608 - Group 7
## Team Members:
1. Jamie Kim - [@Jamie-S-Kim](https://github.com/Jamie-S-Kim)
2. Sienna Wong - [@s1ennawong](https://github.com/s1ennawong)
3. Shaehaan Khaja - [@shaehaankhaja](https://github.com/shaehaankhaja)
4. Seth White - [@sethwhite444](https://github.com/sethwhite444)
5. Owen Verlander - [@owenver](https://github.com/owenver)

## Dataset Description

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
- 
