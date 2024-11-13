This project demonstrates an automated ETL process using SQL Server Integration Services (SSIS) that dynamically extracts data from a SQL Server database based on parameterized queries and exports the results to individual CSV files. Using a For Loop Container, the SSIS package iterates through specified values, updating SQL query parameters on each iteration to extract tailored data subsets. These subsets are transferred to CSV files, creating unique files for each data subset.
Therefore, this solution is particularly useful for scenarios requiring data segmentation and modular data exports, such as monthly reporting, partitioned data archiving, or periodic data snapshots for analysis. It leverages SSIS's robust looping and parameterization features to automate data export, streamline reporting, and ensure flexibility in data output formatting.
1. Create the SSIS Package:
   ----------------------------------------------
  1.	Open Visual Studio and create a new SSIS package.
  2.	In the Control Flow tab, drag a For Loop Container onto the canvas.
2. Configure the For Loop Container
  -----------------------------------------------
  1.	Set the initialization and iteration expressions based on your loop requirements. For instance, if iterating over date ranges or ID ranges, set variables like @Counter 
  2.	Configure the Loop Start, Stop, and Step conditions. For example:
    o	InitExpression: @Counter = 1
    o	EvalExpression: @Counter <= @maxcust
    o	AssignExpression: @Counter = @Counter + 1
3. Define Variables
   ----------------------------------------------
  1.	Add SSIS variables that will store dynamic values used in the SQL query, such as @Counter for query parameters.
  2.	Also, create a variable for the output file path (e.g., @Path) that will dynamically change for each iteration.
  3.	Another variable is for max value as @maxcust
4. Set Up a Parameterized SQL Query
   ----------------------------------------------
  1.	Drag an Execute SQL Task inside the For Loop Container.
  2.	Configure the SQL query with a parameter placeholder. For example:
    Sql code 1:
    select max(customerid) from orders
    Sql code 2:
    select * from orders where customerid=?
5. Data Flow Task for Data Transfer
   -----------------------------------------------
  1.	Inside the For Loop Container, add a Data Flow Task.
  2.	In the Data Flow tab, add an OLE DB Source to retrieve data from SQL Server. Set its SQL command to use the parameterized query you configured in the Execute SQL Task.
  3.	Map the parameter from the Execute SQL Task output to the OLE DB Source query.
6. Configure the Flat File Destination
   -----------------------------------------------
  1.	Add a Flat File Destination in the Data Flow to output data to a CSV file.
  2.	Set the File Connection Manager to use the dynamic file path variable @Path.
    o	To make each file unique, you can append @Counter or a timestamp to @Path in an Expression (e.g., " ""+ @[User::path] +"\\customer"+ (DT_WSTR,12)@[User::counter] +".CSV").MDX formula.
  3.	Map the columns from the source to the flat file destination.
7. Loop and Transfer Data
   ------------------------------------------------
The For Loop Container will iterate through the specified range, each time:
  •	Updating the parameter in the SQL query.
  •	Transferring the data to a new CSV file based on the file path variable.
Summary
---------------------------------------------------
  •	For Loop Container controls the iteration.
  •	Execute SQL Task sets the SQL parameter for each loop iteration.
  •	Data Flow Task transfers data to a CSV file using dynamic file paths.



