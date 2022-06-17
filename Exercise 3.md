# Design a modern data warehouse
A modern data warehouse lets you bring together all your data at any scale easily, and means you can get insights through analytical dashboards, operational reports, or advanced analytics for all your users.
In this exersice you will practice a small scale mapping of OLTP model to OLAP. By the end of this exercise you have a Synapsecdedicated sql pool populated with your star schema fact and dimension tables. 
## Task 1: Determine table category:
Review data that resides in your Lake database. Think of how to design data warehouse fact and dimension tables. Map Customer, LegalEntityCustomer, Order, OrderLine table to 1 fact and 1 dimention tabel.
You can follow mapping below:   
![image](https://user-images.githubusercontent.com/40135849/174264673-907105d5-ee08-4856-9f1a-ea0684b9a33c.png)
To design a well performing distributed tables using dedicated SQL pool we need to follow best practices, see tables below for a short review. for more info on Synapse distributions see [here](<https://docs.microsoft.com/en-us/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-tables-distribute>). 

| Table Indexing | Recommended use |
|--------------|-------------|
| Clustered Columnstore | Recommended for tables with greater than 100 million rows, offers the highest data compression with best overall query performance. |
| Heap Tables | Smaller tables with less than 100 million rows, commonly used as a staging table prior to transformation. |
| Clustered Index | Large lookup tables (> 100 million rows) where querying will only result in a single row returned. |
| Clustered Index + non-clustered secondary index | Large tables (> 100 million rows) when single (or very few) records are being returned in queries. |

| Table Distribution/Partition Type | Recommended use |
|--------------------|-------------|
| Hash distribution | Tables that are larger than 2 GBs with infrequent insert/update/delete operations, works well for large fact tables in a star schema. |
| Round robin distribution | Default distribution, when little is known about the data or how it will be used. Use this distribution for staging tables. |
| Replicated tables | Smaller lookup tables, less than 1.5 GB in size. |


## Task 2: Create Dedicated SQL pool and star schema:
Now that we have our star schema conceptually designed, we need to create a dedicated sql pool and generate tables. 
1. Navigate to Manage blade and select SQL pools. if you have a Dedicated pool active use that, if it's paused resume and use. if there is no dedicated sql pool create one by clicking on +New.
![image](https://user-images.githubusercontent.com/40135849/174266273-4b0de2f3-f26b-415f-8778-61cce9211896.png)
Choose a name for your deducated SQL pool and put performance level to DQ100c. Review + create. Wait for successful deployment.
2. Create a WWI Schema, CustomerDim and OrderFact tables. Navigate to Develope blade click on + and create a new SQL script. Connect to your dedicated SQL pool.
3.  Run scripts below on your dedicated sql pool. These scrips create a new schema and CustomerDim and FactOrder.
```
 CREATE SCHEMA WWI;
```
```
 CREATE TABLE [WWI].[CustomerDim]
 (
   [CustomerId] [uniqueidentifier] NOT NULL,
   [CustomerEstablishedDate] [datetime ]  NULL,
   [CustomerTypeId] [int]  NULL,
   [LedgerId] [int]  NULL,
   [LegalEntityName] [nvarchar](200)  NULL,
   [LegalEntityDateOfEstablishment] [datetime] NULL,
   [StateOfLegalEntityEstablishment] [nvarchar](21)  NULL,
   [StateOfLegalEntityResidence] [nvarchar](150)  NULL
 )
 WITH
 (
   DISTRIBUTION = REPLICATE,
   CLUSTERED COLUMNSTORE INDEX
 )
 GO
```
## Task 3: Populate data warehouse tables with Spark pool
## Task 4: Populate data warehouse tables with Dataflow
