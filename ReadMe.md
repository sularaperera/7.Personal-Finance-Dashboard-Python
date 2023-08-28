<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Images/banner.png></img>

This Python project that involves processing financial transaction data to categorize expenses and incomes automatically. The project aims to organize transaction data from various CSV files, categorize them into predefined categories, and generate a master file with the categorized data for further analysis and reporting.

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

### Categorization Rules:

- Categories are assigned based on predefined keywords found in the 'Description' column.
- Various categories like 'Salary', 'Other Income', 'Rental', 'Utility Bills', etc., are defined with corresponding keyword patterns.
- The np.where function is used to categorize rows based on conditions defined by keyword patterns.

### Categorization Hierarchies:

- A higher-level categorization called 'Category' is assigned based on 'Sub Category' assignments.
- Categories are grouped into broader groups like 'Earnings', 'Transport', 'Living Expenses', etc.
- Similar to the subcategory categorization, np.where is used for category assignments.

### Exporting Processed Data:

- The processed DataFrame is saved as a CSV file in a designated folder using the to_csv method.

### Documentation:

- A brief summary of the project, its objectives, and a detailed explanation of each code section is provided. This documentation aims to help others understand and replicate the project's functionality.
<br>
Import necessary libraries
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/1.png width=800></img>
<br>
Define the path to the ANZ raw data folder and use rglob to recursively find all CSV files in the specified folder
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/2.png width=800></img>

- Iterates through each CSV file in the specified folder
- Reads the CSV file and selects specific columns ('Details', 'Particulars', 'Code', 'Reference', 'Amount', 'Date')
- Adds a new column 'File' containing the name of the processed file |Converts certain columns ('Details', 'Particulars', 'Code', 'Reference') to lowercase
- Merges the selected columns into a new column called 'Description', joining non-empty values with a space.
- Converts the 'Date' column from a string format to a datetime object, and then to a formatted string for presentation purposes.
- Reorders the columns.
- Appends each processed dataframe to a list called 'df_list_anz'.
- Combines all dataframes in 'df_list_anz' into a single dataframe named 'final_df_anz'.
- Displays the first few rows of the combined dataframe.
- Overall, the code reads, processes, and combines CSV data into a structured format for analysis or further processing.
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/3.png width=1000></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/4.png width=800></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/5.png width=800></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/6.png width=800></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/7.png width=800></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/8.png width=800></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/9.png width=800></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/10.png width=800></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/11.png width=800></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/12.png width=800></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/13.png width=800></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/14.png width=800></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/15.png width=800></img>

### Conclusion:
This project demonstrates an automated method for categorizing financial transactions using predefined keyword patterns. It processes CSV files, extracts relevant data, applies categorization rules, and generates a master file with categorized information. The code can be adapted for various financial tracking and analysis purposes.


[Click here to see the code](https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/ETL/ETL_Bank_Trasactions.ipynb)
