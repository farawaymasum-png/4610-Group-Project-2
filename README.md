# 4610-Group-Project-2

Team Members: 

Evelyn Merchant: @evelynjmerch
Divesh Gupta: @legendile7


## Dataset Description
## Questions and Justification
1. How did the surge in daily new cases correlate with the demand for ICU beds and ventilators across U.S. states between October 2020 and March 2021, and was there a specific 'tipping point' where a jump in new cases shows that an uptake in ventilator use was coming?

Justification:
This examines the critical "lag" between community viral spread and the resulting strain on hospital infrastructure. By identifying a predictive relationship between case counts and ventilator demand, it allows healthcare administrators to forecast resource needs (such as specialized staffing and respiratory equipment) roughly 10–14 days in advance, rather than reacting to a crisis as it happens. It highlights the periods where healthcare systems reached their breaking points, illustrating the human cost of the pandemic when life-saving resources were most scarce.

This is a non-trivial question because it requires a complex join between two distinct data providers—the New York Times and The COVID Tracking Project. In addition, it tracks the surge over time across many different variables.

Tables and Columns:

NYT_US_COVID19 
   DATE: This was used to create the timeline portion.
  STATE: To ensure geographical alignment with the join. Can also filter by state.
  CASES_SINCE_PREV_DAY: to track daily surge in new infections.
CT_US_COVID_TESTS
  PROVINCE_STATE: Used as the join key with the NYT table.
  DATE: Used as the join key to align daily reports.
  INICUCURRENTLY: To track the daily count of patients in intensive care.
  ONVENTILATORCURRENTLY: To measure the peak demand for critical respiratory support.

2. 
Which countries had the highest COVID-19 death rates (deaths as a % of confirmed cases), and how do the top 10 most-affected countries compare in total cases vs. total deaths?

Justification:
This question is meaningful because raw death counts alone are misleading. A country with 700,000 deaths but 70 million cases is actually managing the outbreak better than a country with 170,000 deaths out of only 2.5 million cases. By combining total deaths with the case fatality rate (CFR), the question forces a two-dimensional view of pandemic severity — volume of outbreak vs. deadliness of outcome — which is exactly the kind of comparison public health officials and policymakers need to allocate resources, evaluate healthcare system performance, and compare national responses.
It is also analytically nontrivial because it requires pivoting two different case types from the same column into side-by-side metrics and computing a derived ratio, rather than simply reading a number directly from the table.

Tables and Columns

JHU_COVID_19	
	COUNTRY_REGION: The geographical identifier for each country.
	CASE_TYPE: describes the case situation as “Confirmed”, “ Death”, or “Recovered”
	CASES: The numeric count column — the actual number of cases for a given country, date, and case type. Used to calculate TOTAL_CONFIRMED,       TOTAL_DEATHS, and DEATH_RATE_PCT.

## Data Manipulations
## Analysis and Results
1.
<img width="1211" height="386" alt="Screenshot 2026-04-26 at 5 03 02 PM" src="https://github.com/user-attachments/assets/580542bf-331a-491c-8945-b3569930fc70" />

<img width="1193" height="376" alt="Screenshot 2026-04-26 at 5 03 49 PM" src="https://github.com/user-attachments/assets/ef999c4e-c695-4083-ab62-540ccd9e16cf" />

  Our dashboard utilizes a multi-chart approach to distinguish between community transmission 'flow' and hospital resource 'inventory.' This separation is critical for answering our analytical question, as it highlights a significant correlation between new cases and hospital resources and lag that may follow. 
We discovered that while both metrics reached a statistical peak on January 4, 2021 (the tipping point), the data reveals a fundamental difference in their operational behavior. The 'New Cases' peak on Jan 4 was a sharp, isolated event—likely exacerbated by holiday reporting delays—followed by an immediate and rapid decline. Conversely, the demand for Ventilators and ICU beds on that same date represented the culmination of a two-month build-up. On the Ventilator Patients chart, after Jan 4, there is a slight declining slope following the point, illustrating the remaining strain caused by the new cases and shows the lingering effect or lag that it has caused. 
To conclude, this confirms that while the worst day for testing may have been Jan 4, the worst period for hospital resource strain persisted much longer continuing into March, proving that critical care demand is a lagging, more persistent indicator of a healthcare crisis.

2. 

<img width="753" height="416" alt="Screenshot 2026-04-26 at 10 17 02 PM" src="https://github.com/user-attachments/assets/eb7721a7-1695-4e4e-a778-7f44f007519f" />


<img width="807" height="438" alt="Screenshot 2026-04-26 at 10 12 59 PM" src="https://github.com/user-attachments/assets/d2d367ae-3607-49d0-895e-d116b41fbad1" />

Chart 1 — Top 10 Countries by Total COVID-19 Deaths

The United Kingdom leads this chart with approximately 186,138 total deaths, followed closely by Brazil at 179,039 and France at 161,512. We noticed that the rankings are notably closer together than expected, because the gap between the UK at the top and South Africa at the bottom is only about 83,000 deaths, suggesting these countries experienced broadly similar death burdens. An important note is that the US is absent from this chart because JHU stores American data at the state level under the country code 'US' rather than as a unified national row, which affects how the aggregation groups country-level totals.

Chart 2 — Top 10 Countries by COVID-19 Case Fatality Rate (%)

Peru dramatically leads the fatality rate chart at 5.48%, meaning roughly 1 in every 18 confirmed COVID cases in Peru resulted in death. Brazil follows at 2.77%, and South Africa at 2.52%. The contrast with Chart 1 is striking because for example, France, which ranked 3rd in total deaths, does not appear on the CFR chart at all. Its 38.6 million confirmed cases produced a CFR of only 0.42%, which is one of the lowest rates in the dataset. On the other hand, Peru ranked 9th in total deaths but 1st in fatality rate, because its confirmed case count of only 2.1 million was far smaller relative to its death toll.

Together these two charts expose a fundamental tension in pandemic measurement: the countries that died the most are not the same as the countries where infection was most deadly. France had over 161,000 deaths but had such widespread confirmed case detection that its fatality rate was negligible by comparison. Peru, on the other hand, had far fewer recorded cases, yet its death toll relative to those cases was the highest of any country in the top 10. This shows that Peru was a strong signal of either overwhelmed healthcare infrastructure, significant undercounting of confirmed cases during the outbreak, or both. Brazil is the only country that ranks highly on both charts, appearing 2nd in total deaths and 2nd in CFR at 2.77%, making it the clearest example of a country that experienced both high outbreak volume and high outbreak severity simultaneously.

## Streamlit App
Our Streamlit dashboard takes the Snowflake queries and wraps them in a fully interactive, custom-styled "Dark Mode" web application.

**Interactive Element 1: U.S. State Multi-Select Filter**
* **Functionality:** Users can select one or multiple U.S. states from a dropdown menu to dynamically update the dual-axis line chart tracking the Winter 2020-2021 surge.
* **Analytical Value:** The COVID-19 winter surge did not hit all states simultaneously or with the same severity. By allowing users to filter by specific states, the dashboard enables comparative analysis of regional hospital capacity and localized outbreak timelines, rather than just viewing a blurred national average.

**Interactive Element 2: Minimum Confirmed Cases Threshold Slider**
* **Functionality:** Users can adjust a numerical input to set a floor for the minimum number of confirmed cases a country must have to be included in the Global Case Fatality calculation.
* **Analytical Value:** Death rates (fatalities / cases) can be highly skewed by statistical anomalies in countries with tiny populations or limited reporting (e.g., a country with 10 cases and 2 deaths having a 20% fatality rate). By giving the user control over the case threshold, they can filter out statistical noise and focus the analysis on nations experiencing statistically significant, widespread outbreaks.

**AI Assistance Documentation:**
We utilized AI (Gemini) to assist with the technical execution of our dashboard in the following ways:
1. **Python Translation:** Translated our working Snowflake SQL queries into Python syntax.
2. **Advanced Visualization (Dual-Axis):** Overcame a limitation of standard Snowsight visualizations. Because the volume of new cases vastly outnumbered available ventilators, plotting them on the same axis flattened the hospital data. We prompted the AI to use the `altair` library to generate a dual-axis chart (`resolve_scale(y='independent')`).

<img width="1795" height="928" alt="image" src="https://github.com/user-attachments/assets/90e0c39e-9b02-40d5-ad61-d79874156a6e" />
<img width="1765" height="947" alt="image" src="https://github.com/user-attachments/assets/272982c6-5bb6-4579-a855-f819998bcd66" />


