# Comprehensive Azure Data Engineering Solution

This project showcases a complete Azure data engineering solution, beginning with a local SQL database and concluding with Power BI reporting, all automated. Credit to [Mr. K Talks Tech](https://www.youtube.com/@mr.ktalkstech) for inspiring this project.

## Business Objective

This project serves as an educational opportunity for mastering common data engineering practices, with a focus on ETL pipeline techniques. The skills honed here are invaluable for small to medium-sized businesses looking to migrate their local data to the cloud.

![Insert Image](https://github.com/PEPE0211/Azure_DataEngineering_project/blob/main/images/1.png)

## Current Environment

Utilized the Microsoft AdventureWorks dataset. We set up an on-premises Microsoft SQL server on a personal computer and imported the dataset using Microsoft SQL Server Management Studio. We also created a new user profile, "nik," and stored "nik" profile's password credentials securely as a Secret in Azure Key Vault.

![image](https://github.com/PEPE0211/Azure_DataEngineering_project/blob/main/images/3.png)

## 1: Data Ingestion

Data ingestion from the on-premises SQL server to Azure SQL is achieved through Azure Data Factory, involving the following steps:

1. Installation of Self-Hosted Integration Runtime.
2. Establishing a connection between Azure Data Factory and the local SQL Server.
3. Setting up a copy pipeline to transfer all tables from the local SQL server to the Azure Data Lake's "bronze" folder.

![Azure DataFactory](https://github.com/PEPE0211/Azure_DataEngineering_project/blob/main/images/2.png)

## 2: Data Transformation

After ingesting data into the "bronze" folder, it undergoes transformation following the medallion data lake architecture (bronze, silver, gold). Data progresses through bronze, silver, and ultimately gold, making it suitable for business reporting tools like Power BI.

Azure Databricks, using PySpark, is employed for these transformations. Data initially stored in parquet format in the "bronze" folder is converted to the delta format as it advances to "silver" and "gold." These transformations are executed through Databricks notebooks:

1. Mount the storage.
2. Transform data from "bronze" to "silver" layer.
3. Further transform data from "silver" to "gold" layer.

![Databricks Notebooks](https://github.com/PEPE0211/Azure_DataEngineering_project/blob/main/images/3.gif)

Azure Data Factory is updated to automatically run the "bronze" to "silver" and "silver" to "gold" notebooks with each pipeline execution.

![Completed Pipeline](https://github.com/PEPE0211/Azure_DataEngineering_project/blob/main/images/4.png)

## 3: Data Loading

Data from the "gold" folder is loaded into the Business Intelligence reporting application, Power BI, with Azure Synapse being the bridge. The steps involve:

1. Creating a link from Azure Storage (Gold Folder) to Azure Synapse.
2. Developing stored procedures to extract table information as a SQL view.
3. Storing views within a server-less SQL Database in Synapse.

![image](https://github.com/PEPE0211/Azure_DataEngineering_project/blob/main/images/5.png)

## 4: Data Reporting

Power BI connects directly to the cloud pipeline using DirectQuery to dynamically update the database. A Power BI report is designed to visualize AdventureWorks dataset data, including sales, product information, and customer gender.

![power bi gif](https://github.com/PEPE0211/Azure_DataEngineering_project/blob/main/images/6.gif)

## 5: Final Pipeline Test

To validate the end-to-end pipeline, two new customers are added to the local SQL database server. If successful, the pipeline will update, and the Power BI report will dynamically show the new data. The total number of customers should increase from 847 to 849.

![completed](https://github.com/PEPE0211/Azure_DataEngineering_project/blob/main/images/7.png)  
Successful execution!

## Conclusion and Limitations

This project showcases the ability to create a comprehensive ETL cloud solution using Azure. Some considerations:

- The dataset used was relatively small (7mb total, 800 rows) to keep compute and storage costs low.
- Multiple applications were employed for what could be considered a simple task.
- Given the dataset's simplicity, the project could have been managed entirely through Azure Data Factory, with data cleaning performed downstream in Power BI.
- The inclusion of Azure Synapse and Databricks was for the sake of self-learning and emulating real-world business pipelines.
