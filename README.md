
# End-to-End Azure Data Engineering

This project demonstrates an end-to-end Azure data engineering solution, starting from a local SQL database and culminating in Power BI reporting, all automated.

Thanks to [Mr. K Talks Tech](https://www.youtube.com/@mr.ktalkstech) for the project inspiration.

## Business Objective

This project serves as a learning opportunity for common data engineering practices, focusing on ETL pipeline techniques. The skills sharpened here are valuable for small to medium-sized businesses aiming to migrate their local data to the cloud.

![Insert Image](https://github.com/Nikhil-Pickle/End-to-End-Azure-Data-Engineering/blob/main/Screenshot%202023-10-11%20225825.png)

## Current Environment





- Utilized the AdventureWorks dataset from Microsoft.
- Set up an on-premises Microsoft SQL server on a personal computer.
- Imported the dataset using Microsoft SQL Server Management Studio.
- Created a new user profile, "nik."
- Saved "nik" profile's password credentials as a Secret in Azure Key Vault.

![image](https://github.com/Nikhil-Pickle/End-to-End-Azure-Data-Engineering/blob/main/Screenshot%202023-10-12%20011305.png)


## 1: Data Ingestion

Data ingestion from the on-premises SQL server to Azure SQL is accomplished via Azure Data Factory. The process involves:

1. Installation of Self-Hosted Integration Runtime.
2. Establishing a connection between Azure Data Factory and the local SQL Server.
3. Setting up a copy pipeline to transfer all tables from the local SQL server to the Azure Data Lake's "bronze" folder.


![Azure DataFactory](https://github.com/Nikhil-Pickle/End-to-End-Azure-Data-Engineering/blob/main/Screenshot_1.png)



## 2: Data Transformation

After ingesting data into the "bronze" folder, it is transformed following the medallion data lake architecture (bronze, silver, gold). Data transitions through bronze, silver, and ultimately gold, suitable for business reporting tools like Power BI.

Azure Databricks, using PySpark, is used for these transformations. Data initially stored in parquet format in the "bronze" folder is converted to the delta format as it progresses to "silver" and "gold." This transformation is carried out through Databricks notebooks:

1. Mount the storage.
2. Transform data from "bronze" to "silver" layer.
3. Further transform data from "silver" to "gold" layer.

![Databricks Notebooks](https://github.com/Nikhil-Pickle/End-to-End-Azure-Data-Engineering/blob/main/sdfasd.gif)

Azure Data Factory is updated to execute the "bronze" to "silver" and "silver" to "gold" notebooks automatically with each pipeline run.

![Completed Pipeline](https://github.com/Nikhil-Pickle/End-to-End-Azure-Data-Engineering/blob/main/Screenshot%202023-10-12%20002249.png)


## 3: Data Loading

Data from the "gold" folder is loaded into the Business Intelligence reporting application, Power BI. Azure Synapse is used for this purpose. The steps involve:

1. Creating a link from Azure Storage (Gold Folder) to Azure Synapse.
2. Writing stored procedures to extract table information as a SQL view.
3. Storing views within a server-less SQL Database in Synapse.

![image](https://github.com/Nikhil-Pickle/End-to-End-Azure-Data-Engineering/blob/main/Screenshot%202023-10-12%20012305.png)


## 4: Data Reporting

Power BI connects directly to the cloud pipeline using DirectQuery to dynamically update the database. A Power BI report is developed to visualize AdventureWorks dataset data, including sales, product information, and customer gender.

![power bi gif](https://github.com/Nikhil-Pickle/End-to-End-Azure-Data-Engineering/blob/main/gif.gif)



## 5: Final Pipeline Test

To verify the end-to-end pipeline, two new customers are added to the local SQL database server. If successful, the pipeline will update, and the Power BI report will dynamically show the new data. The total number of customers should increase from 847 to 849.

![completed](https://github.com/Nikhil-Pickle/End-to-End-Azure-Data-Engineering/blob/main/Screenshot%202023-10-12%20013527.png)  
Great success!


## Conclusion and Limitations

This project demonstrates the ability to create an end-to-end ETL cloud solution using Azure. Some considerations:

- The dataset used was small (7mb total, 800 rows). This was done to keep compute + storage costs low for myself.
- Multiple applications were employed for a relatively simple task.
- Given the dataset's simplicity, the project could have been managed entirely through Azure Data Factory, with data cleaning done downstream in Power BI.
- The inclusion of Azure Synapse and Databricks was for the sake of self-learning and emulating real-world business pipelines.
