# Load data from source systems
Before being able to do any analysis over our different source systems, we need to copy data from different source systems to a single data lake. A data lake is often designed to be a central repository of unstructured, semi-structured and structured data. in this scenario we would like our analysis to be decoupled from our source systems, therefore we will copy raw  data to a raw zone of a data lake. 
## Task 1: Create a raw zone in your ADLSG2:
## Task 2: Create copy pipeline:
These pipelines should keep a metadata to load at large scale.
  Data Source | User | Pass
  -------|-----|------
  server rvdwwi.database.windows.net  |  admin_user | Hack12345!
 - Navigate to Integrate blade, click on the plus and choose "Copy Data Tool".  
 - Choose Metadata-driven copy task and select a database to create and maintain a control table.  
 - Follow the wizard to choose a source data store and select tables you would like to copy over.   
 - Please exclude below tables:  (These tables include sql geography data type which is not yet supported in "meta data driven copy" activity.)
        Table Name   |  Column Name  
      -------------|-------------
      Suppliers  |                                                                                                                     DeliveryLocation
      Customers     |                                                                                                                  DeliveryLocation
      SystemParameters    |                                                                                                           DeliveryLocation
      StateProvinces_Archive   |                                                                                                       Border
      Countries_Archive  |                                                                                                          Border
      Cities_Archive    |                                                                                                               Location
      Cities     |                                                                                                                     Location
      Countries     |                                                                                                                   Border
      Suppliers_Archive   |                                                                                                             DeliveryLocation
      Suppliers     |                                                                                                                   DeliveryLocation
      Customers_Archive  |                                                                                                              DeliveryLocation
      Customers      |                                                                                                                  DeliveryLocation
      StateProvinces  |                                                                                                                 Border

 - Choose loading behavior per table, select appropriate watermark column for Delta load.  
-  Choose a target destination to land data. make sure tp choose .parquet or .csv format as this will be a requirment to use lakedatabase patterns in Exercise 2.  
-  Review summary of the pipline and deploy. make sure to copy the scripts for control table and execut them on the database.  
 - Review the created pipelines, try to understand what are the activities in each pipeline and try to track the logic.  
-  Trigger the pipeline to move data to your "raw zone".  

  
## Task 3: Run your pipeline and monitor:
  Navigate to Monitor blade and monitor your pipelines run. make sure that all pipelines run successfully before moving to next step.
