<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Images/banner_new.png></img>

The Financial Data ETL (Extract, Transform, Load) Pipeline project aims to automate the process of collecting, organizing, and analyzing financial transaction data from different bank accounts. The project involves downloading CSV files from bank accounts, standardizing and cleaning the data, and then consolidating it into a master CSV file. This documentation provides a detailed overview of the project's purpose, components, and usage instructions.

## 1. Introduction
#### Project Goals
The primary goal of the Financial Data ETL Pipeline is to automate the process of collecting, standardizing, and consolidating financial transaction data from various bank accounts. This enables users to analyze their spending patterns, income sources, and other financial insights easily.
#### Data Sources
The project relies on CSV files downloaded manually from different bank accounts. The CSV files are organized into separate folders based on the bank or card account, such as ASB, ANZ, GEM, and Q Card.
#### Tools and Libraries
The project utilizes the following tools and libraries:

- Python: The scripting language used for the entire pipeline.
- pandas and numpy: Used for data manipulation and transformation.
- Fuzzy matching is performed using the thefuzz library.

## 2. Folder Structure
The project is organized into three main folders, each representing a stage in the data transformation pipeline.

- 1.Raw-Bronze: This folder holds the raw CSV files downloaded from different banks.
- 2.Processed-Silver: Processed data from the raw CSV files is saved here, maintaining consistent data schema.
- 3.Presentation-Gold: Cleaned and categorized data is exported to this folder for presentation and analysis.

Data flows through these layers: Raw → Processed → Presentation.

## 2. Project Components
#### Data Extraction
- The CSV files from banks such as ASB, ANZ, GEM credit card, and Q Credit card are stored in separate folders within the 1.Raw-Bronze directory.
- CSV files are organized by banks within the 1.Raw-Bronze folder. For instance, CSV files from ANZ are saved under ANZ sub-folder. The organization helps maintain data separation and clarity.
#### Data Transformation
Cleaning and Formatting
Python's Pandas library is used for data manipulation. The ANZ data extraction process is similar to other banks.
- CSV files are read into Pandas DataFrames.
- Columns are converted to lowercase for consistency.
- Columns related to transaction details are merged into a 'Description' column.
- Date values are converted to date data type and a formatted date column is added.
- Columns are reordered for better readability.
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/3.png width=1000></img>


#### Creating Master DataFrame

The master DataFrame final_df_processed includes the following columns: 'Date', 'Description', 'Amount', 'File'.
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/6.png width=1000></img>

#### Categorizing Transactions
Transactions are categorized based on their descriptions using fuzzy matching. A list of keywords is defined for common categories. For each keyword:

- Fuzzy matching is performed using the thefuzz library.
- If the matching score is 80 or above, the corresponding category is assigned to the 'Sub Category' column in the DataFrame using NumPy function.

<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/7.png width=1000></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/8.png width=1000></img>

#### Categorization Rules:

- Categories are assigned based on predefined keywords found in the 'Description' column.
- Various categories like 'Salary', 'Other Income', 'Rental', 'Utility Bills', etc., are defined with corresponding keyword patterns.
- The np.where function is used to categorize rows based on conditions defined by keyword patterns.

#### Categorization Hierarchies:

- A higher-level categorization called 'Category' is assigned based on 'Sub Category' assignments.
- Categories are grouped into broader groups like 'Income', 'Transport', 'Living Expenses', etc.
- Similar to the subcategory categorization, np.where is used for category assignments.

## 5. Data Presentation
#### Exporting to Presentation Data
The transformed and categorized data is exported to the 3.Presentation-Gold folder. The final_df_processed DataFrame is exported to a CSV file named Master_File.csv. This file contains cleaned, formatted, and categorized transaction data, ready for analysis and presentation.
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/14.png width=1000></img>
<br>
<img src=https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/Code%20Snippets/15.png width=1000></img>


## 6. Conclusion
This Python project demonstrates an effective approach to handling banking transaction data without direct access to bank APIs. The process involves downloading CSV files, transforming data using Pandas, and categorizing transactions based on descriptions. The well-structured folder organization ensures data separation and ease of management throughout the transformation pipeline. The resulting categorized data is presented in a master CSV file, facilitating further analysis and reporting. While manual CSV downloading is currently employed, the project's modular structure allows for seamless integration of bank APIs in the future.


[Click here to see the full project](https://github.com/sularaperera/Personal-Finance-Dashboard/blob/main/ETL/ETL_Bank_Trasactions.ipynb)
