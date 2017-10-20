# aws-connect-with-active-directory

**Evaluate for Amazon Connect** is a Agent Evaluation product that has been integration with **Amazon Connect**. For a fuller explanation of our **Evaluate** please click [here](http://www.qualtrak.com). This GitHub OSS repository contains two templates that must both be deployed together.  We have also included supporting materials whcih can be found near the end of this file.  The order of deployment is import.  This is covered shortly.  Deploying both templates will stand up everything required for your customers using **Amazon Connect** to have a Quality Monitoring product.

The use-case that these templates deal with fits this configuration:
- Your customer is using **Amazon Connect**
- Your customer is using Active Directory
- Your customer has already set up a Kinesis Stream that streams the CTRs to either RedShift, S3 or other.

## CloudFormation Templates:

- `KStreamToKFirehoseToES` - This template stands up a Kinesis Firehose that captures the CTRs streamed from the current Kinesis stream, and then stores these CTRs in an ElasticSearch search engine.
- `E4AC` - This template stands up an EC2 instance with our product installed along with and RDS (MSSQLServer 2016 Express Edition) and S3.

Each template will prompt the user for various information. These prompts have been documented below as properties.  The order of deployment is important.  First, you must deploy `KStreamToKFirehoseToES` then you must deploy `KStreamToKFirehoseToES`.

## The `KStreamToKFirehoseToES` CFN Template:

#### Properties:

##### Amazon Connect Configuration

- `Which Stream to use?` - The name of your existing Kinesis Stream
- `Which Role to use?` - The name of your existing Kinesis Stream Role
- `Where to write logs?` - The name of the S3 Bucket that Amazon Connect uses to store its call recordings

##### ElasticSearch Configuration

- `What name to use?` - This is the name for the ElasticSearch Domain
- `Allow access from?` - VPC CIDR Block (eg 10.0.0.0/16)

## The `E4AC` CFN Template

This template uses `E4AC-ES-DomainName` & `E4AC-S3Bucket` **Export** values from the **KStreamToKFirehoseToES** Template.

#### Properties:

##### EC2 Configuration

- `Which Key should be used?` - Name of an existing EC2 KeyPair
- `What EC2 size should be used?` - Amazon EC2 instance type, defaulted to m4.Large
- `Which VPC should this be deployed to?` - The VPC where you want this be deployed
- `SubnetId` - 
- `RDP from` - Lockdown RDP access to the bastion host (default can be accessed from anywhere)

##### Amazon RDS Configuration

- `Application database master username` - The database admin account username
- `Application database master password` - The database admin account password
- `Application database username` - The database non-admin account username
- `Application database password` - The database non-admin account password

##### S3 Configuration

- `Retain Evaluate S3?` - After you have deleted this Stack, do you want to retain or delete the Evaluate S3 where all your Evaluation Attachments are stored?

##### Active Directory Configuration

- `Domain DNS` - Fully qualified domain name (FQDN) of the forest root domain e.g. corp.example.com
- `Domain IPs` - Comma separated list of IP addresses of AD Domain
- `Domain Administrator username` - User name for the account that will be added as Domain Administrator. This is separate from the default EC2 Administrator account
- `Domain Administrator password` - "Password of the Domain Administrator's account. This is separate from the default EC2 Administrator account
- `DomainNetBiosName` - Netbios name for the domain, e.g. MYCOMPANY.local

##### Exports

These are the URLs you can browse to. Please note, both the `E4AC-Tlm-Url` & `E4AC-status-Url` use Windows Authentication and not Active Directory

- `E4AC-Url` - Url for Evaluate for Amazon Connect web application (Active Directory)
- `E4AC-Tlm-Url` - Url for Tenant Management System web application (Windows Authentication)
- `E4AC-status-Url` - Url for Cluster Status web application (Windows Authentication)



## Supporting materials

- Solution schematic [open jpg](https://s3.amazonaws.com/Qualtrak/E4AC%20Architecture.jpg)
- Setup Guide [open pdf](https://s3.amazonaws.com/Qualtrak/E4AC%20Setup.pdf)
- Quick Start Guide [open pdf](https://e4ac.s3.amazonaws.com/QuickStartGuide.pdf)
- Product Summary [open jpg](https://s3.amazonaws.com/Qualtrak/E4AC%20Product%20Summary.jpg)
- You can take advantage of free training by clicking [here](https://qualtrak.atlassian.net/servicedesk/customer/portal/8)

----------

**Qualtrak** is a **AWS Standard APN Technology Partner**

![alt](https://s3.amazonaws.com/Qualtrak/logo-qualtrak.jpg)

![alt](https://s3.amazonaws.com/Qualtrak/AWS-Technology-Partner-Logo.png)


