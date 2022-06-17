# Design a modern data warehouse
A modern data warehouse lets you bring together all your data at any scale easily, and means you can get insights through analytical dashboards, operational reports, or advanced analytics for all your users.
In this exersice you will practice a small scale mapping of OLTP model to OLAP. By the end of this exercise you have a Synapsecdedicated sql pool populated with your star schema fact and dimension tables. 
## Task 1: Determine table category:
Review data that resides in your Lake database. Think of how to design data warehouse fact and dimension tables. Map Customer, LegalEntityCustomer, Order, OrderLine table to 1 fact and 1 dimention tabel.
You can follow mapping below:   
![image](https://user-images.githubusercontent.com/40135849/174264673-907105d5-ee08-4856-9f1a-ea0684b9a33c.png)

## Task 2: Create Dedicated SQL pool and star schema:
Now that we have our star schema conceptually designed, we need to create a dedicated sql pool and generate tables. 
1. Navigate to Manage blade and select SQL pools. if you have a Dedicated pool active use that, if it's paused resume and use. if there is no dedicated sql pool create one by clicking on +New.
![image](https://user-images.githubusercontent.com/40135849/174266273-4b0de2f3-f26b-415f-8778-61cce9211896.png)
Choose a name for your deducated SQL pool and put performance level to DQ100c. Review + create. Wait for successful deployment.
2. Create a WWI Schema, CustomerDim and OrderFact tables. Run scripts below on your dedicated sql pool.
```
CREATE SCHEMA WWI;
```
```
```
