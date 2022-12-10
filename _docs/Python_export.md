---
title: "Python DataFrames"
permalink: /docs/Python_export/
excerpt: "Instructions for installing the theme for new and existing Jekyll based sites."
last_modified_at: 2019-08-20T21:36:18-04:00
toc: true
---

## **Use Python to export data from a MySQL database to a CSV using a Pandas DataFrame**

This guide provides instructions on how to use Python to connect to a MySQL database, convert a set of database table entries to a Pandas DataFrame and finally, export the results to a CSV file.

### **Prerequisites**

Python 3 - the latest version of Python

_pip_ - a package installer for Python

Access to a MySQL database

### **Installing Python**

Follow the steps outlined in Python’s official release notes to download and install the latest release for your operating system:

**Mac**- [https://www.python.org/downloads/macos/](https://www.python.org/downloads/macos/) 

**Windows**- [https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/) 

**Linux**- [https://www.python.org/downloads/source/](https://www.python.org/downloads/source/) 

**Other**- [https://www.python.org/download/other/](https://www.python.org/download/other/) 

**Note:** If you already have Python installed, ensure you are running version 3.7 or later at a minimum. At the time of writing the latest version of Python was 3.11.0.
{: .notice--warning}

### **Installing pip**

pip is installed with the official version of Python. If you require a standalone installation, follow the instructions outlined in the official documentation that can be found here: [https://pip.pypa.io/en/stable/installation/](https://pip.pypa.io/en/stable/installation/) 

Check which version of pip is installed by running the command:


```bash
pip --version 
```

**Note:** At the time of writing the latest version of pip was 22.3.1.
{: .notice--warning}

### **Dependencies** 

The following libraries will need to be installed in order to complete the task:

* **Pandas**- a popular Python library used to analyse data
* **MySQLConnector**- a self-contained Python driver used to communicate with MySQL databases

Use pip to install both libraries by running these two commands: 

```bash
pip install pandas

pip install mysql.connector
```

### Running the script

The Python script below will complete the following steps:
1. Connect to a local MySQL database named **Users**, authenticating with a defined username and password.
2. Run a query to access all records from the **userdetails** table located in the **Users** database once a connection has been established. 
3. Create a Pandas DataFrame from the query results in step 2.
4. Print an error if there are any issues connecting to the database.
5. Export the Pandas DataFrame results to a CSV file saved to local disk with a filename of **usersexport.csv** 

The following variables are defined in the script and should be modified to suit your environment as required:


<table>
  <tr>
   <td><strong>Detail</strong>
   </td>
   <td><strong>Description</strong>
   </td>
   <td><strong>Code</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Host</strong>
   </td>
   <td>Hostname of the MySQL server. Can be defined using hostname or IP address
   </td>
   <td><code>localhost</code>
   </td>
  </tr>
  <tr>
   <td><strong>Database</strong>
   </td>
   <td>Name of the database to connect to
   </td>
   <td><code>Users</code>
   </td>
  </tr>
  <tr>
   <td><strong>User</strong>
   </td>
   <td>Username of account to access the database
   </td>
   <td><code>admin</code>
   </td>
  </tr>
  <tr>
   <td><strong>Password</strong>
   </td>
   <td>Password of the account used to access the database
   </td>
   <td><code>admin</code>
   </td>
  </tr>
  <tr>
   <td><strong>Table name</strong>
   </td>
   <td>Name of database table containing data to be exported
   </td>
   <td><code>userdetails</code>
   </td>
  </tr>
  <tr>
   <td><strong>CSV Filename</strong>
   </td>
   <td>File name of the exported CSV file saved to your local disk
   </td>
   <td><code>usersexport.csv</code>
   </td>
  </tr>
</table>


```python
import mysql.connector as connector 
import pandas as pd 
try: 
  mydb = connector.connect(host="localhost", database = 'Users',user="admin",passwd="admin") 
  query = "Select * from userdetails;" 
  result_dataFrame = pd.read_sql(query,mydb) 
  mydb.close() 

except Exception as e: 
  mydb.close() 
  print(str(e))

result_dataFrame.to_csv('usersexport.csv')
```

