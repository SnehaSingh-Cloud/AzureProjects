# Automated Sales Data Ingestion Pipeline using Azure Data Factory

## Project Overview

This project demonstrates the implementation of an automated cloud-based data pipeline using Azure Data Factory (ADF) to efficiently ingest daily sales data. The pipeline is designed to move raw sales reports from a staging area in Azure Blob Storage to a structured raw data zone within Azure Data Lake Storage Gen2, ensuring data organization and availability for downstream analytical processes.

## Problem Solved

Organizations often receive new data periodically (e.g., daily sales reports) in various formats within temporary storage locations (landing zones). Manually moving, organizing, and preparing this data for analysis is time-consuming, prone to errors, and not scalable. This project addresses the need for an automated, reliable, and scalable solution to handle continuous data ingestion.

## Solution Architecture & Components

The solution leverages key Azure services to create a robust data pipeline:

1.  Azure Blob Storage (Landing Zone):
    -   Acts as the source for incoming daily sales CSV files.
    -   Container: `landingzone`

2.  Azure Data Lake Storage Gen2 (Raw Data Zone):
    -   Serves as the destination for the ingested and organized sales data.
    -   Container: `raw-sales`
    -   Data is stored in a date-based hierarchical structure (e.g., `raw-sales/sales/YYYY/MM/DD/`).

3.  Azure Data Factory (ADF):
    -   The central orchestration and ETL tool for building and managing the data pipeline.

### ADF Components:

-   Linked Services:
    -   `snehasaleslanding_zone`: Connects ADF to the Azure Blob Storage account.
    -   `snehadata_lake`: Connects ADF to the Azure Data Lake Storage Gen2 account.

-   Datasets:
    -   `salescsv`: Defines the structure and location of source sales data in Azure Blob Storage. Configured with a wildcard file name (`daily_sales_*.csv`) to dynamically pick up daily files.
    -   `dsrawsales_dest`: Defines the destination for processed sales data in ADLS Gen2. Uses dynamic expressions to create date-based folder paths and name output files (e.g., `sales_ingested_YYYYMMDD.csv`).

-   Pipeline (`pipeline1` or similar):
    -   Contains a single `CopySalesDataToRaw` activity.
    -   Source: Uses the `salescsv` dataset, enabling wildcard-based file selection.
    -   Sink: Uses the `dsrawsales_dest` dataset. The `Copy behavior` is set to `Merge files` to consolidate multiple daily source files into a single output file per pipeline run.
    -   Mapping: Ensures correct schema transfer from source to sink.

## Key Features

-   Automated Data Movement: Eliminates manual efforts for data transfer.
-   Dynamic File Handling: Automatically picks up new daily files using wildcards.
-   Organized Data Lake: Stores data in a clean, date-partitioned structure.
-   Data Consolidation: Merges multiple source files into a single output file.
-   Scalable Solution: Built on Azure's scalable cloud infrastructure.
-   Version Controlled: Project code is integrated with GitHub for source control and collaboration.

## Technologies Used

-   Azure Data Factory (ADF)
-   Azure Blob Storage
-   Azure Data Lake Storage Gen2
-   Git / GitHub (for version control)
-   CSV (data format)

## Setup and Running

To set up and run this project, you would typically:

1.  Provision Azure resources: Azure Data Factory, Azure Blob Storage, Azure Data Lake Storage Gen2.
2.  Deploy the ADF linked services, datasets, and pipelines from this GitHub repository to your ADF instance.
3.  Upload sample `daily_sales_YYYYMMDD.csv` files to the `landingzone` container in your Azure Blob Storage.
4.  Trigger the ADF pipeline (manually or via a schedule) to initiate data ingestion.
5.  Verify the ingested data in the `raw-sales` container of your ADLS Gen2.

## Future Enhancements

-   Add data transformation (e.g., cleaning, aggregation) using ADF Data Flows or Databricks.
-   Implement robust error handling, logging, and alerting mechanisms.
-   Set up recurring schedules (triggers) for automated daily runs.
-   Integrate with downstream data warehousing (e.g., Azure Synapse Analytics) or analytics tools.

## Author

-   Sneha Singh
    -   [https://github.com/SnehaSingh-Cloud/AzureProjects](https://github.com/SnehaSingh-Cloud/AzureProjects)
