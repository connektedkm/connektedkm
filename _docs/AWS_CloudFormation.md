---
title: "CloudFormation"
permalink: /docs/AWS_CloudFormation/
excerpt: "Instructions for installing the theme for new and existing Jekyll based sites."
last_modified_at: 2019-08-20T21:36:18-04:00
toc: true
---


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