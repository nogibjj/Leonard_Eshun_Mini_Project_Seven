# Data Engineering Mini Project Seven

[![CI](https://github.com/nogibjj/Leonard_Eshun_Mini_Project_Seven/actions/workflows/workflow.yml/badge.svg)](https://github.com/nogibjj/Leonard_Eshun_Mini_Project_Seven/actions/workflows/workflow.yml)


This repository is created as an assignment from the Data Engineering course, IDS 706. The aim is to package a python script into a command-line tool.   

The requirements are:   
1. Package a Python script with setuptools or a similar tool
1. Include a user guide on how to install and use the tool
1. Do the standard CI/CD setup
1. Include communication with an external or internal database (NoSQL, SQL, etc)


## The functions and what they do

1. **extract** to extract the read an external csv file via its url and save to file in the /data folder using the name you give it. The database will be created if it doesn't exist.

1. **transform_n_load** to create a number of tables in the SQLite database based on the table structures you give it for transformation, then saves the content of the csv file to the tables you created. 

1. **read_data** to read one data from the SQLite database based on the record id you give it.

1. **read_all_data** to read all the records from the SQLite database.	

1. **save_data** to save records to a table you give it, following the table column structure.

1. **delete_data** to delete a record from the database given a record ID.

1. **update_data** to update a record in the database using the table columns and a record ID.

1. **get_table_columns** to get the column names of a table. This is useful for saving and updating.

> [!NOTE]   
> Please refer to the [User Manual]("User Manual.md") for the details on how to use them.   

## CLI Integration
The main.py script provides a CLI allowing the ETL and CRUD operations to be done from the command line.   

To use the commands, install the package by running the command:   

At the cli prompt, type:   
```
pip install "path_to_the_package_file"
```

Where "path_to_the_package_file" is the path to the package file for distributing this application and ends in _tar.gz_.   

After installation, at the cli command prompt, type:   
```
sqlite_etl "command" "arguments"
```

The list of CRUD and related commands this package can execute are:   
1. extract
1. transform_n_load
1. read_data
1. read_all_data
1. save_data
1. update_data
1. delete_data
1. get_table_columns

Please find a User Manual on the parameters and how to use them [here]("User Manual.md")

> [!IMPORTANT]   
> It's important to provide the arguments in the order and formats as desribed above for the CLI to work.   

**Log of successful operations**    
The test operation saved its steps to a log file to show the success of the operations.   

[Please find the file here](Test_Log.md)


## Steps taken to implement the Package 
**1\. Work in a virtual environment**   
	This is necessary to avoid tampering with your main Python installation. Because the virtual environment provides a clean python environment, it also ensures that we get all the dependencies right in the package definition. If a dependent module isn't available, the package cli tests will immediately fail, even if they're available in the general python installation on the computer, so it can be fixed.   

![Installation Image](Installation.png)

We know we're working in the virtual environment when the command line in the terminal starts with the name of the specified virtual environment, (virtual_env) in this case.   

It was necessary to add the virtual_env folder created in the local Github directory to the .gitignore folder so it doesn't get pushed. Dependencies will be added with the requirements.txt file as a standard.   

Here are the commands to use a virtual environment:   

```
# Create a virtual environment named "myenv"
python3 -m venv myenv

# Activate the environment and use normally
source myenv/bin/activate

# Deactivate when done the environment and revert to the system environment
deactivate
```
Here are some other useful commands for setting up the virtual environment:   
```
# To initialize it with all the requirements of your project already listed for Github
pip install -r requirements.txt

# To display the system/virtual python environment currently being used
which python

# To display the version of python being used
python --version  

# This can be run in the system python environment to get all the current project dependencies
pip freeze > requirements.txt
```

**2\. Setup a Setuptools compliant file structure**   
Setuptools expect specific file structures in order to know what to add to the package and how.   

**The default structure**   
The flat structure.
```   
project_directory/           This is typically the base directory for the Github repository.   
├── pyproject.toml           The Setuptools .toml file for packaging.   
├── package_name/            Python package with source code.
│   ├── __init__.py          Makes the folder a package. Not required since python 3.
│   └── source.py            An example module containing source code.
├── tests/  
│   └── test_source.py       A file containing tests for the code in source.py.
└── README.md                README with information about the project.
```
**The alternative structure**   
This structure ensures that setuptools doesn't accidentally bundle any utility modules stored in the top-level directory with the package. It also helps to separate what is meant for Github from what is going into the package. Directories and files with special names are excluded by default regardless of which layout we choose, such as test/, docs/, and setup.py.   

```
project_directory/           This is typically the base directory for the Github repository.   
├── pyproject.toml           The Setuptools .toml file for packaging.   
├── src/  
│   └── package_name/        Python package with source code.   
│       ├── __init__.py      Makes the folder a package. Not required since python 3.   
│       └── source.py        An example module containing source code.   
├── tests/  
│   └── test_source.py       A file containing tests for the code in source.py.   
└── README.md                README with information about the project.   
```

**3\. Build a SetupTools .toml file**   
In this case, the file name is pyproject.toml. It controls the packaging process. There are situations where non-python files need to be added to the package. The MANIFEST.in file can be used for that. This is how the data files were added. Even though they could have been created via the ETL process, there are situations where we need the files (eg a JSON file with package relevant infomation) to be part of the package, so I added them. Please reference it to see how to include other items.   

**4\. Build the project**   
The command to build the project is:   
```
python -m build  
```
This is necessary to create the ditributable files. These are normally found in the /dist folder. I realized that to reference (import) local libraries, in this case the my_lib package, it works if the name is started with a ".". Please find examples in the transform.py and main.py files.   

**5\. Test installation in the virtual environment**   
During development, the package can be installed with the command:   
```
python -m pip install . 
```
This can be run without first building the package. It implies you can install something for tests which won't be in the packaged .tar.gz file. Thus it's important that the subsequent alternative installation command is used:   
```
pip install dist/sqlite_etl_mini_project-0.0.0.tar.gz
```
I prefer this command because it installs the built package file just as anyone with the file would and thus, tests it.   
The installation is done into the following locations of the virtual environment:   
1. bin -> the script's reference command name can be found here.   
1. lib -> the scripts are installed here.   

**6\. Test CLI implementation**   
At this point, the package would have been installed in the virtual environment. The installed script is then tested at the CLI command prompt to see if everything is working as expected. In this case, the command to execute the script is ``sqlite_etl`` and a sample complete command is ``read_all_data "air_quality.db" indicator``. Please reference the manual for the full set of commands.   

**7\. Uninstall if necessary**   
If errors are encountered, uninstall the package and fix them. The package can be uninstalled with the following command:   
```
python -m pip uninstall sqlite_etl_mini_project 
```
Where ``sqlite_etl_mini_project`` is the name of the package.   

**8\. Repeat the last two cycles till installation is satisfactory**   
I repeated the last two processes until the package worked as expected.   


## Package Download
Please download the package here: [Package for distribution](dist/sqlite_etl_mini_project-2024.1.0.tar.gz)    


## References
**Official guides**   
\- [Official Setuptools Quick Start](https://setuptools.pypa.io/en/latest/userguide/quickstart.html)   
\- [Official Setuptools more details](https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html)   

**Alternative to the official guides**   
\- [Packageing Python - Carpentries Incubator](https://carpentries-incubator.github.io/python_packaging/03-building-and-installing.html)   
\- [Updated guide to setuptools](https://xebia.com/blog/an-updated-guide-to-setuptools-and-pyproject-toml/)   

**Useful for adding data and other files to the package**   
\- [Setuptools Datafiles](https://setuptools.pypa.io/en/latest/userguide/datafiles.html)   
\- [Setuptool Miscellaneous](https://setuptools.pypa.io/en/latest/userguide/miscellaneous.html)   