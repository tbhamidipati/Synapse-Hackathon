# Load data from source systems
Before being able to do any analysis over our different source systems, we need to copy data from different source systems to a single data lake. A data lake is often designed to be a central repository of unstructured, semi-structured and structured data. in this scenario we would like our analysis to be decoupled from our source systems, therefore we will copy raw  data to a raw zone of a data lake. 
## Task 1: Create a raw zone in your ADLSG2:
1. Navigate to [Azure portal](<https://ms.portal.azure.com/>), find you storage account and click on Containers.
2. Click on +Container to create a new container named raw.
![image](https://user-images.githubusercontent.com/40135849/171682074-8374ff09-6449-41bb-9458-baa7629d84f9.png)

## Task 2: Create copy pipeline:
At this step we need to create data pipelines to ingest data from WWI Sales database, these pipelines should keep a metadata to load at large scale.
  Data Source | User | Pass
  -------|-----|------
  server rvdwwi.database.windows.net  |  admin_user | Hack12345!
1. Navigate to Integrate blade, click on the plus and choose "Copy Data Tool".   
![img02](https://user-images.githubusercontent.com/40135849/171682701-71ac460b-9f95-4f8d-a8b2-4f1b75e89c71.png)

3. Choose Metadata-driven copy task and select a database to create and maintain a control table. Metadata-driven copy task make use of a control table to create parameterized pipelines. 
4. Create a Link Service. Linked services are much like connection strings, which define the connection information needed for the service to connect to external resources. 
5. Follow the wizard to choose a source data store and select tables you would like to copy over. For this task make sure you have "Show views" checked. please copy over below tables with following loading behavior.     
   Schema | View Name | Load behaviour | Watermark column name | Watermak column value start
   -------|-----------|--------------|--------------|----------
   WWIHACK_SALES | Customers | Delta load | LastEditedWhen | 2013-01-01
   WWIHACK_SALES | CustomerCategories | Full load
   WWIHACK_SALES | CustomerTransactions | Full load
   WWIHACK_SALES | Orders | Delta load | LastEditedWhen | 2013-01-01
   WWIHACK_SALES | OrderLines | Full load
   WWIHACK_SALES | Invoices | Delta load | LastEditedWhen | 2013-01-01
   WWIHACK_SALES | InvoiceLines | Full load
   WWIHACK_SALES | SpecialDeals | Full load
7.  Choose a target destination to land data. make sure to choose .parquet or .csv format as this will be a requirement  to use lakedatabase patterns in Exercise 2.  
8.  Review summary of the pipeline and deploy. At this stage Synapse is creating two SQL scripts based on the wizard's setup. 
    - The first SQL script is used to create two control tables. The main control table stores the table list, file path or copy behaviors. The connection control table stores the connection value of your data store if you used parameterized linked service.
    - The second SQL script is used to create a store procedure. It is used to update the watermark value in main control table when the incremental copy jobs complete every time. make sure to copy the scripts for control table and execute them on the control database.
9. Open SSMS or Azure Studio to connect to your control table server, and run the two SQL scripts to create control tables and store procedure.
10. Query the main control table and connection control table in SSMS or Azure Studio  to review pipelines metadata. Read more about Control Tables [here](<https://docs.microsoft.com/en-us/azure/data-factory/copy-data-tool-metadata-driven#control-tables>). 
11. Navigate back to Integrate blade, under Pipelines there is a new folder with three pipelines. Review the created pipelines, try to understand what are the activities in each pipeline and try to track the logic. Read more about the pipelines [here](<https://docs.microsoft.com/en-us/azure/data-factory/copy-data-tool-metadata-driven#pipelines>).  
12. Trigger the pipeline to move data to your "raw zone".  

  
## Task 3: Run your pipeline and monitor:
  Navigate to Monitor blade and monitor your pipelines run. make sure that all pipelines run successfully before moving to next step.
