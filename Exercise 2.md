# Transform raw data
This exercise covers data process steps before creating a datawarehouse model. In task 1 you'll get to use Synapse compute engines to investige raw data and apply transformation and checks. Task 2 touches over Synapse lake databases. 

## Task 1: Data quality checks:
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
**Source options:** Navigate to Source options tab, choose File as File mode, browse and select Customer.parquet from datalake raw zone.   
**Projection:** Import schema to show all columns in Customer.parquet file.  
**Data preview:** Make sure you can preview your data here.  
6. Add a Sink step to the data flow. click on + and select sink from the list.  
![image](https://user-images.githubusercontent.com/40135849/174098977-73939afd-c021-470e-a3a9-6173e12108cb.png)  
**Sink:** Rename Output stream name to Customer, keep incomin stream to WWICustomer, choose Workspace DB as Sink type. select Database you've created at step 1. Select Customer table.  
![image](https://user-images.githubusercontent.com/40135849/174099972-ec67f719-4c29-4439-b23a-3696f030807f.png)  
**Mapping:** Make sure you have Skip duplicate input and output columns tick for all steps below.  
![image](https://user-images.githubusercontent.com/40135849/174661898-2fd03b4e-4815-4ae5-8424-989bf9d6ed62.png)  
7. Un-check Auto mapping and manualy set below mappings.  
![image](https://user-images.githubusercontent.com/40135849/174100629-ffc0bd83-5872-482b-a73b-b42b96f6828e.png)  
8. Create another Sink activity from WWICustomer output stream. Rename the sink to LegalEntityCustomer and select LegalEntityCustomer as Database Table. follow same step as above. make below map:  
![image](https://user-images.githubusercontent.com/40135849/174661702-0a86c362-505f-43d6-8e57-4a4b5f894693.png)  
9. Repeat steps 5 and 6 to create a new source for WWIOrder and a Sink to Order table. Make below mapping:  
![image](https://user-images.githubusercontent.com/40135849/174102652-de37b284-c2c9-428a-83cd-2db5e6653091.png)  
10.Repeat steps 5 and 6 to create a new source for WWIOrderLine and a Sink to OrderLine table. Make below mapping:  
![image](https://user-images.githubusercontent.com/40135849/174103031-172200f2-1d95-4696-b710-a11b6d6440e0.png)  
11. Step 10 might rise a column type mismatch, add a "Derived Column" activity in between steps to cast types before sink.  
![image](https://user-images.githubusercontent.com/40135849/174663547-a38cc55e-d233-4944-9d4b-df482a7956d3.png)  
12. Final Data flow should look like below:   
![image](https://user-images.githubusercontent.com/40135849/174103853-912d4eb7-e1f7-49a4-a8e3-0cbc6234a7a0.png)  
13. At this step we want to create a pipeline and trigger Data flow that we made. Navigate to Integrate blade and click on + icon. Create a new pipeline. Select a Data flow activity.  
 ![image](https://user-images.githubusercontent.com/40135849/174104621-748a37dd-f35f-498e-9f7c-d7e15b7c2c18.png)    
14. In Setting tab choose the right dataflow. Add trigger and Trigger now.
15. Navigate to Monitor blade to follow pipeline run. make sure your pipeline runs successfully.  
![image](https://user-images.githubusercontent.com/40135849/174663010-0b0b4049-64f5-4a49-82a5-f73d83cb1916.png)




