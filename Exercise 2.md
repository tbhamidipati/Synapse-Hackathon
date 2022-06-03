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
3. From list of indusctries choose Retail and click continue.
4. Take couple of minutes to review list of tables available. By clicking on each table you can read more infomation and see relationshios. For this task to keep it simple we will use only below tables.        
   Category | Table Name
   ---------|-----------
   Customer | Customer
   Order    | Order
   Order    | OrderLine

6. Select Map data (Preview) to create new data mapping, you need to turn on data flow debug mode for this step. if not you'll get a pop up to do so. This too helps you create ETL mappings and mapping data flows from raw data in ADLSG2 to Synapse lake database tables.
7. Select source type. in this task we are working with ADLSG2, next select linked service you created before to your ADLSG2 where your raw data resides. Choose the source data type (.Parquet). lastly browse your datalake to raw container and choose the path to your raw data. Click Continue. \
  ![image](https://user-images.githubusercontent.com/40135849/171680714-473151e3-0c3f-4cf7-bc02-2b7f95d122a8.png)
4. Choose a name for the new data maping and continue. This will take sometime to create the mapping.

