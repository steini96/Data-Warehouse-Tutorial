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
Now you need to create the Fact and Dimension Database Schemas. Click "New Query" to open up a new query window and copy the "CreateSchemas" from the Scripts folder in this Repository and paste it to the new query. Now execute the script by for example selecting the whole code and click "Execute". Now you hace created the Fact and Dimension Schemas and you can see them if you run the query "SELECT * FROM sys.schemas;".  

# 2. Creating the tables in the warehouse
Now we need to create the tables, here we will create one Fact table and one schema table. Begin with opening a new query window then find the "CreateTables" in the Scripts folder and copy them into the query window and execute the code.  
Now you have created the tables that we need in the data warehouse in this tutorial.  
We also want to create an UNIQUE CLUSTERED INDEX though. To create it you need to open the "CreateUniqueClusteredIndex" file and run the script from it in a new query window.


# 3. Creating the tables in the SQL source database
We need a database to use as a source database to try out our 
Go on and create a new database by clicking "New Database..." and name it to your liking, I will call it "SteiniSourceDB". Set the "Recovery model" to simple and click "Ok".  
You have now created the database and now need to create the tables. Copy the script from the "CreateTablesSQLSource" file and run it in a new query window. Then insert some dummy data into the tables by copying the script from the "InsertIntoSQLSourceTables" file and running it in a new query.  
You have now have tables with data that you can move into your Data Warehouse.

# 4. Creating the excel table as an excel source
We are going to take data from an excel source so we need to make an excel file to use as such. You need to go so the "Excel sources" folder in the repository and save the "ExcelSourceSales" file from there to your computer.

# 5. Setting up the SSIS project
Here in this tutorial we are using SSIS as an ETL tool. We are using it to copy the data from a couple of different sources to the data warehouse database.  
To use SSIS you need to have Visual Studio and SQL Server Data Tools in Visual Studio installed. When you have that ready you can search for Visual Studio on the Windows Start menu and select Visual Studio. In the window that pops up you need to click "Create a new project" and then search for "Integration" and select "Integration Services Project". Then in the "Configure your new project" window choose an appropriate name, I will use the name "SteiniDWSSISProject", and choose an location for the project on our computer. After that you can click "Create" and therfore create your SSIS project.

# 6. Creating data flow tasks
Data flow tasks are tasks that cann be executed that transport data between sources.
Now create three data flow tasks in the control flow window by finding and selecting "Data Flow Task" three times in the "SSIS Toolbox" on the left side of the screen. Name those tasks "Fill up Store", "Fill up Sales" and Fill up Sales from Excel".  

### 6.1 Fill up store
Double click the "Fill up Store" task and then you are in its data flow window. In the data flow window you can say what is supposed to happen when the task is executed. In this task we want to take the data from the Store source table and move it into the Stores table in the data warehouse. Now double click the "Source Assistant" in the "SSIS Toolbox and a modal pop up should appear.  
Select "SQL Server" as the source type and double click "New..." in the "Select connection managers" section. Then another popup modal will appear that is called Connection Manager. In the Connection Manager you need to set the provider as "Native OLE DB\SQl Server Native Client 11.0". Then set the servername as the name of your SQL Server instance.  
You can see the name of your server instance in SQL Server Management Studio in the object explorer on the left side of the window. It is the object at the top of the object explorer and if you right click and select properties, you can see the name of the server instance.
Then set the name of the database you want to connect to in the SQL Server instance. You want to use the name of your SQL source database, for me it is "SteiniSourceDB". Then to test the connection to the database you can click "Text Connection" and then press "OK" to comfirm.  
Now double click the "Destination Assistant in the SSIS toolbox, set the destination type as "SQL Server" and double click "New..." just as before. Set the provider as "Native OLE DB\SQl Server Native Client 11.0", the server name as the name of your server instance and set the database name as the name of the Data Warehouse database(in my case it is "SteiniDW"). Then Test the connection and confirm by clicking "OK".  
Now you have a source and an destination. Double click the source object and set the data access mode to "SQL command". Then take the script, "SelectForStores", and paste it into the text field. Now you can preview the data that is being selected and then press "OK".  
Now you need to drag the blue arrow from the source to the destination. Then double click the destination and select the Store table from the list of tables, click "Keep Identity" and then go to mappings. In mappings you need to set what columns corrrespond to each other. Then press the "OK" button.  
Then you can go back to the control flow window, right click the "Fill up Store" task and click "Execute Task".  
Then the Stores table in the data warehouse should have the data from the SQL source database.

### 6.2 Fill up Sales
Double click the "Fill up Sales" task and then double click the Source assistant. Select SQL Server and double click "New...",  now you should be inside the Connection Manager. There you need to set the Provider as "Native OLE DB\SQl Server Native Client 11.0", also set the server name as your server instance. Then select your source database in the "Connect to a database" section, test the connection and press "OK".  
Now double click the destination assistant, select SQl Server and double click "New...". There you need to set the Provider as "Native OLE DB\SQl Server Native Client 11.0", also set the server name as your server instance. Then select your data warehouse database in the "Connect to a database" section, test the connection and press "OK".  
Now you have a source and an destination. Double click the source object,set the data access mode to "Table or view" and then select the Sales table. Now you can click "OK". 
You should now draw an blue arrow from the source to the destination and double click the destination. Set the data mode as "Table or view- fast load", select the Sales table and set activate "Keep identity". Now go to Mappings and set the corresponding input columns to the destination columns. Go back to Connection Manager and click "OK".
Then you can go back to the control flow window, right click the "Fill up Sales" task and click "Execute Task".  
Then the Sales table in the data warehouse should have the data from the SQL source database.

### 6.3 Fill up Sales from Excel
Double click the "Fill up Sales from Excel" task and then double click the Source assistant. Select Excel and double click "New...", now you should be in the Connection Manager. Now click browse and select the "ExcelSourceSales" file that you can get in this repository. Keep "First row has column names" selected and press the OK button. Now you have a source and an destination. Double click the source object,set the data access mode to "Table or view" and then select the Excel sheet. Now you can click "OK".  
Now double click the destination assistant, select SQl Server and double click "New...". There you need to set the Provider as "Native OLE DB\SQl Server Native Client 11.0", also set the server name as your server instance. Then select your data warehouse database in the "Connect to a database" section, test the connection and press "OK". 
You should now draw an blue arrow from the source to the destination and double click the destination. Set the data mode as "Table or view- fast load", select the Sales table and set activate "Keep identity". Now go to Mappings and set the corresponding input columns to the destination columns. Go back to Connection Manager and click "OK".  
Then you can go back to the control flow window, right click the "Fill up Sales from Excel" task and click "Execute Task".  
Then the Sales table in the data warehouse should have the data from the Excel source.

# 7. Create a report in Power BI
We are going to create a report in Power BI to visualize the data from the data warehouse.  
Start with opening Power BI and selecting "Get Data". Click the Database button to see only database sources and double click SQL Server Database. Write the name of your SQL Server instance under "Server" and write the name of the data warehouse database under "Database". Now you select "Fact.Sales" and click "Load".  
Now you have added one of two tables we are going to add into this Power BI project. On the left side of the Power BI desktop you can see three figures. One represents the Report page, one the Data page and one the Model page. We want to add another table into this project so you need to navigate onto the Data page and click the Get data button under Home. There you again double click SQL Server Database, set the server as the name of your SQL Server instance and write the name of your data warehouse database under database. You can then click the ok button and select the Dimension.Store table and click "Load".  
Now you have two tables imported into your project and you can see them on the right of the screen on the data page. Now we need to make the report from the data we just imported. You now need to go to the report page and select a stacked column chart under visualizations. Then you need to drag the name column in the field section on the page to the "Axis" in the Visualization section of the page. Then you need to drag the SalesID to "Values" and Country to "Legend". Now you have a simple chart of sales in different stores and see by the colors what country they are in.  
Next we want to create a map visualization(the sign for it is a globe) so click map. Now drag City to Location, SalesID to Size and Country to Legend. Then you can see that you have a map with circles of different size to indicate the amount of sales at each store.  
Now you know how to make simple visualization in Power BI and I encourage you to play around in it, it is a pretty straight forward reporting tool.

