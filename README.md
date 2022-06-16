# Synapse Hackathon

## Exercise 0: Environment Setup: 
For this workshop you need to have access to an Azure Synapse instance. To create an instance you can either choose from Microsoft Analytics end-to-end with Azure Synapse ARM template which deploys full set of services related to Analytics including Synapse.  
[![image](https://user-images.githubusercontent.com/40135849/174113982-d6f86cc2-7590-49b7-9a44-c18400614444.png)](<https://portal.azure.com/#blade/Microsoft_Azure_CreateUIDef/CustomDeploymentBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-synapse-analytics-end2end%2Fmain%2FDeploy%2FAzureAnalyticsE2E.json/uiFormDefinitionUri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-synapse-analytics-end2end%2Fmain%2FDeploy%2FcreateUiDefinition.json>)  
Or [create a single Synapse workspace](<https://docs.microsoft.com/en-us/azure/synapse-analytics/quickstart-create-workspace>) via [Azure Portal](<https://portal.azure.com/>). 

## End-To-End Solution Architecture:
![Solution Arc](https://user-images.githubusercontent.com/40135849/174117794-0063d7bd-4cdc-4cfc-8108-669b9cff89a8.jpg)


## Exercise 1: [Load data](<./Exercise 1.md>) 
At this exercise you connect to different data sources that WWI needs to collect data and create a pipeline to move data into a centralized data lake. No transformation over data is required. The solution should cover delta load for big tables.

## Exercise 2: [Transformation](<./Exercise 2.md>) 
At this exercise you get to explore different flavors of Synapse runtime. Do ad-hoc data wrangling and use Azure Synapse Lake databse patterns.
 
  
## Exercise 3: Datawarehouse (D2 3H)
   Star schema transformation:
   <ul> 
    <li> Data flows
    <li> Spark Pools
    </ul>
    Save data as Delta files
    
## Exercise 4: Reporting (Mark-Marijn) (D2 1H)
  PowerBI integration with Synapse
  Push data marts to PowerBI
  
## Good to have Exercises:
### Exercise 5: Machine Learning (share a ONNX model to play with)
### Exercise 6: Purview
### Exercise 7: Data Mesh

