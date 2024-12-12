# Quick Guide: Ingest Data from SAP HANA to Databricks

Follow these steps to integrate SAP HANA with Databricks.

## 1. Import the Wheel File into Databricks Workspace
- Upload the provided wheel file to Databricks.
- Confirm the import by selecting **Import**.

## 2. Create a New Job in Databricks
> **Note**: [See here](examples.md) to create a job from the default yaml file
- Navigate to **Databricks Workflows** and create a job.
- Fill in the required parameters for the job (e.g., Task Name, Type, Package Name, etc.).

## 3. Add Dependent Libraries
- Add the imported wheel file and `ngdbc.jar` to the job configuration.

## 4. Set Environment Parameters
- Define necessary parameters (e.g., table name, primary keys, SAP credentials).

## 5. Start the Job Run
- Navigate to **Workflows > Jobs > {your_job_name} > Runs** and start a new run to execute the job.

## 6. Check the Logs
- After the job runs, go to **Job Runs** and select the run you just executed.
- Review the outputs printed to the console during execution.
