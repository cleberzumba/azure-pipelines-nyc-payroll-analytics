
# Data Integration Pipelines for NYC Payroll Data Analytics

Author: [Cleber Zumba](https://github.com/cleberzumba)

Last Updated: January 22, 2025

## What is this project for?

New York City would like to develop a data analytics platform on Azure Synapse Analytics to achieve two main goals:

1- Analyze how the city’s financial resources are allocated and how much of the city’s budget is being dedicated to overtime.

2- Make the data available to the interested public to show how the city’s budget is being spent on salaries and overtime for all city employees.

As a Data Engineer, I need to create high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operation. The source data resides in Azure Data Lake and needs to be processed in a NYC data warehouse. The source datasets consist of CSV files with employee master data and monthly payroll data entered by various city agencies.

![imagem](images/DB-schema.jpg)

I used Azure Data Factory to create Data views in Azure SQL DB from the source data files in DataLake Gen2. Then I built the dataflows and pipelines to create payroll aggregated data that will be exported to a target directory in DataLake Gen2 storage over which Synapse Analytics external table is built. At a high level, my pipeline will look like the one below

![imagem](images/pipeline-overview.jpg)


## Project Environment

This project was done in the Azure Portal, using several Azure resources, including:

  - Azure Data Lake Gen2
  - Azure SQL DB
  - Azure Data Factory
  - Azure Synapse Analytics


## Prepare the Data Infrastructure

### 1.Create the data lake and upload data

Create an Azure Data Lake Storage Gen2 (storage account) and associated storage container resource

![imagem](images/create-storage-account.jpg)

![imagem](images/create-container.jpg)

Create three directories in this storage container named

- `dirpayrollfiles`
- `dirhistoryfiles`
- `dirstaging`

`dirstaging` will be used by the pipelines we will create as part of the project to store staging data for integration with Azure Synapse.

Upload these files from the project data to the dirpayrollfiles folder

- EmpMaster.csv
- AgencyMaster.csv
- TitleMaster.csv
- nycpayroll_2021.csv

Upload this file (historical data) from the project data to the `dirhistoryfiles` folder

- nycpayroll_2020.csv

![imagem](images/create-folders.jpg)

![imagem](images/upload-files-in-dirpayrollfiles.jpg)

![imagem](images/upload-files-in-dirhistoryfiles.jpg)

### 2. Create an Azure Data Factory Resource

![imagem](images/create-data-factory.jpg)

### 3. Create a SQL Database

![imagem](images/create-sql-database.jpg)



### 4. Create a Synapse Analytics workspace

![imagem](images/create-sunapse-analytics-workspace.jpg)


### 5. Create summary data external table in Synapse Analytics workspace

The external table references the `dirstaging` directory of DataLake Gen2 storage for staging payroll summary data.

![imagem](images/create-external-table.jpg)


### 6. Create master data tables and payroll transaction tables in SQL DB

- Employee Master Data table
- Job Title Table
- Agency Master table
- Payroll 2020 transaction data table
- Payroll 2021 transaction data table
- Payroll summary data table

![imagem](images/create-tables-in-sql-database.jpg)


## Create Linked Services

### 1.Create a Linked Service for Azure Data Lake and a Linked Service to SQL Database that has the current (2021) data.

![imagem](images/create-linked-service.jpg)


## Create Datasets in Azure Data Factory

### 1. Create the datasets for the 2021 Payroll file on Azure Data Lake Gen2 and for all the data tables in SQL DB and datasets for destination (target) table in Synapse Analytics

![imagem](images/create-datasets-in-azure-data-factory.jpg)


## Create Data Flows

In Azure Data Factory, I created data flow to load 2020 Payroll data from Azure DataLake Gen2 storage to SQL db table created earlier

![imagem](images/data-flow.jpg)


## Data Aggregation and Parameterization

In this step, I extracted the 2021 year data and historical data, merge, aggregate and store it in DataLake staging area which will be used by Synapse Analytics external table. The aggregation will be on Agency Name, Fiscal Year and TotalPaid.

![imagem](images/create-aggregate-dataflow-Data-Factory.jpg)


## Pipeline Creation

I created a pipeline to load data from Azure DataLake Gen2 storage into SQL Database for individual datasets, perform aggregations, and store the summarized results back into the SQL Database target table and Datalake temporary storage directory, which will be consumed by Synapse Analytics via CETAS.

![imagem](images/create-pipeline-nyc-payroll.jpg)


## Trigger and Monitor Pipeline

![imagem](images/successful-pipeline-run.jpg)


## Verify Pipeline run artifacts

### 1. Query data in SQL DB summary table (destination table). This is one of the sinks defined in the pipeline.

![imagem](images/query-db.jpg)


### 2. Check the dirstaging directory in Datalake if files got created. This is one of the sinks defined in the pipeline

![imagem](images/files-saved-dirstaging.jpg)


### 3. Query data in Synapse external table that points to the dirstaging directory in Datalake.

![imagem](images/query-synapse.jpg)


## Conclusion

The Azure Pipelines NYC Payroll Analytics project demonstrated the power of using cloud tools such as Azure Data Factory and Synapse Analytics to ingest, transform, and analyze data. By integrating technologies such as GitHub for versioning and collaboration, it was possible to create a scalable and well-structured pipeline to process payroll data for the City of New York.

This solution highlights the potential of data analytics to provide valuable insights that can help inform strategic decision-making and optimize resource management.
