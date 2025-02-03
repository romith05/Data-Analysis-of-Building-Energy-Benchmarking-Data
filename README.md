# Data-Analysis-of-Building-Energy-Benchmarking-Data
This README.md documents the methodology, challenges encountered, and insights gained from a building energy analysis. The analysis focused on exploring energy consumption patterns, identifying factors influencing energy use, and uncovering potential areas for improvement.

**Methodology**

**Data Acquisition and Cleaning:**
The Python library pandas was used to load and manipulate the data from a CSV file.
Regular expressions (Regex) were employed to clean specific text-based data columns:
	
**Property GFA:** </br> Extracted numerical values from strings using ```re.findall(r'\d+', x)[0]```.</br>
**Postal Code:** </br>Standardized the format using ```re.sub(r'(\w\d\w)[-\s]*(\d\w\d)', r'\1 \2', x)```.</br>
**Property Name:** </br> Removed special characters using ```re.sub(r'[^a-zA-Z0-9 ]', '', x)```.</br>
**Missing data was handled by:**
Dropping columns with more than 40% missing values using 
		```df.dropna(thresh=0.4 * len(df), axis=1)```.</br>
Imputing numerical columns with the median value using 
		```df[col].fillna(df[col].median(), inplace=True)```.</br>
Imputing object columns with the mode (most frequent value) using 
		```df[col].fillna(df[col].mode()[0], inplace=True)```.</br>
	
**Regular expressions (Regex)** were employed to clean specific text-based data columns.</br>
**Extracting Numerical Values from Columns:**</br>
Regex pattern r'\d+' is used to find all occurrences of digits within the specified columns (Property GFA - Self-Reported (m²), Total GHG Emissions (Metric Tons CO2e), Source Energy Use (GJ)).</br>
The first occurrence of digits is extracted and converted to a float using ```float(re.findall(r'\d+', x)[0])```.</br>
If no digits are found, np.nan is assigned to the value.</br>
**Cleaning Property Names:**</br>
The ```re.sub(r'[^a-zA-Z0-9 ]', '', x)``` pattern removes any character that is not a letter, number, or space from the Property Name column, resulting in a cleaner and more uniform property name.</br>
**Standardizing Postal Codes:** </br>
The ```re.sub(r'(\w\d\w)[-\s]*(\d\w\d)', r'\1 \2', x)``` pattern standardizes the postal code format by removing any hyphens or spaces between the first three characters and the last three characters.
df['Postal Code'] = df['Postal Code'].str.upper() converts the postal codes to uppercase for consistency.</br>
**2. Data Cleaning Functions**</br>
**clean_property_name(name):**</br>
Removes special characters and extra spaces using the ```re.sub(r'[^\w\s]', '', name)``` pattern.
Converts the property name to title case by capitalizing the first letter of each word.</br>
**clean_address(address):**
Removes special characters using ```re.sub(r'[^\w\s\.,-]', '', address)```.</br>
Standardizes street number format by removing extra spaces between the number and the street name using ```re.sub(r'(\d+)\s+(\w+)', r'\1 \2', cleaned_address)```.</br>
Standardizes street type abbreviations by replacing variations of "St", "Ave", "Rd", etc., with their standard abbreviations using multiple ```re.sub()``` operations.</br>
**Exploratory Data Analysis (EDA) and Aggregations:**
Descriptive statistics were calculated using df.describe() to summarize the data's central tendency, spread, and distribution.
Aggregations were performed to:</br>
Calculate average Energy Use Intensity (EUI) by property type using ```df.groupby('Property Type')['Site EUI (GJ/m²)'].mean()```.</br>
Find the total GHG emissions per year using```df.groupby('Year')['Total GHG Emissions (Metric Tons CO2e)'].sum()```.</br>
Identify the top 5 energy-consuming buildings using``` df.nlargest(5, 'Total Energy Consumption (GJ)')```.</br>
Outliers in the Total GHG Emissions column were detected using the Interquartile Range (IQR) and replaced with the median value.</br>
**Data Visualization:**
Time-series plots were created using matplotlib.pyplot to visualize trends in GHG emissions over time.
Comparative bar charts were generated using plt.barh to showcase the top 10 buildings with the highest GHG emissions.
Heatmaps were created using seaborn.heatmap to depict the variations in EUI across property types and years.
**Further Analysis:**</br>
Correlation analysis was conducted using ```df[['Total Energy Consumption (GJ)', 'Total GHG Emissions (Metric Tons CO2e)', 'Property GFA']].corr()``` </br>to quantify the relationships between energy consumption, GHG emissions, and property floor area.
Hypothesis testing using ```scipy.stats.ttest_ind``` was performed to compare the Energy Star Scores of office and residential buildings, revealing potential differences in energy efficiency between these property types.</br>
**Challenges Faced**</br>
**Data Cleaning:** Extracting numerical values from inconsistent text formats in the Property GFA column required careful application of Regex patterns.</br>
**Missing Data:** Handling missing values involved a combination of dropping columns with excessive missingness, imputation with median/mode values, and addressing outliers in the GHG emissions data.</br>
**Error Correction:** The code snippet you shared contained an error where the x-axis (years) and y-axis (EUI values) had different lengths. This would have resulted in a ValueError. To rectify this, ensure your data has the same number of elements for both axes. You can achieve this by:</br>
	Verifying the data source and correcting any inconsistencies.</br>
	Removing extra values or duplicating missing values to match the lengths.</br>
 **Insights Gained**
The analysis revealed variations in energy consumption and GHG emissions across different property types. Recreation facilities, for instance, appeared to have higher energy demands.
Time-series plots provided insights into potential trends in energy use over the years.
The correlation matrix indicated a positive correlation between total energy consumption and property floor area, suggesting that larger buildings tend to consume more energy.
Hypothesis testing could be used to investigate and potentially confirm significant differences in energy efficiency between property types based on Energy Star Scores.
