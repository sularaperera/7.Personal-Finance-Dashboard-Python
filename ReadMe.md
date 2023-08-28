<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Images/banner.png></img>

This Python code snippet demonstrates a project that involves processing financial transaction data to categorize expenses and incomes automatically. The project aims to organize transaction data from various CSV files, categorize them into predefined categories, and generate a master file with the categorized data for further analysis and reporting.

## Project Steps:

### Import Required Libraries:

-  The project starts by importing necessary libraries such as pandas, numpy, Path from pathlib, re for regular expressions, and datetime for date manipulation.

### Defining Input Paths:

- The path to the directory containing raw transaction data files is defined using the Path class.
- The code lists all CSV files in the specified directory and its subdirectories using the rglob function.

### Reading and Processing Data:

- A list named df_list_anz is created to store individual DataFrames after processing.
- For each CSV file found in the specified directory:
  - The relevant columns ('Date', 'Details', 'Particulars', 'Code', 'Reference', 'Amount') are extracted and reordered.
  - A 'File' column is added to store the file name.
  - Text columns are converted to lowercase for consistency.
  - Columns are merged to create a 'Description' column.
  - The 'Date' column is converted to a datetime object.
  - A 'Formatted_Date' column is added for presentation purposes.
  - The DataFrame is appended to the df_list_anz.

### Concatenating DataFrames:

- All individual DataFrames in df_list_anz are concatenated into a single DataFrame called final_df_anz.

### Data Cleaning and Transformation:

- In the 'final_df_anz':
  - Missing values are filled with 'N/A'.
  - The 'Date' column is converted to datetime format.
  - A new column 'Sub Category' is initialized with 'Unassigned'.

- Categorization Rules:

Categories are assigned based on predefined keywords found in the 'Description' column.
Various categories like 'Salary', 'Other Income', 'Rental', 'Utility Bills', etc., are defined with corresponding keyword patterns.
The np.where function is used to categorize rows based on conditions defined by keyword patterns.
Categorization Hierarchies:

A higher-level categorization called 'Category' is assigned based on 'Sub Category' assignments.
Categories are grouped into broader groups like 'Earnings', 'Transport', 'Living Expenses', etc.
Similar to the subcategory categorization, np.where is used for category assignments.
Exporting Processed Data:

The processed DataFrame is saved as a CSV file in a designated folder using the to_csv method.
Documentation:

A brief summary of the project, its objectives, and a detailed explanation of each code section is provided. This documentation aims to help others understand and replicate the project's functionality.
