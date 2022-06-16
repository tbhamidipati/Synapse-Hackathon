# Transform raw data
During this exercise we would like to implement data quilty checks and transform raw data into a industry schema.

## Task 1: Create data quality check rules:
The purpose of this task is to make you familiar with different options that Synapse offers to query and work with data. This task is achievable by Spark pool, sql serverless or mapping data flows. You can choose your approach to implement one of the below checks on tables:
   - Null values
   - Summary, Average, Standard Deviation, Percentiles for Numeric Columns
   - Distinct Count
   - Distribution Count
   - Data Quality (regex pattern)   


Here is an example for "Sales.Orders" Table. [Sales.Orders Data Checks](<./SalesOrders-DQ-Check.ipynb>)  
This example is based on: (<https://github.com/akashmehta10/profiling_pyspark>)

## Task 2: Create Lake database:
The lake database in Azure Synapse Analytics enables customers to bring together database design, meta information about the data that is stored and a possibility to describe how and where the data should be stored. Lake database addresses the challenge of today's data lakes where it is hard to understand how data is structured. Azure Synapse Analytics provides industry specific database templates to help standardize data in the lake. These templates provide schemas for predefined business areas, enabling data to be loaded into a lake database in a structured way. Use these templates to create your lake database and use Azure Synapse analytical runtime to provide insights to business users.
1. Navigate to Data blade. click on the plus and select Lake datbase.![image](https://user-images.githubusercontent.com/40135849/171678383-5e7f773c-2135-4ad3-aaeb-096fc17cff46.png)
2. Click on +Table to create tables from template.   
![image](https://user-images.githubusercontent.com/40135849/171694472-75b309ad-f17d-419c-8c94-3b964aa5f15d.png)   
3. From list of indusctries choose Retail and click continue.                                                                                            ![image](https://user-images.githubusercontent.com/31285245/171861020-5c68e8f8-6c5d-4802-9f2c-e70d8a6e2cab.png)

4. Take couple of minutes to review list of tables available. By clicking on each table you can read more infomation and see relationshios. For this task to keep it simple we will use only below tables.        
   Category | Table Name
   ---------|-----------
   Customer | Customer
   Customer | LegalEntityCustomer
   Order    | Order
   Order    | OrderLine  
![image](https://user-images.githubusercontent.com/40135849/174072478-0579d7a9-4365-44d2-a86c-b78d23145aa8.png)
Add all tables and give your lake database a name, publish.
5. Now that we have our Lake database schema ready we need to populate tables with data. There are 3 approaches to achieve this; Spark, Sql, Dataflows. feel free to choose the approach that fits you the best. for sake of this task we will investigate Dataflows. Navigate to Develope blade and click on + icon and create a new Data flow. add a new source by clicking on "Add Sources".
**Source settings:** Rename the source to WWICustomer, we will be loading customer data from datalake raw zone into lake database customer table. Choose Inline for Source type and select Parquet as dataset type, choose Linke service to ADLSG2 and test connection. ![image](https://user-images.githubusercontent.com/40135849/174074353-8b77d17b-0f57-4c94-bf2a-34077b4c6d02.png)
**Source options:** S

