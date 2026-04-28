# Climate Challenge Week 0

## Setup Instructions

1. Clone repository:
   git clone <repo-url>

2. Create virtual environment:
   python -m venv venv

3. Activate environment:
   venv\Scripts\activate  (Windows)

4. Install dependencies:
   pip install -r requirements.txt

## CI
GitHub Actions runs automatically on push.
Project initialized successfully.
Project setup complete.
Task2:Data Profiling, Cleaning & EDA
Objective: Profile, clean, and conduct a focused exploratory data analysis on the climate dataset to extract meaningful insights about African climate trends in the lead-up to COP32.

	Data Loading & Preprocessing
The dataset was loaded using pandas and prepared for analysis. A Country column was added to identify the dataset, and a proper datetime column was constructed by combining the YEAR and DOY (Day of Year) fields using:
pd.to_datetime(df["YEAR"] * 1000 + df["DOY"], format="%Y%j")
A Month column was then extracted from the datetime field to enable seasonal analysis.
This transformation allowed the dataset to be analyzed as a time series.

	Data Cleaning
	Handling Missing Values
All occurrences of -999 were replaced with NaN, as this value represents missing or invalid measurements in NASA datasets.
After replacement, no significant missing values were detected, indicating that the dataset is relatively complete and reliable for analysis.

	Duplicate Records
Duplicate rows were checked using:
df.duplicated().sum()
No significant duplicates were found, ensuring data integrity.
	Outlier Detection
Outliers were identified using the Z-score method across key climate variables:
	Temperature (T2M, T2M_MAX, T2M_MIN) 
	Precipitation (PRECTOTCORR) 
	Humidity (RH2M) 
	Wind speed (WS2M, WS2M_MAX) 
Rows with |Z| > 3 were flagged as potential outliers 
These outliers were retained in the dataset because:
	Climate data naturally includes extreme values 
	These values may represent real phenomena such as heatwaves or heavy rainfall events 
Removing them could distort the true climate patterns.
	Exploratory Data Analysis
   Temperature Trends
   Precipitation Patterns
   Correlation Analysis
	Distribution Analysis
	Multivariable Relationship (Bubble Chart)
	Conclusion
The dataset of five contries (Ethiopia, Kenya, Nigeria, Sudan and Tanzania) separtely  was successfully cleaned, validated, and analyzed.
The EDA revealed:
	Clear seasonal climate patterns 
	Meaningful relationships between key variables 
	Evidence of variability in precipitation and temperature 
Overall, the analysis provides valuable insights into Ethiopia’s, Kenya's, Nigeria's , Sudan's and Tanzaniea's climate system and highlights patterns that are relevant for environmental planning and policy discussions.
Task 3:Task 3: Cross-Country Comparison & Climate Vulnerability Ranking
#Step 1: Load & combine datasets
import pandas as pd

countries = {
    "Ethiopia": "../data/ethiopia_clean.csv",
    "Kenya": "../data/kenya_clean.csv",
    "Nigeria": "../data/nigeria_clean.csv",
    "Sudan": "../data/sudan_clean.csv",
    "Tanzania": "../data/tanzania_clean.csv"
}

dfs = []

for country, path in countries.items():
    df = pd.read_csv(path)
    df["Country"] = country
    dfs.append(df)

combined_df = pd.concat(dfs, ignore_index=True)

combined_df.head()
combined_df["Country"].value_counts()
#Check all countries:
combined_df["Country"].unique()
#Random sample:
combined_df.sample(10)
#Group preview:
combined_df.groupby("Country").head(2)
What this means for the Task 3

we are now in a very strong position:

✅ Data combined correctly
✅ Equal number of records
✅ Ready for fair comparison
#to check each country dzta  loading or not
dfs = []

for country, path in countries.items():
    print(f"Loading {country} from {path}")
    
    try:
        df = pd.read_csv(path)
        df["Country"] = country
        print(f"{country} loaded: {df.shape}")
        dfs.append(df)
    except Exception as e:
        print(f"Error loading {country}: {e}")

import matplotlib.pyplot as plt

combined_df["DATE"] = pd.to_datetime(combined_df["DATE"])

monthly_temp = combined_df.groupby(
    ["COUNTRY", pd.Grouper(key="DATE", freq="ME")]
)["T2M"].mean().reset_index()

plt.figure(figsize=(12,6))

for country in monthly_temp["COUNTRY"].unique():
    subset = monthly_temp[monthly_temp["COUNTRY"] == country]
    plt.plot(subset["DATE"], subset["T2M"], label=country)

plt.legend()
plt.title("Monthly Average Temperature (T2M)")
plt.xlabel("Year")
plt.ylabel("Temperature (°C)")
plt.grid(True)
plt.show()


Temperature Trend Insights

- All countries exhibit strong seasonal temperature cycles, indicating consistent climatic patterns over time.
- Sudan records the highest temperatures and the largest variability, suggesting increased exposure to extreme heat conditions.
- Ethiopia shows the lowest temperature range, likely influenced by its higher altitude, indicating relatively lower heat stress.
- Nigeria and Tanzania demonstrate moderate and stable temperature patterns, though slight upward trends suggest gradual warming.
- Overall, there is evidence of increasing temperature peaks in recent years, pointing toward a regional warming trend.

These findings highlight varying levels of climate exposure across countries, with Sudan appearing most vulnerable to heat-related climate risks.
# Cross-Country Climate Analysis & Vulnerability
temp_summary = combined_df.groupby("COUNTRY")["T2M"].agg(
    ["mean", "median", "std"]
)

temp_summary
Temperature Trend Insights

- All countries exhibit strong seasonal temperature cycles, indicating consistent climatic patterns over time.
- Sudan records the highest temperatures and the largest variability, suggesting increased exposure to extreme heat conditions.
- Ethiopia shows the lowest temperature range, likely influenced by its higher altitude, indicating relatively lower heat stress.
- Nigeria and Tanzania demonstrate moderate and stable temperature patterns, though slight upward trends suggest gradual warming.
- Overall, there is evidence of increasing temperature peaks in recent years, pointing toward a regional warming trend.

These findings highlight varying levels of climate exposure across countries, with Sudan appearing most vulnerable to heat-related climate risks.
## Precipitation Variability Insights
import seaborn as sns
import matplotlib.pyplot as plt

sns.boxplot(data=combined_df, x="COUNTRY", y="PRECTOTCORR")

plt.title("Precipitation Distribution by Country")
plt.xlabel("Country")
plt.ylabel("Precipitation (mm)")
plt.show()

All countries exhibit highly skewed precipitation distributions, with most days receiving little to no rainfall and occasional extreme events.
- Nigeria and Tanzania show the greatest variability in precipitation, indicating unstable and unpredictable rainfall patterns.
- Nigeria records the most extreme rainfall events, suggesting a high risk of flooding.
- Sudan and Kenya have the lowest median precipitation levels, indicating persistent dry conditions and higher drought vulnerability.
- Ethiopia demonstrates moderate precipitation levels and variability, suggesting exposure to both drought and occasional heavy rainfall events.

These patterns highlight significant differences in climate risk profiles across countries, particularly between drought-prone and flood-prone regions.