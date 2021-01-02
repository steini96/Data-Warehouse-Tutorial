# Data-Warehouse-Tutorial
A tutorial about how to set up a simple data warehouse with SQL Server, SQL Server Integration Services(SSIS) and Power BI.

# Neccesary tools
SQL Server  
SQL Server Management Studio(SSMS) 
SQL Server Integration (SSIS)  
Power BI  

# Chapter Overview
Introduction
Creating the data warehouse database  
Creating the tables in the warehouse  
Using SSIS as a ETL tool  
Creating reports in Power BI

# Introduction
Welcome to this short Data Warehouse tutorial. Here we will create a few data sources and centralize that data in the Data Warehouse. We will also make a simple report from the data with PowerBI. The Data Warhouse will use a SQL Server database to hold the data and the ETL(Extract Transform Load) tool we will be using for transporting the data from the sources to the Data Warehouse will be SQL Server Integration Services.

# 1. Creating the data warehouse database
Start with creating a new database to hold the data in the new data warehouse. To do that you open SQL Server Management Studio and right click "Databases" in the Object Explorer. Then click "New Database..." and a modal window pops up.  
In the modal window you need to first fill out the name of the new database, I will call it SteiniDW and you can too or choose a name that you like. Then go into "Options" on the left side and set the "Recovery model" to Simple. After that you can go on and click "Ok".  
Then you can see your new database under databases.  
Now you need to create the Fact and Dimension Database Schemas. Click "New Query" to open up a new query window and copy the "CreateSchemas" from the Scripts folder in this Repository and paste it to the new query. Now execute the script by for example select the whole code and click "Execute". Now you hace created the Fact and Dimension Schemas and you can see them if you run the query "SELECT * FROM sys.schemas;".  

# 2. Creating the tables in the warehouse
Now we need to create the tables, here we will create one Fact table and one schema table. Begin with opening a new query window then find the "CreateTables" in the Scripts folder and copy them into the query window and execute the code.  

# 3. Creating the tables in the SQL source database
We need a database to use as a source database to try out our 
Go on and create a new database by clicking "New Database..." and name it to your liking, I will call it "SteiniSourceDB". Set the "Recovery model" to simple and click "Ok".  
You have now created the database and now need to create the tables. Copy the script from the "CreateTablesSQLSource" file and run it in a new query window. Then insert some dummy data into the tables by copying the script from the "InsertIntoSQLSourceTables" file and running it in a new query.  
You have now have tables with data that you can move into your Data Warehouse.


