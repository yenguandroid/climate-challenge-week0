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

