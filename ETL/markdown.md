# %% [markdown]
# # import packages

# %%
import pandas as pd
import numpy as np
from pathlib import Path
import glob, os, re
from datetime import datetime

# %% [markdown]
# # extract and transform ANZ data

# %%
folder_path_anz_raw = Path(r'../Data/1.Raw-Bronze/ANZ/')

# %%
list(folder_path_anz_raw.rglob('*.csv'))

# %%
df_list_anz = []
for file in folder_path_anz_raw.rglob('*.csv'):
    df_anz = pd.read_csv(file, usecols=['Details','Particulars','Code','Reference','Amount','Date'])[['Date','Details','Particulars','Code','Reference','Amount']]

    # adding a new 'File' column to df with file name
    df_anz['File'] = file.stem
    
    # convert to lower case
    columns_to_convert = ['Details', 'Particulars', 'Code', 'Reference']
    df_anz[columns_to_convert] = df_anz[columns_to_convert].applymap(lambda x: x.lower() if isinstance(x, str) else x)

    # merge columns into a new column
    columns_to_merge = ['Details', 'Particulars', 'Code', 'Reference']
    df_anz['Description'] = df_anz[columns_to_merge].apply(lambda row: ' '.join(row.dropna().astype(str)), axis=1)

    # convert Date type object to date datatype
    # - step 1 - convert series of dates in original 'Date' column from string to datetime object (this is needed for date calculations)
    df_anz['Date'] = df_anz['Date'].apply(lambda x:datetime.strptime(x, "%d/%m/%Y"))
    # - step 2 - re-convert 'Date' datetime column values from datetime object to string object just for presentation purposes - here you can specify any format you like
    df_anz['Formatted Date'] = df_anz['Date'].apply(lambda x: x.strftime('%d-%m-%Y-%A'))
    
    # reorder columns
    df_anz = df_anz[['Date','Formatted Date','Description','Details', 'Particulars','Code','Reference','Amount','File']]

    # append each dataframe to a list
    df_list_anz.append(df_anz)
   
#df_list

final_df_anz = pd.concat(df_list_anz)
final_df_anz.head()

# %%
final_df_anz.dtypes

# %% [markdown]
# ### load ANZ data to "Processed-Silver" folder

# %%
folder_path_anz_procs = Path(r'../Data/2.Processed-Silver/')
file_anz_processed = folder_path_anz_procs / "ANZ_Transactions.csv"
final_df_anz.to_csv(file_anz_processed, index=False)

# %% [markdown]
# # extract and transform ASB data

# %%
folder_path_asb_raw = Path(r'../Data/1.Raw-Bronze/ASB/')

# %%
list(folder_path_asb_raw.rglob('*.csv'))

# %%
df_list_asb = []
for file in folder_path_asb_raw.rglob('*.csv'):
    df_asb = pd.read_csv(file, usecols=['Date','Payee','Memo','Amount'], skiprows=6)[['Date','Payee','Memo','Amount']]
    # adding a new 'File' column to df with file name
    df_asb['File'] = file.stem
    
    # convert to lower case
    columns_to_convert = ['Payee', 'Memo']
    df_asb[columns_to_convert] = df_asb[columns_to_convert].applymap(lambda x: x.lower() if isinstance(x, str) else x)

    # merge certain columns into a new column
    columns_to_merge = ['Payee', 'Memo']
    df_asb['Description'] = df_asb[columns_to_merge].apply(lambda row: ' '.join(row.dropna().astype(str)), axis=1)

    # convert Date type object to date datatype
    # - step 1 - convert series of dates in original 'Date' column from string to datetime object (this is needed for date calculations)
    df_asb['Date'] = df_asb['Date'].apply(lambda x:datetime.strptime(x, "%Y/%m/%d"))
    # - step 2 - re-convert 'Date' datetime column values from datetime object to string object just for presentation purposes - here you can specify any format you like
    df_asb['Formatted Date'] = df_asb['Date'].apply(lambda x: x.strftime('%d-%m-%Y-%A'))
    
    # reorder columns
    df_asb = df_asb[['Date','Formatted Date','Description','Payee','Memo','Amount','File']]
    
    # append each dataframe to a list
    df_list_asb.append(df_asb)
   
#df_list

final_df_asb = pd.concat(df_list_asb)
final_df_asb.head()

# %%
final_df_asb.dtypes

# %% [markdown]
# ### load ASB data to "Processed-Silver" folder

# %%
folder_path_asb_procs = Path(r'../Data/2.Processed-Silver/')
file_asb_processed = folder_path_asb_procs / "ASB_Transactions.csv"
final_df_asb.to_csv(file_asb_processed, index=False)

# %% [markdown]
# # extract and transform QCard data

# %%
folder_path_Qcard = Path(r'../Data/1.Raw-Bronze/QCard/')

# %%
list(folder_path_Qcard.rglob('*.csv'))

# %%
df_list_Qcard = []
for file in folder_path_Qcard.rglob('*.csv'):
    df_Q = pd.read_csv(file, usecols=['Date','Description','Amount'])[['Date','Description','Amount']]
    
    # adding a new 'File' column to df with file name
    df_Q['File'] = file.stem

    # convert to lower case
    columns_to_convert = ['Description']
    df_Q[columns_to_convert] = df_Q[columns_to_convert].applymap(lambda x: x.lower() if isinstance(x, str) else x)

    # Convert 'Amount' column to float - had to replace $ and , with empty char
    df_Q['Amount'] = df_Q['Amount'].apply(lambda x: float(x.replace('$', '').replace(',', '')))

    # convert Date type object to date datatype
    # - step 1 - convert series of dates in original 'Date' column from string to datetime object (this is needed for date calculations)
    df_Q['Date'] = pd.to_datetime(df_Q['Date'], dayfirst=True)
    # - step 2 - re-convert 'Date' datetime column values from datetime object to string object just for presentation purposes - here you can specify any format you like
    df_Q['Formatted Date'] = df_Q['Date'].apply(lambda x: x.strftime('%d-%m-%Y-%A'))
    
    # reorder columns
    df_Q = df_Q[['Date','Formatted Date','Description','Amount','File']]
    
    # append each dataframe to a list
    df_list_Qcard.append(df_Q)
   
#df_list

final_df_Q = pd.concat(df_list_Qcard)
final_df_Q.head()

# %%
final_df_Q.dtypes

# %% [markdown]
# ### load QCard data to "Processed-Silver" folder

# %%
folder_path_Qcard_procs = Path(r'../Data/2.Processed-Silver/')
file_Qcard_processed = folder_path_Qcard_procs / "QCard_Transactions.csv"
final_df_Q.to_csv(file_Qcard_processed, index=False)

# %% [markdown]
# # extract and transform ASB CC data

# %%
folder_path_asb_cc = Path(r'../Data/1.Raw-Bronze/ASBCC/')

# %%
list(folder_path_asb_cc.rglob('*.csv'))

# %%
df_list_asb_cc = []
for file in folder_path_asb_cc.rglob('*.csv'):
    df_asb_cc = pd.read_csv(file, usecols=['Date of Transaction','Description','Amount'])
    df_asb_cc = df_asb_cc.rename(columns={'Date of Transaction':'Date'})
    
    # adding a new 'File' column to df with file name
    df_asb_cc = df_asb_cc.dropna()
    df_asb_cc['File'] = file.stem

    # convert to lower case
    columns_to_convert = ['Description']
    df_asb_cc[columns_to_convert] = df_asb_cc[columns_to_convert].applymap(lambda x: x.lower() if isinstance(x, str) else x)

    # Convert 'Amount' column to float
    df_asb_cc['Amount'] = df_asb_cc['Amount'].apply(lambda x: float(x))

    # CC Amount column has different format than other files - so here I'm subtracting each value by 0 to swap the - & + values
    df_asb_cc['Amount'] = df_asb_cc['Amount'].apply(lambda x: (0-x))


    # convert Date type object to date datatype
    # - step 1 - convert series of dates in original 'Date' column from string to datetime object (this is needed for date calculations)
    df_asb_cc['Date'] = pd.to_datetime(df_asb_cc['Date'], dayfirst=True)
    # - step 2 - re-convert 'Date' datetime column values from datetime object to string object just for presentation purposes - here you can specify any format you like
    # df_asb_cc['Formatted_Date'] = df_asb_cc['Date'].apply(lambda x: x.strftime('%d-%m-%Y-%A'))
    
    # reorder columns
    df_asb_cc = df_asb_cc[['Date','Description','Amount','File']]
    
    # append each dataframe to a list
    df_list_asb_cc.append(df_asb_cc)
   
#df_list

final_asb_cc = pd.concat(df_list_asb_cc)
final_asb_cc.head()

# %%
final_asb_cc.dtypes

# %% [markdown]
# ### load ASB CC data to "Processed-Silver" folder

# %%
folder_path_asb_cc_procs = Path(r'../Data/2.Processed-Silver/')
file_asb_cc_processed = folder_path_asb_cc_procs / "ASB_CC_Transactions.csv"
final_asb_cc.to_csv(file_asb_cc_processed, index=False)

# %% [markdown]
# # append all files in processed folder

# %%
folder_path_processed = Path(r'../Data/2.Processed-Silver/')

# %%
list(folder_path_processed.rglob('*.csv'))

# %%
df_list_processed = []
for file in folder_path_processed.rglob('*.csv'):
    df_processed = pd.read_csv(file, usecols=['Date','Description','Amount','File'])[['Date','Description','Amount','File']]
        
    # append each dataframe to a list
    df_list_processed.append(df_processed)
   
final_df_processed = pd.concat(df_list_processed)

# fill missing values: replace all NaN (not a number) values in the 'final_df_processed' dataframe with 'N/A'.
final_df_processed.fillna('N/A', inplace=True)

# convert date format: using a lambda function, change the 'date' column in the 'final_df_processed' dataframe, from the string format "yyyy-mm-dd" to a datetime object.
final_df_processed['Date'] = final_df_processed['Date'].apply(lambda x:datetime.strptime(x, "%Y-%m-%d"))

# %%
final_df_processed.info()

# %%
final_df_processed.head()

# %%
# retrieve the number of rows and columns in the 'final_df_processed' DataFrame.
final_df_processed.shape

# %%
# set the 'sub category' column in the 'final_df_processed' DataFrame to have the value 'unassigned' for all rows.
final_df_processed['Sub Category'] = 'Unassigned'
final_df_processed.head()

# %% [markdown]
# # experimenting automating sub categories with fuzzy matching - dynamically create sub categories

# %% [markdown]
# #### automation process for categorizing transactions based on their descriptions, using fuzzy matching.

# %%
# import libraries for string matching and similarity comparison
from thefuzz import fuzz, process

# convert the values from the 'Description' column of the 'final_df_processed' DataFrame to a list.
discription_list = final_df_processed['Description'].tolist()

# list of categories to add to the 'Sub Category' column
my_list = ['barfoot','auto finance','gas','mylotto','transfer','cake','restaurant','baby','pizza','ez wash','laundry','dash','beenz','rayden','big chill', 'paramount','transcoint','producepronto','uber','kfc','pak n save','countdown','new world','mcdonald','noodle canteen','menulog','dsouza']

for item in my_list:
    # fuzzy matching: using the 'process.extractOne' function, find the closest matching item to the given 'item' from the 'description_list', and store the result in the 'match_list'
    match_list = list(process.extractOne(item, discription_list))

    # same fuzz function, but with 'limit' you can have number of checks with different matching score
    # match_list = list(process.extract(item, discription_list, limit=3))

    # using 'scorer=fuzz.token_sort_ratio' method
    # match_list = list(process.extract(item, discription_list, limit=8, scorer=fuzz.token_sort_ratio))

    # fuzz gives a list of matching values and matching scores
    print(item,'=', match_list)

    # if the matching score between the keyword and the best match is greater than or equal to 80 (as determined by match_list[1]), the code enters a conditional block.
    if int(match_list[1]) >= 80:
        final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(item), str(item).title(), final_df_processed['Sub Category'])

final_df_processed.head()


# %%
# import and view: import the 'xlwings' library and use it to display the 'final_df_processed' DataFrame in Excel
import xlwings as xw
xw.view(final_df_processed)

# %% [markdown]
# # assign sub-categories - a manual way of sub categorizing

# %%

#---------------------------------------------------------
salary = 'beenz|rayden|big chill|paramount|transcoint|producepronto'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(salary) & (final_df_processed['Amount'] > 0), 'Salary', final_df_processed['Sub Category'])
#---------------------------------------------------------
other_income = 'dsouza|upwork|freelance|trading'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(other_income) & (final_df_processed['Amount'] > 0), 'Other Income', final_df_processed['Sub Category'])
#---------------------------------------------------------
rental = 'barfootnthomp|raywhite|kiwiprop'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(rental) & (final_df_processed['Amount'] < 0), 'Rental', final_df_processed['Sub Category'])
#---------------------------------------------------------
utility = 'water|carpetclean|matagas|pulse energy|kiwi elec|watercare'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(utility) & (final_df_processed['Amount'] < 0), 'Utility Bills', final_df_processed['Sub Category'])
#---------------------------------------------------------
restr = 'hambagu|donuts|zoo keeper|burger|bakery|pavilion|mac roast|calimero|bakeries|wendys|starbucks|mcdonald|smovenpick|kitchen|lunch bar|takeaway|subway|cuisine|pizza|bunga raya|umiya|father ted|kfc|aroy|lonestar|burgerfuel|cake|restaurant|paradise|top in town|wok|takashi|kreem|scoopers|mamarich|cafe'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(restr) & (final_df_processed['Amount'] < 0), 'Restaurants', final_df_processed['Sub Category'])
#---------------------------------------------------------
grocs = 'countdown|pak n save|serandibmt|spiceinvader'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(grocs) & (final_df_processed['Amount'] < 0), 'Groceries', final_df_processed['Sub Category'])
#---------------------------------------------------------
transfers = 'transfer|transfers'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(transfers), 'Transfers', final_df_processed['Sub Category'])
#---------------------------------------------------------
transport = 'carjam|nz transport agency|nz motor factory|vtnz'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(transport) & (final_df_processed['Amount'] < 0), 'Motor Vehicle', final_df_processed['Sub Category'])
#---------------------------------------------------------
fuel = 'fuel|filling|mobil|waitomo'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(fuel) & (final_df_processed['Amount'] < 0), 'Fuel', final_df_processed['Sub Category'])
#---------------------------------------------------------
travel = 'qantas|transco|uber|dash|athop|airnz'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(travel) & (final_df_processed['Amount'] < 0), 'Travel', final_df_processed['Sub Category'])
#---------------------------------------------------------
internet = 'slingshot'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(internet) & (final_df_processed['Amount'] < 0), 'Internet', final_df_processed['Sub Category'])
#---------------------------------------------------------
gym = 'jetts|cityfitness'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(gym) & (final_df_processed['Amount'] < 0), 'Gym', final_df_processed['Sub Category'])
#---------------------------------------------------------
Laundry = 'laundromat|laundry'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(Laundry) & (final_df_processed['Amount'] < 0), 'Laundry', final_df_processed['Sub Category'])
#---------------------------------------------------------
entertainment = 'hoyts|event cinemas'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(entertainment) & (final_df_processed['Amount'] < 0), 'Entertainment', final_df_processed['Sub Category'])
#---------------------------------------------------------
car_finance = 'auto finance direct|toyota finance'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(car_finance) & (final_df_processed['Amount'] < 0), 'Car Finanace', final_df_processed['Sub Category'])
#---------------------------------------------------------
insurance = 'aia nz|aioi nissay'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(insurance) & (final_df_processed['Amount'] < 0), 'Insurance', final_df_processed['Sub Category'])
#---------------------------------------------------------
shopping = 'bunnings|paper plus|briscoes|shein|aliexpress|baby fac|kmart|pb technologies|cotton on'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(shopping) & (final_df_processed['Amount'] < 0), 'Shopping', final_df_processed['Sub Category'])
#---------------------------------------------------------
telecom = '2degrees|vodafone|onenz'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(telecom) & (final_df_processed['Amount'] < 0), 'Telecom', final_df_processed['Sub Category'])
#---------------------------------------------------------
edu = 'industryconnec|industry connect|udemy'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(edu) & (final_df_processed['Amount'] < 0), 'Education', final_df_processed['Sub Category'])
#---------------------------------------------------------
health = 'otahuhu health|local doctors'
final_df_processed['Sub Category'] = np.where(final_df_processed['Description'].str.contains(health) & (final_df_processed['Amount'] < 0), 'Health & Medical', final_df_processed['Sub Category'])



final_df_processed.head()

# %% [markdown]
# # assign categories

# %%
# create new column "Category" and add 'Unassigned' to each row initially
final_df_processed['Category'] = 'Unassigned'

#---------------------------------------------------------
income = 'Other Income|Salary'
final_df_processed['Category'] = np.where(final_df_processed['Sub Category'].str.contains(income), 'Earnings', final_df_processed['Category'])

#---------------------------------------------------------
transport = 'Fuel|Car Finance|Motor Vehicle'
final_df_processed['Category'] = np.where(final_df_processed['Sub Category'].str.contains(transport), 'Transport', final_df_processed['Category'])

#---------------------------------------------------------
living = 'Groceries|Restaurants|Shopping'
final_df_processed['Category'] = np.where(final_df_processed['Sub Category'].str.contains(living), 'Living Expenses', final_df_processed['Category'])


final_df_processed.head()

# %% [markdown]
# # assign category type

# %%
final_df_processed['Category Type'] = 'Unassigned'

#---------------------------------------------------------
income_type = 'Earnings'
final_df_processed['Category Type'] = np.where(final_df_processed['Category'].str.contains(income_type), 'Income', final_df_processed['Category Type'])

#---------------------------------------------------------
expense_type = 'Transport|Living Expenses'
final_df_processed['Category Type'] = np.where(final_df_processed['Category'].str.contains(expense_type), 'Expense', final_df_processed['Category Type'])

final_df_processed.head()

# %% [markdown]
# # exporting data to master File

# %%
folder_path_presentation = Path(r'../Data/3.Presentation-Gold/')

# %% [markdown]
# #### select date range to export

# %%
# start_date = pd.to_datetime("2023-01-01")
# end_date = pd.to_datetime("2023-06-30")

# final_df_processed_ranged = final_df_processed[(final_df_processed['Date'] >= start_date) & (final_df_processed['Date'] <= end_date)]

# # export data from above given date range
# file_master = folder_path_presentation / "Master_File.csv"
# final_df_processed_ranged.to_csv(file_master, index=False)
# final_df_processed_ranged.head()

# ---------------------------------------------------------------------

# same with use of a function
# def select_date_range(df:final_df_processed, start, end)->pd.DataFrame:
#     final_df_processed_ranged = df[(df['Date'] >= pd.to_datetime(start)) & (df['Date'] <= pd.to_datetime(end))]
#     return final_df_processed_ranged


# Calling the function
# option to set a date range for export
# fdf = select_date_range(final_df_processed,"2023-01-01", "2023-06-30")
# file_master = folder_path_presentation / "Master_File.csv"
# fdf.to_csv(file_master, index=False)

# %% [markdown]
# #### export the full final dataframe to a csv file

# %%
file_master = folder_path_presentation / "Master_File.csv"
final_df_processed.to_csv(file_master, index=False)
final_df_processed.head()

# %% [markdown]
# # show unassigned values

# %%
# unassigned = final_df_processed.loc[final_df_processed['Category'] =='Unassigned']
# unassigned

# %% [markdown]
# # optional step - try later

# %%
# file_path_to_check = Path(r'C:\Users\sular\OneDrive\1.Data Analysis & Engineering\Data Engineering\Data Engineering Projects\12.Personal Finance Dashboard - Power BI\Data\2.Processed-Silver')
# list_A = list(file_path_to_check.rglob('*.csv'))
# list_A

# for file in list_A:
#     if os.path.isfile(file):
#         os.remove(file)
#     else:
#         final_df_asb.to_csv(file_path_asb, index=False)
#         final_df_anz.to_csv(file_path_anz, index=False)


# if list_A=0:
#     print(f"File does not exist. New file is downloaded.")
#     final_df_asb.to_csv(file_path_asb, index=False)
#     final_df_anz.to_csv(file_path_anz, index=False)
# else:
#     os.remove(file_path)
#     print(f"File is already exists and its deleted. Please run the code again")
    

# %% [markdown]
# # loading data to SQL database

# %%
# import sqlalchemy, pyodbc, socket

# %%
# socket.gethostname()

# %%
# pyodbc.drivers()

# %%
# conn = sqlalchemy.create_engine(f'mssql+pyodbc://{socket.gethostname()}/Personal_Finanace?trusted_connection=yes&driver=ODBC Driver 17 for SQL Server')
# final_df_processed.to_sql("Master_Data", con=conn, if_exists="replace", index=False)



