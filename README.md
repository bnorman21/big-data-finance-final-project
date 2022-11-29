# big-data-finance-final-project

**Intro**

The goal of my project is to explore Market Cap/Revenue multiples (Price-to-sales ratio) of public Information Technology and Communication Services companies with differing revenue growth and net profit margin over the past 25 years in contrasting interest rate environments

I am very interested in venture investing and, like in many other areas of the market, VCs have significantly flipped their weighting of growth versus profitability in the past year due to rising rates and inflation. I am motivated to see how tech and media companies have historically been valued based on their revenue growth and profitability in differing cost-of-capital time periods. Public markets are where consensus gets formed and this historical natural tension between growth and profitability is what I am looking to delve deeper into.

**How I created my data set**

_Company Financial Data_

To get the quarterly revenue and net income data, I used S&P Global Market Intelligence’s Compustat Fundamentals Quarterly database through the Wharton Research Data Services (WRDS) platform. I queried all companies in the database with a GIC Sector of 45 (Information Technology) or 50 (Communication Services) from 1995 to the present. The key information I pulled in my query was the quarterly net income and revenue for each company along with the company’s CUSIP and industry group, industry, and sub-industry identifier.

I used Python and its Pandas library to further process the financial history dataset:
- To ensure less variability, I filtered out all companies whose quarterly revenue was less than $15M.
- Using Panda’s groupby and pct_change functionalities, I calculated the quarterly revenue growth rate for each company compared to its revenue for the same quarter the previous year. 
- I calculated the quarterly net profit margin for each company by dividing the company’s quarterly net income by its quarterly revenue. 
- I created a quarterly growth profitability score (gp_score) for each company by adding together its quarterly net profit margin and quarterly revenue growth rate together

_Company Market Capitalization Data_

To get the data for the quarterly average market cap for each company, I used the Center for Research in Security Price’s (CRSP) Stock Monthly Aggregate Security Data through the Wharton Research Data Services (WRDS) platform. For each company in my financial history dataset, I queried all available monthly market capitalization data from 1995 to the present.

I used Python and its Pandas library to further process the market cap history dataset:
- I restructured the data set to include a year and quarter for each monthly market cap data point.
- I then grouped the data by CUSIP, quarter, and year, to generate a quarterly average market cap for each company.

_Merging Market Cap and Financial History Data_

To accurately merge the market cap and financial history data and performed a few steps of data wrangling to help in the process
- In the financial history data set, I made sure to include a current quarter and a forward quarter
- The current calendar quarter (‘datacqtr’ in the raw data) represents the financial data the quarter pertains to
- The forward calendar quarter (‘forward_c_qtr’ in the raw data) represents the following quarter after the financial results
- The market cap history data set and financial history data set are then merged on the CUSIP and forward calendar quarter
- This results in the quarterly revenue growth rate, quarterly net profit margin, and quarterly growth profitability score for each company being linked to its quarterly average market cap for the following quarter instead of the same quarter. 
  - Note this is not a perfect approach since earnings are not released at the beginning of each forward quarter and sometimes can be released toward the middle or end of the forward quarter (room for improvement here that I'm currently thinking about)
- To calculate the Price/Sales ratio I then divided the average market cap in the forward quarter by the revenue in the previous quarter for each company for each quarter.

_Linking Interest Rates to the Financial and Market Cap History Merged Dataset_

To accurately gauge the cost of capital environment, I pulled the monthly data of the ICE BofA BBB US Corporate Index Effective Yield from FRED here (open to suggestions if you feel a different tracker would be better). In a similar manner to the way I handed the market cap data:
- I restructured the data set to include a year and quarter for each monthly yield data point.
- I then grouped the data by quarter to generate an average quarterly bbb interest rate
- I then merged the interest rate to the broader data set by performing an outer join on the forward calendar quarter date


**Analyzing the Data**

This is where most of the remaining work lies. Stay tuned!
