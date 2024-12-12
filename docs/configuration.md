# Configuration
### How to Ingest Data from SAP HANA to Databricks

> **Note**: Make sure the [prerequisites](./prerequisites.md) are met.

For a **quick guide**, click [here](quickguide.md) to get started instantly.

---
## Table of Contents
1. [Step 1: Add the Provided Wheel File to Your Databricks Workspace](#step-1-add-the-provided-wheel-file-to-your-databricks-workspace)
2. [Step 2: Use the Wheel File to Deploy a New Job](#step-2-use-the-wheel-file-to-deploy-a-new-job)
    - [Adding Dependent Libraries](#adding-dependent-libraries)
    - [Setting Environment Parameters](#setting-environment-parameters)
3. [Step 3: Start a New Run with the Previously Created Job](#step-3-start-a-new-run-with-the-previously-created-job)
4. [Step 4: Check the Logs of the Run](#step-4-check-the-logs-of-the-run)
    - [Explanation of Log Outputs](#explanation-of-log-outputs)
---

## Step 1: Add the Provided Wheel File to Your Databricks Workspace

1. Import the wheel file into the workspace. The wheel file acts as a Databricks Asset Bundle, containing programming logic, metadata, and other necessary components for the connector to function.
   ![Import Wheel File](./images/import_wheel.png)
    > **Note**: Ensure you have the required permissions to execute tasks on the linked data table.

2. After selecting the corresponding wheel file, confirm the import by pressing **Import**.


## Step 2: Use the Wheel File to Deploy a New Job
![Create Job](./images/create_task.png)
> **Note** To make the injection of the data described in Step 2 easier, you can also use the default YAML file with your parameters. How this is done can be found [here](examples.md).

1. Go to **Databricks Workflows** and create a new job.

   A Job in Databricks enables you to run code (e.g., defined in a wheel file) on a cluster with specified environment parameters.

2. After creating the job, it should include a filled-out configuration with all required fields. Ensure the following elements are set:

### Required Parameters

| **Parameter**           | **Description**                                                      | **Example Value**                     |
|-------------------------|----------------------------------------------------------------------|---------------------------------------|
| **Task Name**           | Name of the job to run.                                              | `SAP_VBAP`                            |
| **Type**                | Type of package used for the connector (e.g., Notebook, Python Wheel)| `Python wheel`                        |
| **Package Name**        | Name of the package containing the wheel file logic.                 | `databricks-sap-connector`            |
| **Entry Point**         | Entry point containing the command for data extraction.              | `sap_table`                           |
| **Cluster**             | Cluster where the connector will run.                                | Use an appropriate available cluster  |
---

### Adding Dependent Libraries

To use the SAP HANA connector, two dependent libraries are needed:

1. The added wheel file.
2. An `ngdbc.jar` file, which contains the necessary information to access the SAP HANA database and enable communication.

    > **Note**: The `ngdbc.jar` file is typically located in your Volumes.

After completing these steps, the task will have the necessary information to execute the connector.
Now the connector just needs the specific parameters, to run your job.

---

### Setting Environment Parameters

Define the argument parameters needed for the run:

**Example Parameter Input:**

```plaintext
["--partitions", "40", "--table-name", "\"SAPHANADB.VBAP\"", "--dt-schema-name-checkpoints", "checkpointschema",  
"--dt-table-name", "vbap", "--dt-schema-name", "default", "--dt-catalog-name", "dev_jomach",  
"--columns", "*", "--primary-keys", "MANDT, VBELN", "--sap-hana-host", "...", "--scope", "sharedsecretvault",  
"--user-key", "hana-db-username", "--password-key", "hana-db-password", "--database-name", "All", "--limit", "1000", "--loglevel", "DEBUG"]
```

| Parameter                          | Type      | Description                                                                                                  |
|------------------------------------|-----------|--------------------------------------------------------------------------------------------------------------|
| `--partitions`                     | INTEGER   | Number of partitions to be used in the extraction                                                           |
| `--table-name`                     | TEXT      | Name of the SAP table to be extracted                                                                       |
| `--dt-schema-name-checkpoints`     | TEXT      | Schema where to store the checkpoints. This job needs create and write permissions on this schema           |
| `--dt-table-name`                  | TEXT      | Name of the Databricks destination table where the checkpoints are written to. Defaults to table_name.       |
| `--dt-schema-name`                 | TEXT      | Schema where to store the destination table. This job needs create and write permissions on this schema      |
| `--dt-catalog-name`                | TEXT      | Catalog where to store the destination table.                                                               |
| `--columns`                        | TEXT      | Comma-separated list of columns to select from the table.                                                   |
| `--primary-keys`                   | TEXT      | Comma-separated list of primary key columns.                                                                |
| `--incremental-field`              | TEXT      | Name of the incremental field to track changes on a table.                                                  |
| `--sap-hana-host`                  | TEXT      | Target URL of the HANA instance                                                                             |
| `--sap-hana-host-extra-options`    | TEXT      | Extra options for the target URL.                                                                           |
| `--scope`                          | TEXT      | Name of Databricks secret scope for retrieving SAP credentials                                              |
| `--user-key`                       | TEXT      | Key for SAP username in Databricks secret scope                                                             |
| `--password-key`                   | TEXT      | Key for SAP password in Databricks secret scope                                                             |
| `--database-name`                  | TEXT      | SAP database name                                                                                           |
| `--limit`                          | INTEGER   | Limit how much data will be extracted from SAP Table. Defaults to 0 (no limit)                              |
| `--loglevel`                       | TEXT      | If true, will print debug information                                                                       |

> **After completing these steps, it should look like this:**

![Final Job](./images/parameter_input.png)

## Step 3: Start a New Run with the Previously Created Job

Navigate to **Workflows > Jobs > {your_job_name} > Runs** and start a new run to execute the created job.

This will automatically initiate a new run of the task defined in the task section.  

---

## Step 4: Check the Logs of the Run

1. Go to **Job Runs** and select the run you just executed.  
2. Here, you can review the outputs that were printed to the console during execution.  

 ![Output](./images/test_run_output.png)
---

### Explanation of Log Outputs

| Output                            | Meaning                                                                 |
|-----------------------------------|-------------------------------------------------------------------------|
| **No Checkpoint found**           | The system tried to load a checkpoint for incremental data extraction, but none were found. |
| **Loading table**                 | Indicates the system is attempting to load the corresponding SAP HALA table. |
| **Bounds**                        | Displays the shape of the table being processed.                       |
| **Writing into table**            | Specifies the table where the extracted data is being written.         |
| **Pipeline finished: None -> N**  | Indicates the pipeline has finished extracting data, starting from checkpoint `None` and is now at row `N`. |
