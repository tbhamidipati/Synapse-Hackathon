# Load data from source systems
Before being able to do any analysis over our different source systems, we need to copy data from different source systems to a single data lake. A data lake is often designed to be a central repository of unstructured, semi-structured and structured data. in this scenario we would like our analysis to be decoupled from our source systems, therefore we will copy raw  data to a raw zone of a data lake. 
## Task 1: Create a raw zone in your ADLSG2:
Navigate to [Azure portal](<https://ms.portal.azure.com/>), find you storage account and click on Containers.
***Img01***
## Task 2: Create copy pipeline:
At this step we need to create data pipelines to ingest data from WWI Sales database, these pipelines should keep a metadata to load at large scale.
  Data Source | User | Pass
  -------|-----|------
  server rvdwwi.database.windows.net  |  admin_user | Hack12345!
1. Navigate to Integrate blade, click on the plus and choose "Copy Data Tool".   
2. Choose Metadata-driven copy task and select a database to create and maintain a control table. Metadata-driven copy task make use of a control table to create parameterized pipelines. 
3. Create a Link Service. Linked services are much like connection strings, which define the connection information needed for the service to connect to external resources. 
4. Follow the wizard to choose a source data store and select tables you would like to copy over.   
5. Choose loading behavior per table, select appropriate watermark column for Delta load.  
6.  Choose a target destination to land data. make sure to choose .parquet or .csv format as this will be a requirment to use lakedatabase patterns in Exercise 2.  
7.  Review summary of the pipline and deploy. At this stage Synapse is creating two SQL scripts based on the wizard's setup. 
    - The first SQL script is used to create two control tables. The main control table stores the table list, file path or copy behaviors. The connection control table stores the connection value of your data store if you used parameterized linked service.
    - The second SQL script is used to create a store procedure. It is used to update the watermark value in main control table when the incremental copy jobs complete every time. make sure to copy the scripts for control table and execut them on the control database.
8. Open SSMS or Azure Studio to connect to your control table server, and run the two SQL scripts to create control tables and store procedure.
9. Query the main control table and connection control table to review the metadata in it.
10. Review the created pipelines, try to understand what are the activities in each pipeline and try to track the logic.  
11. Trigger the pipeline to move data to your "raw zone".  

  
## Task 3: Run your pipeline and monitor:
  Navigate to Monitor blade and monitor your pipelines run. make sure that all pipelines run successfully before moving to next step.
