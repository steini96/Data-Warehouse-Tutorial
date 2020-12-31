# Data-Warehouse-Tutorial
A tutorial about how to set up a simple data warehouse with SQL Server, SQL Server Integration Services(SSIS) and Power BI.

# Neccesary tools
SQL Server  
SQL Server Management Studio(SSMS) 
SQL Server Integration (SSIS)  
Power BI  

# Chapter Overview
Creating the data warehouse database  
Creating the tables in the warehouse  
Using SSIS as a ETL tool  
Creating reports in Power BI

# 1. Creating the data warehouse database
We start with creating a new database to hold the data in the new data warehouse. To do that we open SQL Server Management Studio and right click "Databases" in the Object Explorer. Then you click "New Database..." and a modal window pops up.  
In the modal window you need to first fill out the name of the new database, I will call it SteiniDW and you can too or choose a name that you like. Then go into "Options" on the left side and set the "Recovery model" to Simple. After that you can go on and click "Ok".  
Then you can see your new database under databases.  
Now you need to create the Fact and Dimension Database Schemas. Click "New Query" to open up a new query window and copy the "CreateSchemas" from the Scripts folder in this Repository and paste it to the new query. Now execute the script by for example select the whole code and click "Execute". Now you hace created the Fact and Dimension Schemas and you can see them if you run the query "SELECT * FROM sys.schemas;".  

# 2. Creating the tables in the warehouse
Now we need to create the tables, here we will create one Fact table and one schema table. Begin with opening a new query window then find the "tables_script" in the Scripts folder and copy them into the query window and execute the code.  
