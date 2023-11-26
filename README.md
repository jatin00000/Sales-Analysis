# Sales-Analysis
## Data Analysis Outline
Data Analysis method requires a proper strategy to get useful insights from the data-set. <br/>
Before creation of Dashboard, we require to follow guildlines laid by AIMS Grid System.
### AIMS Grid System
The AIMS Grid System Project is a comprehensive exploration of data analysis methodologies, focusing on the AIMS grid system. This project entails understanding and implementing various steps, from defining the purpose to establishing success criteria. The primary objective is to harness data insights effectively using the AIMS grid system. <br/>
4 components of this system are<br/>
- **Purpose:** where the focus is on understanding the pain points and determining the specific objectives.<br/>
- **Stakeholders:** addressing the question of who will be involved in the project. <br/>
- **End Result:** a clear and specific objective to achieve after the project concludes, avoiding vague statements. Often, a powerful dashboard. <br/>
- **Success Criteria:** ensuring that, post-project completion, success can be precisely defined. AIMS grid provides a structured approach to articulate these success criteria, allowing for a clear evaluation of project success, even after significant investments.
- - -
### Data Warehouse
Often, when dealing with large volumes of data, it's crucial to ensure that your MySQL database remains unaffected by the queries executed in Power BI. To address this, many opt for the design of a data warehouse. In essence, a data warehouse involves extracting data from MySQL, an Online Transaction Processing (OLTP) system vital for regular sales operations. Preserving the stability of this OLTP system is paramount, as any disruption can hinder primary business functions.

The process involves performing Extract, Transform, Load (ETL) operations, commonly executed using tools like Apache NiFi or Python with pandas. This transformation is essential for reformatting data to suit analytical queries efficiently. Directly querying a MySQL database poses two significant challenges: potential slowdowns affecting mainstream operations and the fact that data in OLTP is often not in the desired format. ETL tools handle tasks like currency conversion, column removal, and various other transformations, optimizing data for analytical purposes.<br/>

Popular data warehouses like Teradata or Snowflake store the transformed data. Here, the role of ETL tools, such as Informatica or Apache NiFi, comes into play. Data engineers manage these tools, ensuring the smooth operation of the transformation process and the maintenance of data warehouse infrastructure. Subsequently, data analysts leverage data from the warehouse to construct Power BI dashboards.<br/>

In certain scenarios, the required data for analytics might not be readily available within the organization. Data analysts then invest time in capturing the necessary information to facilitate comprehensive data analysis. <br/>
- - -
## Project
### Problem statement
The problem faced by AtliQ Hardware, a company supplying computer hardware and peripherals, is outlined as follows: Bhavan Patel, the Sales Director, is encountering challenges due to the dynamic growth of the market. Despite having Regional Managers for North, South, and Central India, obtaining accurate insights into sales proves difficult. The conversations with regional managers are primarily verbal, leading to issues of incomplete or overly optimistic information. When Bhavan requests specific numbers, the managers provide numerous Excel files with extensive data, making it challenging for him to grasp the actual state of the business. Bhavan desires simplified, easily digestible insights to identify areas for improvement. He envisions a dashboard or visualization tool, like the one presented, to facilitate data-driven decision-making, allowing him to focus on strategies to enhance overall sales. <br/>
### AIMS Grid
- **Purpose** To unlock sales insights that are not visible before for sales team for decision support and automate them to reduced manual time spent in data gathering.
- **Stakeholders**
  - Sales Director
  - Marketing Team
  - Customer Service Team
  - Data and Analytics Team
  - IT
- **End Result** An automated dashboard providing quick and latest insights in order to support data driven decision making.
- **Success Criteria**
  - Dashboard(s) uncovering sales order insights with latest data available
  - Sales team able to take better decisions and prove 10% cost savings of total spend
  - Sales Analysts stop data gathering manually in order to save 20% of their business time and reinvest it value added activity.
### ETL performed
- Remove markets with no zone
- In transactions table, remove entries with transaction amount of less than 1.
- Transaction table, convert sales amount from USD to INR by creating a new column.
### Folder Structure
- **Data.sql** is the original mysql data-base.
- **Sales Analysis.pbix** is the dashboard created using Microsoft Power BI.
### Dashboard

### Data Analysis using Mysql
These are MySQL queries used to discover the dataset. <br/>
1. Show all customer records

    `SELECT * FROM customers;`

1. Show total number of customers

    `SELECT count(*) FROM customers;`

1. Show transactions for Chennai market (market code for chennai is Mark001

    `SELECT * FROM transactions where market_code='Mark001';`

1. Show distrinct product codes that were sold in chennai

    `SELECT distinct product_code FROM transactions where market_code='Mark001';`

1. Show transactions where currency is US dollars

    `SELECT * from transactions where currency="USD"`

1. Show transactions in 2020 join by date table

    `SELECT transactions.*, date.* FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020;`

1. Show total revenue in year 2020,

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and transactions.currency="INR\r" or transactions.currency="USD\r";`
	
1. Show total revenue in year 2020, January Month,

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and and date.month_name="January" and (transactions.currency="INR\r" or transactions.currency="USD\r");`

1. Show total revenue in year 2020 in Chennai

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020
and transactions.market_code="Mark001";`
### Data Analysis Using Power BI
Formula to create norm_amount column
`= Table.AddColumn(#"Filtered Rows", "norm_amount", each if [currency] = "USD" or [currency] ="USD#(cr)" then [sales_amount]*80 else [sales_amount], type any)`