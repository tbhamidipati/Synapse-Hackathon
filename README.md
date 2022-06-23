# Synapse Hackathon

## Exercise 0: Environment Setup: 
For this workshop you need to have access to an Azure Synapse instance. To create an instance you can either choose from Microsoft Analytics end-to-end with Azure Synapse ARM template which deploys full set of services related to Analytics including Synapse.  
[![image](https://user-images.githubusercontent.com/40135849/174113982-d6f86cc2-7590-49b7-9a44-c18400614444.png)](<https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-synapse-analytics-end2end%2Fmain%2FDeploy%2FAzureAnalyticsE2E.json/uiFormDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-synapse-analytics-end2end%2Fmain%2FDeploy%2FcreateUiDefinition.json>)  
Or [create a single Synapse workspace](<https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-create-workspace>) via [Azure Portal](<https://portal.azure.com/>). 

## End-To-End Solution Architecture:
![Solution Arc](https://user-images.githubusercontent.com/40135849/174117794-0063d7bd-4cdc-4cfc-8108-669b9cff89a8.jpg)


## Exercise 1: [Data ingestion](<./Exercise 1.md>) 
At this exercise you connect to different data sources that WWI needs to collect data and create a pipeline to move data into a centralized data lake. No transformation over data is required. The solution should cover delta load for big tables.  
Task 1: [Create a raw zone in your ADLSG2](<https://github.com/MarziehBarghandan/Synapse-Hackathon/blob/main/Exercise%201.md#task-1-create-a-raw-zone-in-your-adlsg2>)  
Task 2: [Create copy pipeline](<https://github.com/MarziehBarghandan/Synapse-Hackathon/blob/main/Exercise%201.md#task-2-create-copy-pipeline>)  
Task 3: [Run your pipeline and monitor](<https://github.com/MarziehBarghandan/Synapse-Hackathon/blob/main/Exercise%201.md#task-3-run-your-pipeline-and-monitor>)
## Exercise 2: [Data preparation and transformation](<./Exercise 2.md>) 
At this exercise you get to explore different flavors of Synapse runtime. Do ad-hoc data wrangling and use Azure Synapse Lake databse patterns.  
Task 1: [Data quality checks](<https://github.com/MarziehBarghandan/Synapse-Hackathon/blob/main/Exercise%202.md#task-1-data-quality-checks>)  
Task 2: [Create Lake database](<https://github.com/MarziehBarghandan/Synapse-Hackathon/blob/main/Exercise%202.md#task-2-create-lake-database>)  

  
## Exercise 3: [Datawarehouse ](<./Exercise 3.md>)  
At this exercise you work on designing Datawarehouse schema. You investigate what Azure Synapse offers in terms of indexing and partitioning and pipeline design.    
Task 1: [Determine table category](<https://github.com/MarziehBarghandan/Synapse-Hackathon/blob/main/Exercise%203.md#task-1-determine-table-category>)  
Task 2: [Create Dedicated SQL pool and star schema](<https://github.com/MarziehBarghandan/Synapse-Hackathon/blob/main/Exercise%203.md#task-2-create-dedicated-sql-pool-and-star-schema>)  
Task 3: [Populate data warehouse tables with Spark pool](<https://github.com/MarziehBarghandan/Synapse-Hackathon/blob/main/Exercise%203.md#task-3-populate-data-warehouse-tables-with-spark-pool>)  
Task 4: [Populate data warehouse tables with Dataflow](<https://github.com/MarziehBarghandan/Synapse-Hackathon/blob/main/Exercise%203.md#task-4-populate-data-warehouse-tables-with-dataflow>)  
    
## Challenge 1: Power BI
In this challenege you need to create an ad-hoc report in Synapse to identify Top 20 states that are generating most order amount. The result of this task is shown below:
![image](https://user-images.githubusercontent.com/40135849/175316440-bd82f9ec-37e8-409d-ae08-60b6a70e8ac8.png)

Tip: [Link PowerBI workspace to Synapse workspace](<https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-power-bi>)

## Challenge 2: Machine Learning

## Challenge 3: Purview


