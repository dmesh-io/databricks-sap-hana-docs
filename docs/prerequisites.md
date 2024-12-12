# Prerequisities
## 1. Catalog
Set up a single catalog (e.g., `dev` or `prod`) to manage checkpoints. The catalog is essential for:  
- Restarting from a saved state in case of failures   
- Reducing the risk of data loss during processing

The `pipeline_name` is used as a key within the catalog.

## 2. Permissions
Ensure you have appropriate permissions in Databricks for:  
- File uploads  
- Job creation  
- Task execution  

[Learn more about permissions here](https://docs.databricks.com/en/jobs/privileges.html).

## 3. SAP HANA Access
Prepare valid SAP HANA credentials, including:  
- Username and password  
- JDBC URL  

[Learn more about obtaining these credentials](https://help.sap.com/docs/SAP_HANA_PLATFORM?locale=en-US).

## 4. Required Files
Ensure you have the following files ready:  
1. **`wheel.whl`**: Contains the connector logic and is [provided by us](contacts.md).  
2. **`ngdbc.jar`**: Required for database communication and is given by [SAP](https://support.sap.com/en/index.html).  

## 5. Databricks Cluster
Verify access to a suitable Databricks cluster with sufficient resources to run the pipeline.

**Please note:** We do not support databricks sample data services, as we require the ngdbc.jar file for proper database communication.

> **Once the requirements are met, proceed to the [configuration section](configuration).**


