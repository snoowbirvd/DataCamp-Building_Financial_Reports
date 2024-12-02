# DataCamp-Building_Financial_Reports
Creating a holistic beginner’s guide for financial analysis through DataCamp's Projects

Given dataset: 
[Balance_sheet.xlsx](data/Balance_Sheet.xlsx)
[Income_Statement.xlsx](data/Income_Statement.xlsx)

### Inspecting the Data

```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 60 entries, 0 to 59
Data columns (total 14 columns):
 #   Column                     Non-Null Count  Dtype  
---  ------                     --------------  -----  
 0   Unnamed: 0                 60 non-null     int64  
 1   Year                       60 non-null     int64  
 2   comp_type                  60 non-null     object 
 3   company                    60 non-null     object 
 4   Accounts Payable           60 non-null     int64  
 5   Cash                       60 non-null     int64  
 6   Inventory                  44 non-null     float64
 7   Property Plant Equipment   60 non-null     int64  
 8   Short Term Investments     37 non-null     float64
 9   Total Assets               60 non-null     int64  
 10  Total Current Assets       60 non-null     int64  
 11  Total Current Liabilities  60 non-null     int64  
 12  Total Liab                 60 non-null     int64  
 13  Total Stockholder Equity   60 non-null     int64  
dtypes: float64(2), int64(10), object(2)
memory usage: 6.7+ KB
None
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 60 entries, 0 to 59
Data columns (total 9 columns):
 #   Column                    Non-Null Count  Dtype 
---  ------                    --------------  ----- 
 0   Unnamed: 0                60 non-null     int64 
 1   Year                      60 non-null     int64 
 2   comp_type                 60 non-null     object
 3   company                   60 non-null     object
 4   Cost Of Goods Sold        60 non-null     int64 
 5   Gross Profit              60 non-null     int64 
 6   Operating Income          60 non-null     int64 
 7   Total Operating Expenses  60 non-null     int64 
 8   Total Revenue             60 non-null     int64 
dtypes: int64(7), object(2)
memory usage: 4.3+ KB
None

```
### 1. Understanding the Dataset
We are analyzing financial data from two key datasets:

 - 1. Balance Sheet: Details the company’s financial position—what it owns (assets), what it owes (liabilities), and what’s left for shareholders (equity).
 - 2. Income Statement: Summarizes the company’s income (revenue) and expenses over time.

### Objectives
The goal of this analysis is to:

 - Identify which type of company (comp_type) has the lowest profitability ratio.
 - Identify the company type with the highest leverage ratio.
 - Examine the relationship between leverage and profitability for real estate companies.
   
This helps us understand how industries differ in profitability and risk, and how debt impacts profitability for specific industries.

### 2. Financial Ratios

#### Profitability Ratio
#### What It Is: #### Shows how efficiently a company converts its revenue into profit.
#### Why It Matters:#### High profitability ratios indicate efficient cost management and strong business performance, while low ratios may signal high expenses or pricing issues.
- **Formula**:  
  Profitability Ratio = (Total Revenue - Cost of Goods Sold) / Total Revenue
- **Code**:
  ```python
  df_ratios["profitability_ratio"] = (df_ratios["Total Revenue"] - df_ratios["Cost Of Goods Sold"]) / df_ratios["Total Revenue"]

