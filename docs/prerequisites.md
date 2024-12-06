## Prerequisites

Before starting the integration of SAP HANA with Databricks, ensure that the following prerequisites are met:
1. **Databricks Workspace Access**: You need an active Databricks account and workspace to import the necessary files and execute jobs.
2. For checkpoints it requires one catalog. Example: dev and prod. Pipeline_name is the only key
3. **Permissions**: Ensure you have the required permissions to upload files, create jobs, and execute tasks in the Databricks workspace.
4. **SAP HANA Access**: You need valid access credentials for the SAP HANA database, including the necessary username, password, and JDBC connection URL.
5. **Required Files**: Ensure that the following files are available:
   - The **wheel file** containing the SAP HANA connector logic.
   - The **ngdbc.jar** file for communication with the SAP HANA database.
6. **Cluster Availability**: Make sure you have access to an appropriate Databricks cluster where the job can be executed. The cluster must meet the necessary resource and environment requirements.

Once these prerequisites are fulfilled, you can proceed with the setup and data ingestion process.

