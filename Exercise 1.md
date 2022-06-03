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

3. Choose Metadata-driven copy task and select a database to create and maintain a control table. Metadata-driven copy task make use of a control table to create parameterized pipelines.                                                                                                                      ![image](https://user-images.githubusercontent.com/31285245/171844258-1ec10bc2-ea4f-400b-a0da-0a34dc6053b9.png)

4. Create a Link Service to control table server. Linked services are much like connection strings, which define the connection information needed for the service to connect to external resources.                                                                                                            ![image](https://user-images.githubusercontent.com/31285245/171844767-1c6e55e2-8348-42db-b402-2b562f3b96e2.png)

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
![image](https://user-images.githubusercontent.com/31285245/171845525-b6b9bd40-35db-4578-939a-c79f4f1a000e.png)
![image](https://user-images.githubusercontent.com/31285245/171846038-d5a777f3-ff5a-4b1e-967f-d1b99711a64f.png)

6.  Choose a target destination to land data. make sure to choose .parquet or .csv format as this will be a requirement  to use lakedatabase patterns in Exercise 2.                                                                                                                                              ![image](https://user-images.githubusercontent.com/31285245/171846469-f5c8ebb7-af29-44d6-8c77-f2e2a339b63f.png)
 ![image](https://user-images.githubusercontent.com/31285245/171846563-09c9ceb4-b3d4-45d0-86e5-821f4402506b.png)
 ![image](https://user-images.githubusercontent.com/31285245/171846702-ee6fe692-a3fd-4aa7-9d7e-0030f0f8f45d.png)

7.  Review summary of the pipeline and deploy. At this stage Synapse is creating two SQL scripts based on the wizard's setup.                           ![image](https://user-images.githubusercontent.com/31285245/171846825-eb2ca53b-57d5-4f68-8f50-d38ccefea766.png)

    - The first SQL script is used to create two control tables. The main control table stores the table list, file path or copy behaviors. The connection control table stores the connection value of your data store if you used parameterized linked service.
    - The second SQL script is used to create a store procedure. It is used to update the watermark value in main control table when the incremental copy jobs complete every time. make sure to copy the scripts for control table and execute them on the control database.                               

8. Open SSMS or Azure Studio to connect to your control table server, and run the two SQL scripts to create control tables and store procedure.
9. Query the main control table and connection control table in SSMS or Azure Studio  to review pipelines metadata. Read more about Control Tables [here](<https://docs.microsoft.com/en-us/azure/data-factory/copy-data-tool-metadata-driven#control-tables>).                                            ![image](https://user-images.githubusercontent.com/31285245/171847381-689d276e-5510-4a97-a042-fb2c8c8efe53.png) 
10. Navigate back to Integrate blade, under Pipelines there is a new folder with three pipelines. Review the created pipelines, try to understand what are the activities in each pipeline and try to track the logic. Read more about the pipelines [here](<https://docs.microsoft.com/en-us/azure/data-factory/copy-data-tool-metadata-driven#pipelines>). 
![image](https://user-images.githubusercontent.com/31285245/171837367-bd3ca165-f087-452d-86e0-33e19b4f72d8.png)
11. Trigger the pipeline to move data to your "raw zone".  
![image](https://user-images.githubusercontent.com/31285245/171837761-da6ea66c-f0a1-4f09-a91a-16857692bd74.png) 
## Task 3: Run your pipeline and monitor:
  Navigate to Monitor blade and monitor your pipelines run. make sure that all pipelines run successfully before moving to next step.
