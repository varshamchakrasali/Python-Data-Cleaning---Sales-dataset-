Task 1: Data Cleaning and Preprocessing
 Objective: Clean and prepare a raw dataset (with nulls, duplicates, inconsistent formats).
 Tools: Excel / Python (Pandas)

# Python-Data-Cleaning---Sales-dataset-

1.	Removed Duplicates
	•	Identical rows were removed to avoid counting the same sales records multiple times.

2.	Dropped Rows with Critical Nulls
	•	Any rows missing essential fields like CUSTOMERNAME or ORDERNUMBER were removed, since they are key identifiers for sales.

3.	Filled Missing Values
	•	STATUS: Filled with 'unknown' if missing.
	•	QTR_ID: Filled with the most frequent quarter ID.
	•	SALES: Filled with the average sales value.

4.	Standardized Text Format
	•	All text fields were:
	•	Trimmed of extra spaces
	•	Converted to lowercase (for consistency)

5.	Converted Dates
	•	ORDERDATE column was converted into proper datetime format.
	•	Invalid or malformed dates were set as NaT (missing date).

6.	Formatted Country Names
	•	COUNTRY column values were converted to Title Case (e.g., “usa” → “Usa”, “canada” → “Canada”).

7.	Exported Final Dataset
	•	The cleaned data was saved as cleaned_sales_data.csv, ready for analysis or visualization.



Source code: 

import pandas as pd

# Load the dataset (adjust encoding if needed)
df = pd.read_csv("sales_data_sample.csv", encoding='latin1')

# 1. Remove duplicate rows
df = df.drop_duplicates()

# 2. Handle missing values
# Drop rows missing critical fields (e.g., Customer Name, Order Number)
df = df.dropna(subset=["CUSTOMERNAME", "ORDERNUMBER"])

# Optional: Fill remaining missing values with placeholder or mean
df = df.fillna({
   'STATUS': 'Unknown',
   'QTR_ID': df['QTR_ID'].mode()[0],
   'SALES': df['SALES'].mean()
})

# 3. Standardize text: trim and lower-case strings
for col in df.select_dtypes(include='object'):
   df[col] = df[col].str.strip().str.lower()

# 4. Convert date columns (if any)
if 'ORDERDATE' in df.columns:
   df['ORDERDATE'] = pd.to_datetime(df['ORDERDATE'], errors='coerce')

# 5. Standardize country or product line if needed
if 'COUNTRY' in df.columns:
   df['COUNTRY'] = df['COUNTRY'].str.title()

# 6. Export cleaned dataset
df.to_csv("cleaned_sales_data.csv", index=False)

print("Cleaning complete. Cleaned dataset saved as 'cleaned_sales_data.csv'.")

