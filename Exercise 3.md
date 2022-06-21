# Design a modern data warehouse
A modern data warehouse lets you bring together all your data at any scale easily, and means you can get insights through analytical dashboards, operational reports, or advanced analytics for all your users.
In this exersice you will practice a small scale mapping of OLTP model to OLAP. By the end of this exercise you have a Synapsecdedicated sql pool populated with your star schema fact and dimension tables. 
## Task 1: Determine table category:
Review data that resides in your Lake database. Think of how to design data warehouse fact and dimension tables. Map Customer, LegalEntityCustomer, Order, OrderLine table to 1 fact and 1 dimention tabel.
You can follow mapping below:   
![image](https://user-images.githubusercontent.com/40135849/174661007-e129ab94-76b6-41b4-bc1e-865c549703cb.png)
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
2. Navigate to Develope blade click on + and create a new SQL script. Connect to your dedicated SQL pool.
![image](https://user-images.githubusercontent.com/40135849/174281029-7e2f3299-41ff-4efe-866a-04a0380082f4.png)

4. Create a WWI Schema, CustomerDim and OrderFact tables. Run scripts below on your dedicated sql pool. These scrips create a new schema and CustomerDim and FactOrder.
``` sql
 CREATE SCHEMA WWI;
```
``` sql
 CREATE TABLE [WWI].[CustomerDim]
 (
   [CustomerId] [int] NOT NULL,
   [CustomerEstablishedDate] [datetime ]  NULL,
   [CustomerTypeId] [int]  NULL,
   [LedgerId] [int]  NULL,
   [LegalEntityName] [nvarchar](200)  NULL,
   [LegalEntityDateOfEstablishment] [datetime] NULL,
   [StateOfLegalEntityEstablishment] [nvarchar](150)  NULL
 )
 WITH
 (
   DISTRIBUTION = REPLICATE,
   CLUSTERED COLUMNSTORE INDEX
 );
```
``` sql
CREATE TABLE [WWI].[OrderFact]
  (
    [OrderId] [int]  NOT NULL,
    [OrderConfirmationNumber] [int]  NOT NULL,
    [OrderEnteredByEmployeeId] [smallint]  NOT NULL,
    [OrderReceivedTimestamp] [datetime]  NOT NULL,
    [OrderEntryTimestamp] [datetime]  NULL,
    [OrderRequestedDeliveryDate] [datetime]  NULL,
    [CustomerId] [int]  NOT NULL,
    [OrderLineNumber] [int]  NOT NULL,
    [ItemSku] [int]  NOT NULL,
    [Quantity] [tinyint]  NOT NULL,
    [ProductSalesPriceAmount] [money ]  NOT NULL,
    [OrderLineNote] [nchar]  NOT NULL
  )
  WITH
  (
    DISTRIBUTION = HASH ( [CustomerId] ),
    CLUSTERED COLUMNSTORE INDEX
  );
```
## Task 3: Populate data warehouse tables with Spark pool
Now we have our Datawarehouse schema and tables ready. We need to create a pipeline to populate fact and dimention tables. We can achieve this using Spark pool, Sql and Dataflow. Task 3 and Task 4 are resulting in same output. you can choose between either of them.
Within this task we will use Spark pool to read data from Lake database do some transformation and write to dedicated sql database.
1. Navigate to Data blade, under Lake database expand WWI. expand Tables and hover over Customer table actions, choose new notebook> Load to DataFrame. This will generate pyspark code to read data from lake database. Attach your spark pool to the notebook and run the cell. This might take sometime to warm up Spark cluster.
![image](https://user-images.githubusercontent.com/40135849/174665368-891736e6-7625-4302-9dee-d3d907b5ebcc.png)  
2. Replace the cell with below code to read data from Customer and LegalEntityCustomer, join data and cast types.
``` python
from pyspark.sql.functions import col
import pyspark.sql.types

%%pyspark
# Reading data from Customer table, select columns that we will use.
CustomerDF = spark.sql("SELECT CustomerId, CustomerEstablishedDate, CustomerTypeId, LedgerId  FROM `WWI_Hack`.`Customer` ")
CustomerDF.show(10)

# Reading data from LegalEntityCustomer table, select columns that we will use.
LECustomerDF = spark.sql("SELECT CustomerId,LegalEntityName,LegalEntityDateOfEstablishment,StateOfLegalEntityEstablishment FROM `WWI_Hack`.`LegalEntityCustomer` ")
LECustomerDF.show(10)

# Joining customer data 
inner_join = CustomerDF.alias("a").join(LECustomerDF.alias("b"), CustomerDF.CustomerId == LECustomerDF.CustomerId).select("a.*","b.LegalEntityName","b.LegalEntityDateOfEstablishment","b.StateOfLegalEntityEstablishment")

# Check schema for mismatch with CustomerDim table
inner_join.printSchema()

# Casting TimestampType
inner_join=inner_join.withColumn("CustomerEstablishedDate",col("CustomerEstablishedDate").cast(TimestampType()))\
                     .withColumn("LegalEntityDateOfEstablishment",col("LegalEntityDateOfEstablishment").cast(TimestampType())
                     
# Create a schema to match CustomerDim table
CustomerDimSchema =    [StructField('CustomerId',IntegerType(),False),\
                        StructField('CustomerEstablishedDate',TimestampType(),True),\
                        StructField('CustomerTypeId',IntegerType(),True),\
                        StructField('LedgerId',IntegerType(),True),\
                        StructField('LegalEntityName',StringType(),True),\
                        StructField('LegalEntityDateOfEstablishment',TimestampType(),True),\
                        StructField('StateOfLegalEntityEstablishment',StringType(),True)]
CustomerDim = sqlContext.createDataFrame(inner_join.rdd, StructType(CustomerDimSchema))

CustomerDim.show(10)
```
3.  Use below code to write the spark Dataframe from step 2 into CustomerDim sql table. Fill in values in < > . For more info read [here](<https://docs.microsoft.com/en-us/azure/synapse-analytics/spark/synapse-spark-sql-pool-import-export?tabs=python%2Cpython1%2Cpython2%2Cpython3%2Cpython4%2Cpython5#write-using-azure-ad-based-authentication>)
``` python
# Write using AAD Auth to internal table
# Add required imports
import com.microsoft.spark.sqlanalytics
from com.microsoft.spark.sqlanalytics.Constants import Constants

# Configure and submit the request to write to Synapse Dedicated SQL Pool
# Sample below is using AAD-based authentication approach; See further examples to leverage SQL Basic auth.
(df.write
 # If `Constants.SERVER` is not provided, the `<database_name>` from the three-part table name argument
 # to `synapsesql` method is used to infer the Synapse Dedicated SQL End Point.
 .option(Constants.SERVER, "<sql-server-name>.sql.azuresynapse.net")
 # Like-wise, if `Constants.TEMP_FOLDER` is not provided, the connector will use the runtime staging directory config (see section on Configuration Options for details).
 .option(Constants.TEMP_FOLDER, "abfss://<container_name>@<storage_account_name>.dfs.core.windows.net/<some_base_path_for_temporary_staging_folders>")
 # Choose a save mode that is apt for your use case.
 # Options for save modes are "error" or "errorifexists" (default), "overwrite", "append", "ignore".
 # refer to https://spark.apache.org/docs/latest/sql-data-sources-load-save-functions.html#save-modes
 .mode("overwrite")
 # Required parameter - Three-part table name to which data will be written
 .synapsesql("<database_name>.<schema_name>.<table_name>"))
```
4. Navigate to Data blade, under SQL database expand dedicated sql pool. Select Top 100 rows on WWI.CustomerDim to make sure our data has been moved.
![image](https://user-images.githubusercontent.com/40135849/174674946-6a6e4d69-88bb-488a-b434-133663f0e981.png)



## Task 4: Populate data warehouse tables with Dataflow
![image](https://user-images.githubusercontent.com/36922019/174898516-69ae3396-4479-4cd9-8cd2-f13d729ee829.png)
![image](https://user-images.githubusercontent.com/36922019/174900234-88bdd284-464f-4f53-8947-205e13eb1e64.png)
![image](https://user-images.githubusercontent.com/36922019/174900481-836223ed-e385-4066-8eb6-2094b761feea.png)
![image](https://user-images.githubusercontent.com/36922019/174900800-f27d993a-ac0d-43d2-86d9-21819c3d2e40.png)
![image](https://user-images.githubusercontent.com/36922019/174901016-c8134c1c-613b-4dd0-ba22-fe7966156b14.png)
![image](https://user-images.githubusercontent.com/36922019/174901771-17a052d2-4445-4c88-bb7a-656cbcaa426a.png)
![image](https://user-images.githubusercontent.com/36922019/174902155-ca4a99e7-53e4-44bb-80e7-fa030bd0872b.png)
![image](https://user-images.githubusercontent.com/36922019/174902357-6af30585-0360-4633-b82e-d01a43d8249f.png)
![image](https://user-images.githubusercontent.com/36922019/174902721-db273695-7576-46e5-a212-f114ab46545c.png)
![image](https://user-images.githubusercontent.com/36922019/174903314-07db117b-2cc3-40db-b225-bf4eee833b00.png)
![image](https://user-images.githubusercontent.com/36922019/174903691-a1b75db1-b318-4b6a-a993-f5b1f7b48739.png)
![image](https://user-images.githubusercontent.com/36922019/174905283-881591bd-cd7d-4499-bffb-9724d8a56dc4.png)
![image](https://user-images.githubusercontent.com/36922019/174905437-273465e2-dc51-4f0f-84d9-c151009f6d9b.png)

