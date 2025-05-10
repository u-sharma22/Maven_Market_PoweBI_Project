# Maven_Market_PowerBI_Project

Maven Market Power BI Project
For this project, I utilized Power BI to clean, analyze and visualize a data set from Maven Analytics called Maven Market. Maven Market is a business which owns a variety of grocery stores of differing sizes across multiple countries including the United States, Canada and Mexico.

In this dashboard analysis I will review the insights I gained from creating and the report, some of the different thought processes I used to reach those conclusions as well as offering some potential solutions. This project was a bonus project from the Power BI Desktop For Business Intelligence Course from Maven Analytics and can be found on Udemy.

The Data Set
As stated earlier, Maven Market is a business which owns grocery stores in several different countries and of varying sizes and profitability.

The raw data came in the from of CSV files which were then brought into Power BI using Get Data. The data was then transformed and cleaned into a more workable set and was then closed and applied. This process was repeated for each table/CSV file until all 8 tables had been brought in and transformed.

The cleaning process for this data set was fairly straightforward, requiring mainly header promotion, data type corrections and creation of additional columns which would make working with the data set easier later on. Additionally, the raw data set provided 2 tables for transactions, one for each year (1997 and 1998). As both of these tables had the same table and column structure, they were appended into one table titled “Transactions Data” containing both years. I then conducted data verification and ensured the new table contained the correct number of rows and columns.

The Data Model
With all my tables set I began working on connecting them. For this data model I employed a snowflake schema for all but 2 of the tables and a star schema for the Regions and Stores tables. I designed the layout of the data model with the fact tables on the bottom (Transaction Data, Returns(97–98) Data) and the dimensional tables above them. This was done to achieve greater ease of viewing cardinality and cross filter direction.

For the purposes of this project, many to one cardinality and single cross filter direction were solely needed.

DAX Functions
After completing the tables, I was able to begin writing some DAX functions to create measures. These were used to calculate my KPI’s and other metrics which would later be placed inside my visuals. Some of the functions I used include:

CALCULATE(): this function was essential in the calculation of metrics such as those dealing with Returns, Profits, Revenue and Transactions.
Last Month Transactions = CALCULATE([Total Transactions], 
      DATEADD('Calendar Lookup'[Date],-1,MONTH))

      All(): This function was used in conjunction with the CALCULATE function to assist with the calculation of Returns, Profits, Revenue and Transactions by overriding all filter context.
All Returns = CALCULATE([Total Returns], ALL('Returns(97-98) Data'))
DATESINPERIOD(): Used to create a 60-Day Rolling Revenue measure by using the CALCULATE function to take the Total Revenue from the last 60 days by specifying the DATESINPERIOD to use the Date column and further utilize the MAX function on that same column, adding our interval of -60, and then specifying type as DAY.
60-Day Rolling Revenue = CALCULATE([Total Revenue], 
        DATESINPERIOD('Calendar Lookup'[Date], 
            MAX('Calendar Lookup'[Date]), -60, DAY))
SUM/SUMX(): Both SUM and SUMX were used in the report. The iterator function SUMX was used in this instance to calculate the Total Cost by multiplying the Quantity column from the Transactions table by the Product Cost Column in the Product Lookup table through the use of the RELATED() function.
Total Cost = 
SUMX('Transaction Data', 'Transaction Data'[quantity]
* 
RELATED('Product Lookup'[Product_Cost]
))
COUNTROWS/DISTINCTCOUNT(): This function was used to calculate the measure Total Returns by counting the number of rows in the Returns Data table.
Total Returns = 
COUNTROWS(
    'Returns(97-98) Data')
As a part of good practice, the measures were placed inside their own table, titled Measures Table.

Creating Visuals
For the report, I began by creating some KPI cards to display at the top of the report. These include Total Transactions, Total Profit, and Total Orders. Additionally, I included new measures for each of these indicators to calculate the previous month’s values and used it as a goal for the current month.

The report required a visual to demonstrate profit, transactions and returns as well as other indicators to give us a better understanding of business health. A matrix was added with these metrics broken down by product brand was added into the report and data bars and gradients were implemented for greater ease of understanding. Return Rate was set to High is Bad while the others remained in their default setting.

As I stated earlier, Maven Market owns grocery stores in 3 different countries; the United States, Canada and Mexico. Knowing this, it seemed beneficial to have a Map visual showing the number of transactions broken down by city, region, and country. A slicer was provided to show the data by country with a select multiple or select all option also added.

To round out the report, I decided to add 2 visuals for revenue. The first was a gauge chart showing Maven Market’s current month’s revenue with a target. For this target, I wrote a new measure which adds 5% to the previous month’s revenue. The second visual is a column chart which shows the weekly trending revenue for Maven Market based on the start of week and Total Revenue.

Insight Analysis
From an overall, executive perspective we can first begin by examining the KPI cards listed at the top which each show a specific metric based on the current month. As a reminder, these cards each have their own goal for the current month, which is the metric from the previous month.

The first card (Transactions) shows that we have surpassed our target for the month by 5.69% or approximately 1,000 transactions. The next card shows the current month’s profit and again we can see that we have exceeded our previous month’s profit by 5.6% or approximately $3,800. Lastly, we have the Returns KPI card which shows that we have exceeded our goal for returns and therefore the values are labeled in red. We will further evaluate these cards later on in the analysis. From an overall standpoint, Maven Market appears to be in overall good health, but there are clearly some areas for improvement. Let’s dive deeper.

Using the matrix I can check which brands had the highest transactions through the whole company and found the top 3 are Tell Tale (5,112), Tri-State (5,099) and Sunset (3,953). All 3 have relatively good profit margins of at least 58 percent. However, when viewing through a geographic lens, I found some values I wasn’t expecting.

The US and Mexico both seem to have fairly steady transactions, profits, profit margins and low return rates. However, when we look at the performance of the stores in Canada the data is telling a much different story. Total transactions in Canada (3,613) are much lower compared to the US (21,305) and Mexico (16,094). Canada’s Total Profit (13,915) is also substantially lower than the US (80,618) and Mexico’s (61,220). Profit Margin remains almost the exact same among the 3 countries. Finally, Canada’s Return Rate (11.25%) is more than 5 times higher than that of the US and Mexico’s.

Potential Solutions
There are many possible solutions to the poor performance of the Canadian stores varying in severity. One potential solution to address the low rate of transactions and the high rate of returns could be the brands currently being sold in the Canadian specific stores may not have the market or demand needed to be profitable and therefore should be pulled and new ones introduced. This solution would come with risks as there is no guarantee new products would be profitable. Additionally, market research and analysis would need to be conducted before moving forward which is time consuming and costly.

Another potential explanation could be that with the high rate of returns the brands products are defective in some way. However, given that the return rates for the same product are more than 4 times lower in the US and Mexico this seems unlikely.

Another, more aggressive, solution to this problem could be to close the Canadian Stores. After having completed this analysis with the information and context I have, I feel this would be the best option. The Canadian stores, while only having a presence in 2 cities, have demonstrated a highly volatile and consistently low revenue trend. Additionally, total transactions are exceptionally low at (453) resulting in consistently low profits. Finally, Return Rates for the products in these stores has been very high. Out of the 30 brands sold, 20 of them have return rates at or above 10% and the lowest rate being 6.67 % which is more than both the US and Mexico’s highest product return rates.

Conclusion
In conclusion, analyzing Maven Market’s operations across the United States, Mexico, and Canada has provided invaluable insights into the company’s performance and potential areas for improvement. Through meticulous examination, it became evident that the Canadian market was facing significant challenges with low sales, profits and alarmingly high return rates. As a solution, the strategic decision to close the two underperforming stores in Canada emerged as a practical course of action to optimize resources and refocus efforts on more promising markets.

This bonus project from the Power BI Desktop for Business Intelligence course, courtesy of Maven Analytics, has been a transformative experience, showcasing the power of data-driven decision-making. It has not only expanded my analytical skills but also deepened my understanding of business dynamics in a global context. The opportunity to work on this project has been immensely rewarding, reaffirming my passion for data analysis and its potential to drive meaningful change. I look forward to sharing these findings on my LinkedIn page, and I am excited to continue leveraging data insights to make informed decisions in the ever-evolving business landscape.
