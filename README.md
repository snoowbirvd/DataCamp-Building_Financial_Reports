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
**What It Is:** Shows how efficiently a company converts its revenue into profit.

**Why It Matters:** High profitability ratios indicate efficient cost management and strong business performance, while low ratios may signal high expenses or pricing issues.
- **Formula**:
  Profitability Ratio = (Profit) / Total Revenue
  Profit = Total Revenue - Cost of Goods Sold 
- **Code**:
  ```python
  df_ratios["profitability_ratio"] = (df_ratios["Total Revenue"] - df_ratios["Cost Of Goods Sold"]) / df_ratios["Total Revenue"]

#### Leverage Ratio
**Definition**: Indicates how much debt a company uses to finance its operations compared to equity.
**Why It Matters:** High leverage can support growth but increases financial risk, especially during economic downturns.

- **Formula**:  
  Leverage Ratio = Total Liabilities / Total Shareholder Equity
**Code**:
  ```python
  df_ratios["leverage_ratio"] = df_ratios["Total Liab"] / df_ratios["Total Stockholder Equity"]

### 3. How Did We Analyze the Data?
**Step 1: Merging Datasets**

We combined the balance sheet and income statement data into a single dataset (df_ratios) for comprehensive analysis.
**Code**:
  ```python
 df_ratios = pd.merge(balance_sheet, income_statement, on=['company', 'comp_type', 'Year'])
  ```
**Step 2: Calculating Ratios**

We added two new columns:

- Profitability Ratio: Measures how much profit companies make relative to their revenue.
- Leverage Ratio: Indicates the extent to which companies rely on debt financing.

**Code**:
  ```python
 df_ratios["profitability_ratio"] = (df_ratios["Total Revenue"] - df_ratios["Cost Of Goods Sold"])/df_ratios["Total Revenue"]

 df_ratios['leverage_ratio'] = df_ratios['Total Liab'] / df_ratios['Total Stockholder Equity']

print(df_ratios.info())
  ```
```markdown 
<class 'pandas.core.frame.DataFrame'>
Int64Index: 60 entries, 0 to 59
Data columns (total 22 columns):
 #   Column                     Non-Null Count  Dtype  
---  ------                     --------------  -----  
 0   Unnamed: 0_x               60 non-null     int64  
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
 14  Unnamed: 0_y               60 non-null     int64  
 15  Cost Of Goods Sold         60 non-null     int64  
 16  Gross Profit               60 non-null     int64  
 17  Operating Income           60 non-null     int64  
 18  Total Operating Expenses   60 non-null     int64  
 19  Total Revenue              60 non-null     int64  
 20  profitability_ratio        60 non-null     float64
 21  leverage_ratio             60 non-null     float64
dtypes: float64(4), int64(16), object(2)
memory usage: 10.8+ KB
```

**Step 3: Identifying Patterns**

Using pivot tables and visualizations, we:

1. Found the company type with the lowest profitability ratio.
2. Identified the company type with the highest leverage ratio.
3. Examined how leverage impacts profitability in real estate companies.

### 4. Analyzing Results
**Finding 1:** Lowest Profitability Ratio
**Result:** FMCG (Fast-Moving Consumer Goods) has the lowest profitability ratio.

**Code**:
  ```python
  profitability_pivot = df_ratios.pivot_table(index="comp_type", values="profitability_ratio")
  lowest_profitability = profitability_pivot['profitability_ratio'].idxmin()
  print('The company type with the lowest profitability ratio is: ' + str(lowest_profitability))

```
```markdown   
comp_type           profitability_ratio
                     
fmcg                  0.514396
real_est              0.534848
tech                  0.572062
The company type with the lowest profitability ratio is: fmcg
```
---
Explanation:

Low Margins: FMCG operates on low margins but compensates with high sales volumes.
Cash Flow: They sell goods quickly, resulting in low inventory levels and fast turnover.
Why It Matters: While low profitability might seem unfavorable, FMCG thrives on efficient operations and strong market presence.


