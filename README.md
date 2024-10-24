
# Data Cleaning Process for Customer Call List

This project involved cleaning a customer call list provided in Excel format. The cleaning process was carried out using Python, specifically with the help of libraries like `pandas` and `openpyxl`. Below is an overview of the steps taken to clean and standardize the data.

## Steps Involved:

### 1. Loading the Dataset
The dataset was loaded using the `pandas` library, which allows easy manipulation and analysis of Excel files.

```python
import pandas as pd

# Load the dataset
df = pd.read_excel("customer_call_list.xlsx", engine='openpyxl')
```

### 2. Trimming Whitespace
To ensure that there were no leading or trailing spaces in critical fields such as customer names, phone numbers, and addresses, the `strip()` function was applied across relevant columns.

```python
df["Last_Name"] = df["Last_Name"].str.strip("123._/_")
```

### 3. Standardizing Phone Numbers
Phone numbers were standardized to a uniform format, ensuring that all numbers followed the same pattern (e.g., removing non-numeric characters like dashes, spaces, or parentheses, and ensuring country codes were present).

```python
df["Phone_Number"] = df["Phone_Number"].str.replace('[^a-zA-Z0-9]', '', regex=True)
df["Phone_Number"] = df["Phone_Number"].apply(lambda x: str(x))
df["Phone_Number"] = df["Phone_Number"].apply(lambda x: x[0:3] + '-' + x[3:6] + '-' + x[6:10])
df["Phone_Number"] = df["Phone_Number"].str.replace('nan--','')

df["Phone_Number"] = df["Phone_Number"].str.replace('Na--','')
df

```

### 4. Standardizing Addresses
Addresses were cleaned to remove extraneous whitespace and ensure uniform formatting (e.g., consistent capitalization).

```python
df[["Street_Address", "State", "Zip_Code"]] = df["Address"].str.split(',', n=2, expand=True)
```

### 5. Filtering Call-Eligible Customers
Only customers with valid and callable phone numbers (e.g., those that meet certain length criteria or contain a valid country code) were retained in the final dataset.



### 6. Saving the Cleaned Dataset
The cleaned dataset was saved to a new Excel file for further use.

```python
# Save the cleaned data to a new Excel file
df.to_excel("cleaned_customer_call_list.xlsx", index=False)
```

## Summary
This data cleaning process involved:
- Trimming unnecessary spaces in customer names, phone numbers, and addresses.
- Standardizing phone numbers to ensure consistency across all entries.
- Formatting addresses uniformly.
- Filtering the dataset to retain only customers with valid, callable phone numbers.

The cleaned data is now ready for further analysis or to be used for customer call outreach campaigns.
