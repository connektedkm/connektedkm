## **Writing Example 1 - How to create an AWS VPC with DNS enabled using CloudFormation**

The following instructions detail how to create an AWS VPC with DNS enabled using a CloudFormation template. 


### **Before you begin**

You must have:



* An AWS account
* Access to a text editor such as Sublime Text or Notepad++


### **Create a CloudFormation template**

Resources to be created by AWS must first be defined in a CloudFormation template, which is a JSON or YAML formatted text file.

Choose your preferred format from the two, and paste one of the code snippets below into a new file in your text editor:

**JSON**


```
{
   "AWSTemplateFormatVersion": "2010-09-09",
   "Description": "AWS CloudFormation VPC Template: Template to create VPC with DNS enabled.",
   "Resources": {
      "MyCompanyVPC": {
         "Type": "AWS::EC2::VPC",
         "Properties": {
            "CidrBlock": "10.0.0.0/16",
            "EnableDnsSupport": true,
            "EnableDnsHostnames": true,
            "Tags": [{"Key": "Name", "Value": "PrimaryVPC" }]                  
         }
      }
   }
}
```


**YAML**


```
AWSTemplateFormatVersion: '2010-09-09'
Description: 'AWS CloudFormation VPC Template: Template to create VPC with DNS enabled.'
Resources:
  MyCompanyVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
      EnableDnsSupport: true
      EnableDnsHostnames: true
      Tags:
      - Key: Name
        Value: PrimaryVPC
```


Whichever format is chosen, the code used in both examples will create a VPC with the following settings:


<table>
  <tr>
   <td><strong>Setting</strong>
   </td>
   <td><strong>Value</strong>
   </td>
  </tr>
  <tr>
   <td><strong>Logical ID</strong>
   </td>
   <td>MyCompanyVPC
   </td>
  </tr>
  <tr>
   <td><strong>CIDR Block</strong>
   </td>
   <td>10.0.0.0/16
   </td>
  </tr>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td>PrimaryVPC
   </td>
  </tr>
</table>


If required, modify these values in your text editor to suit your environment. 

**Note: **The Logical ID and CIDR block **cannot** be changed once the VPC has been created.

Save the file once your template is configured correctly.


### **Create a VPC from the CloudFormation template**

CloudFormation templates are converted to CloudFormation stacks upon upload to AWS. CloudFormation stacks then provision and configure the resources defined in the template file.

Follow these steps to create the VPC from your CloudFormation template:



1. Sign in to your AWS account using the web console via [aws.amazon.com](aws.amazon.com) 
2. Ensure you are in the correct AWS Region using the navigation bar. Your VPC will be created in this Region.
3. Open the **CloudFormation_ _**service page.
4. Select **Create stack.**
5. Select **Upload a template file** from the **Specify template** menu. 
6. Select **Choose file** and upload your template file.
7. Click **Next**, enter a logical **Stack name** and click **Next** again
8. Accept the default stack options.
9. Review the stack details and select **Submit.**

CloudFormation will now direct you to the **Events_ _**tab, a log of the steps taken by AWS to create the defined resources which includes a timestamp and status: 



* A status of _CREATE_IN_PROGRESS_ will be displayed during the creation of the VPC.
* A status of _CREATE_COMPLETE_ will be displayed to confirm the successful creation of the VPC.

The **Events** page automatically refreshes every minute. Click the **Resources** tab once the stack reaches a _CREATE_COMPLETE _status. 

The** Physical ID** displayed here is a unique identifier assigned to the VPC by AWS. This takes the form of **vpc-** followed by a unique 17 character string, for example,  [vpc-0bf8c011c81218c2d](https://eu-west-2.console.aws.amazon.com/vpc/home?region=eu-west-2#VpcDetails:VpcId=vpc-0bf8c011c81218c2d).

Click the Physical ID to access the newly created VPC. 

The **Details** tab will allow you to confirm that the VPC Name, IPv4 CIDR and DNS configuration match the settings defined in your original CloudFormation template. 




## **Writing Example 2 - Network Address Translation**


### **What it is**

Network Address Translation (NAT) is the process of mapping one or more private IPv4 addresses to one or more public IPv4 addresses. Its primary function is to provide devices in a local network with the capability to communicate with an external network, most commonly the Internet, whilst preserving the public IPv4 address space.


### **History**

NAT provides a solution to _IPv4 address exhaustion_; a term used to describe the depletion of unique IPv4 addresses caused by widespread Internet adoption, which was not accounted for during the design phase of the web’s initial architecture.

A total of 4.3 billion unique IPv4 addresses were made available when engineers opted for a 32-bit address space in 1977. Whilst this number matched the global population at the time, the explosive growth in global connectivity in the years that followed made it clear assigning a unique public IPv4 address to every device was impossible.

Although IPv6’s 128-bit address system was introduced to also contend with the same issue, adoption has been slow, meaning NAT is still widely used and an important concept to understand.


### **How NAT works**

NAT can be implemented in three different ways:



1. **Static NAT** - one-to-one mapping of a single private IPv4 address to a single public IPv4 address.
2. **Dynamic NAT** - mapping of a single private IPv4 address to a public IPv4 address allocated from a pool of public addresses.
3. **Port Address Translation** - mapping of multiple private IPv4 addresses to a single public IP address using port numbers.

Port Address Translation is the most common application of NAT. The process is usually handled by physical devices, with routers usually performing the function in both home and corporate networks. 

The diagram below gives a visual representation of how NAT works, showing the flow of traffic in a common home network, consisting of two laptops and one smartphone behind a router. 

The configuration is as follows:



* All three devices have been assigned a private IPv4 address by the router
* The router has a public IP address of 85.241.20.11

Each device is attempting to access [https://www.google.com](https://www.google.com) from the local network. 



<p id="gdcalert1" ><span style="color: red; font-weight: bold">>>>>>  gd2md-html alert: inline image link here (to images/image1.png). Store image on your image server and adjust path/filename/extension if necessary. </span><br>(<a href="#">Back to top</a>)(<a href="#gdcalert2">Next alert</a>)<br><span style="color: red; font-weight: bold">>>>>> </span></p>


![alt_text](images/image1.png "image_tooltip")


The successful transmission of data from source to destination and back again follows this order:



1. All three end devices send data to Google, with a destination IP address of **8.8.8.8** and a destination port of **443**.
2. The router receives the data and adds the private source IP address and private source port of each device to its NAT table.
3. The router translates each private source IP address into its own public IP address and each private source port into a unique public port and adds this information to the NAT table.
4. The router forwards the data to **8.8.8.8:443.**
5. The return traffic for each end device arrives back at the router, with a destination IP address of **85.241.20.11** (the public IP address of the NAT device) and a destination port of the device's public port. 
6. The router translates the public IP address and public port of each data packet back to the private IP address and public port numbers stored in the NAT table before delivering the data to its final destination. 


### **Security Benefit of NAT**

NAT adds an additional layer of security to home and corporate networks by virtue of its obfuscation of private IP addresses. The use of a single IP public address in front of multiple devices allows incoming traffic to be filtered by the NAT device before it can reach the end destination, allowing policies and rules to be put in place in order to filter or block unwanted traffic.




## **Writing Example 3 - Use Python to export data from a MySQL database to a CSV using a Pandas DataFrame**

This guide provides instructions on how to use Python to connect to a MySQL database, convert a set of database table entries to a Pandas DataFrame and finally, export the results to a CSV file.


### **Prerequisites**

Python 3 - the latest version of Python

_pip_ - a package installer for Python

Access to a MySQL database


### **Installing Python**

Follow the steps outlined in Python’s official release notes to download and install the latest release for your operating system:

**Mac **- [https://www.python.org/downloads/macos/](https://www.python.org/downloads/macos/) 

**Windows **- [https://www.python.org/downloads/windows/](https://www.python.org/downloads/windows/) 

**Linux** - [https://www.python.org/downloads/source/](https://www.python.org/downloads/source/) 

**Other** - [https://www.python.org/download/other/](https://www.python.org/download/other/) 

**Note:** If you already have Python installed, ensure you are running version 3.7 or later at a minimum. At the time of writing the latest version of Python was 3.11.0.


### **Installing pip**

 

pip is installed with the official version of Python. If you require a standalone installation, follow the instructions outlined in the official documentation that can be found here: [https://pip.pypa.io/en/stable/installation/](https://pip.pypa.io/en/stable/installation/) 

Check which version of pip is installed by running the command:


```
pip --version 
```


**Note: **At the time of writing the latest version of pip was 22.3.1.


### **Dependencies** \


The following libraries will need to be installed in order to complete the task:



* **Pandas** - a popular Python library used to analyse data
* **MySQLConnector **- a self-contained Python driver used to communicate with MySQL databases

Use pip to install both libraries by running these two commands: \
 \



```
pip install pandas

pip install mysql.connector
```



###  \
**Running the script \
 \
**The Python script below will complete the following steps:



1. Connect to a local MySQL database named **Users, **authenticating with a defined username and password.
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


 \



```
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