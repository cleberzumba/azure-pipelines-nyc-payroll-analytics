
under construction...

# Data Integration Pipelines for NYC Payroll Data Analytics

Author: [Cleber Zumba](https://github.com/cleberzumba)

Last Updated: January 20, 2025

## What is this project for?

New York City would like to develop a data analytics platform on Azure Synapse Analytics to achieve two main goals:

1- Analyze how the city’s financial resources are allocated and how much of the city’s budget is being dedicated to overtime.

2- Make the data available to the interested public to show how the city’s budget is being spent on salaries and overtime for all city employees.

As a Data Engineer, I need to create high-quality data pipelines that are dynamic, can be automated, and monitored for efficient operation. The project team also includes the city’s quality assurance specialists who will test the pipelines to find any errors and improve the overall data quality.

The source data resides in Azure Data Lake and needs to be processed in a NYC data warehouse. The source datasets consist of CSV files with employee master data and monthly payroll data entered by various city agencies.

## Project Environment

This project was done in the Azure Portal, using several Azure resources, including:

  - Azure Data Lake Gen2
  - Azure SQL DB
  - Azure Data Factory
  - Azure Synapse Analytics
