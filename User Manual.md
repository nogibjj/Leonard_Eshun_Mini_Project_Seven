# User Manual

**Table of contents**  
1. How to install the package
1. The CLI commands
1. How to use the Commands


## How to install the package  

At the command prompt, type:   
```
pip install "path_to_the_package_file"
```
Where "path_to_the_package_file" is the path to the package file for distributing this application and ends in _tar.gz_.   

> [!NOTE]
> The optional dependencies defined in the .toml file are black (22.3.0), pytest (7.4.0), ruff (0.6.5). These are not regularly used by the end user but are used during development and testing. To install the optional dependencies, use:

```
pip install dist/sqlite_etl_mini_project-0.0.0.tar.gz [black,pytest,ruff]
```


## The CLI Commands

1. **extract** to extract the read an external csv file via its url and save to file in the /data folder using the name you give it. The database will be created if it doesn't exist.
	```python
	extract(url: str, file_name: str,) -> str
	```
	The parameters are:
	- url : The url for the external CSV file
	- file_name : The file name to use to save the CSV file locally

	>**Note:**
	>Give the CSV file a header (first row).


1. **transform_n_load** to create a number of tables in the SQLite database based on the table structures you give it for transformation, then saves the content of the csv file to the tables you created. 
	```python
	transform_n_load(    
		local_dataset: str,
    	database_name: str,
    	new_data_tables: dict,
    	new_lookup_tables: dict,
    	column_attributes: dict,
    	column_map: dict,)
	```
	The parameters are:
	- local_dataset : The local CSV file to load
	- database_name : The name of the database to be created	
	- new_data_tables : A dictionary of the tables non-lookup tables to be created. The key is the table name and the value is an array of columns.
	- new_lookup_tables : A dictionary of the tables lookup tables to be created. The key is the table name and the value is an array of columns.
	- column_attributes : A dictionary of the column attributes, eg. Integer, Primary Key. The key is the column name and the values are the attributes.
	- column_map : A dictionary maping the columns in the new tables defined above to the column indices in the CSV file. The key is the column.

	>**Note:**
	>The ID Primary Key of the table should always be the first column. 
	>Column names also shouldn't have spaces.


1. **read_data** to readon one data from the SQLite database based on the record id you give it.
	```python
	read_data(database_name: str, table_name: str, data_id: int)
	```
	The parameters are:
	- database_name : The name of the SQLite database containing the data.
	- table_name : The name of the table in the SQLite database.	
	- data_id : The ID of the record to be read.	

1. **read_all_data** to read all the records from the SQLite database.
	```python
	read_all_data(database_name: str, table_name: str)
	```
	The parameters are:
	- database_name : The name of the SQLite database containing the data.
	- table_name : The name of the table in the SQLite database.	


1. **save_data** to save records to a table you give it, following the table column structure.
	```python
	save_data(database_name: str, table_name: str, row: list)
	```
	The parameters are:
	- database_name : The name of the SQLite database containing the data.
	- table_name : The name of the table in the SQLite database.
	- row : A list of the items to be saved. The order should follow the exact output of the ```get_table_columns``` function for that table.

1. **delete_data** to delete a record from the database given a record ID.
	```python
	delete_data(database_name: str, table_name: str, data_id: int)
	```
	The parameters are:
	- database_name : The name of the SQLite database containing the data.
	- table_name : The name of the table in the SQLite database.	
	- data_id : The ID of the record to be deleted.	

1. **update_data** to update a record in the database using the table columns and a record ID.
	```python
	update_data(database_name: str, table_name: str, things_to_update: dict, data_id: int)
	```
	The parameters are:
	- database_name : The name of the SQLite database containing the data.
	- table_name : The name of the table in the SQLite database.
	- things_to_update : A dictionary of the items to be updated. The key is the column name and the value is the new data. The column names use must be in the output of the  ```get_table_columns``` function for that table.
	- data_id : The ID of the record to be updated.	

1. **get_table_columns** to get the column names of a table. This is useful for saving and updating.
	```python
	get_table_columns(database_name: str, table_name: str)
	```
	The parameters are:
	- database_name : The name of the SQLite database.
	- table_name : The name of the table in the SQLite database.   



## How to use the Commands
The main.py script provides a CLI allowing the ETL and CRUD operations to be done from the command line.


sqlite_etl read_all_data "air_quality.db" "indicator"


At the cli prompt, type:
```
python main.py
```

and follow with the argument information below, leaving space between arguments.
```python
	extract:
   		url
        file_name

    transform_n_load:
        local_dataset
        database_name
        new_data_tables, type=dict
        new_lookup_tables, type=dict
        column_attributes, type=dict
        column_map, type=dict

    read_data:
        database_name
        table_name
        data_id, type=int

    read_all_data:
        database_name
        table_name

    save_data:
        database_name
        table_name
        row, type=list

    update_data:
        database_name
        table_name
        data_id, type=int
        things_to_update, type=dict

    delete_data:
        database_name
        table_name
        data_id, type=int

    get_table_columns:
        database_name
        table_name
```

> [!IMPORTANT]
> It's important to provide the arguments in the order and formats as desribed above for the CLI to work.   


This selects includes all 3 optional dependencies during installation. Any number or combinations can be used.