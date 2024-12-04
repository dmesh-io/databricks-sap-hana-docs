## Usage

### How to Ingest Data from SAP HANA to Databricks

#### Step 1: Add the Provided Wheel File to Your Databricks Workspace

1. Navigate to the directory where you want to run the connector (e.g., `SAP_Connector_New`).
2. Import the wheel file into the workspace. The wheel file acts as a Databricks Asset Bundle, containing programming logic, metadata, and other necessary components for the connector to function.

![Import Wheel File](./images/import_wheel.png)

3. After selecting the corresponding wheel file, confirm the import by pressing **Import**.

> **Note**: Ensure you have the required permissions to execute tasks on the linked data table.

#### Step 2: Use the Wheel File to Deploy a New Job

1. Go to **Databricks Workflows** and create a new job.

![Create Job](./images/create_task.png)

   A Job in Databricks enables you to run code (e.g., defined in a wheel file) on a cluster with specified environment parameters.

2. After creating the job, it should look like this:

![Created Job](./images/created_job.png)

3. To configure the job, set the following elements:

### Required Parameters

| **Parameter**     | **Description**                                                                           | **Value**                          |
|-------------------|-------------------------------------------------------------------------------------------|------------------------------------|
| **Task Name**     | Name of the job to run.                                                                   | Choose individually, e.g., `test` |
| **Type**          | Type of package used for the connector (e.g., Notebook, Python Wheel).                    | `Python wheel`                     |
| **Package Name**  | Name of the package containing the wheel file logic (defined in the Makefile file).        | `databricks-sap-connector`         |
| **Entry Point**   | Entry point containing the command for data extraction.                                   | `sap_table`                        |
| **Cluster**       | Cluster where the connector will run.                                                     | Use an appropriate available cluster |

---

### Adding Dependent Libraries

To use the SAP HANA connector, two dependent libraries are needed:
1. The added wheel file.
2. An `ngdbc.jar` file, which contains the necessary information to access the SAP HANA database and enable communication.

1. Press **Add** under **Dependent Libraries** and select the wheel file in your workspace.

![Add Wheel File](./images/add_wheel_as_dl.png)

2. Repeat the same steps for the `ngdbc.jar` file. 
   > **Note**: The `ngdbc.jar` file is typically located in your Volumes.

After completing these steps, the task will have the necessary information to execute the connector.

---

### Setting Environment Parameters

Define the argument parameters needed for the run:

![Input Parameters](./images/parameter_input.png)

| **Parameter** | **Description**                                                                                 | **Value**                        |
|---------------|-------------------------------------------------------------------------------------------------|----------------------------------|
| **p**         | Number of partitions to be used in the extraction                                               | 40                               |
| **t**         | Name of the SAP table to be extracted                                                           | `"\"databricks-sap-connector\"`  |
| **tpf**       | Full path of the SAP table to be extracted                                                      | `"databricks-sap-connector"`     |
| **sc**        | Schema of the Databricks checkpoint table with permissions to write table cehckpoint table into | `dev_mabras.checkpointschema`    |
| **td**        | Name of the Databricks destination table where the checkpoints are written to                   | e.g., `example_sap.adplapsa2`    |
| **c**         | Name of the Databricks destination table where the checkpoints are written to                   | e.g., `PLANT, RECORDMODE`        |
| **pk**        | Comma-separated list of primary key columns                                                     | e.g., `PLANT`                    |
| **if**        | Name of the incremental field to track changes                                                  | e.g., `ROW_IDENTIFIER`           |
| **url**       | Target URL of the HANA instance                                                                 | e.g., `"jdbc:sap://test_url.ex"` |
| **s**         | Name of Databricks secret scope for retrieving SAP credential                                   | e.g., `sharedsecretvault`        |
| **uk**        | Key for SAP username in Databricks secret scope                                                 | e.g., `hana-username`            |
| **pwk**       | Key for SAP password in Databricks secret scope                                                 | e.g., `hana-password`            |
| **db**        | SAP database name                                                                               | e.g., `P23`                      |

**Example Parameter Input**:
```bash
["-p","40","-t","ADPLAPSA2","-tpf","\"databricks-sap-connector\"","-sc","dev_mabras.checkpointschema",
"-td","dev_mabras.example_sap.adplapsa2","-c","PLANT,RECORDMODE,BPARTNER,COUNTY_CDE,COUNTRY,REGION",
"-pk","PLANT","-if","ROW_IDENTIFIER","-url","jdbc:sap://test_url.example_sap.adplapsa2","-s","sharedsecretvault",
"-uk","hana-p23-username","-pwk","hana-p23-password","-db","P23"]
```
### Step 3: Start a New Run with the Previously Created Job

Navigate to **Workflows > Jobs > {your_job_name} > Runs** and start a new run to execute the created job.

![Create Run](./images/create_run.png "Create a new run")

This will automatically initiate a new run of the task defined in the task section.  
The output of the run might look like this:

![Run Results](./images/runs_results.png "Show run results")

---

### Step 4: Check the Logs of the Run

1. Go to **Job Runs** and select the run you just executed.  
2. Here, you can review the outputs that were printed to the console during execution.  

It should look similar to this:  

![Run Logs](./images/test_run_output.png "Run logs")

---

### Explanation of Log Outputs

| Output                                  | Meaning                                                                 |
|-----------------------------------------|-------------------------------------------------------------------------|
| **No Checkpoint found**                 | The system tried to load a checkpoint for incremental data extraction, but none were found. |
| **Loading table**                       | Indicates the system is attempting to load the corresponding SAP HALA table. |
| **Bounds**                              | Displays the shape of the table being processed.                       |
| **Writing into table**                  | Specifies the table where the extracted data is being written.         |
| **Pipeline finished: ... : None -> N**  | Indicates the pipeline has finished extracting data, starting from checkpoint `None` and is now at row `N`. |








