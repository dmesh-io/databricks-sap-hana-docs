# Prerequisites for the SAP HANA Connector to Databricks

> **Note:** Our connector does not support the sample databricks service as it requires a JAR file to function.
1. **Catalog**: Set up a single catalog for checkpoints (e.g., dev or prod). Use `pipeline_name` as the key.  
2. **Permissions**: Ensure permissions for file uploads, job creation, and task execution in Databricks. [Details here](https://docs.databricks.com/en/jobs/privileges.html).  
3. **SAP HANA Access**: Have valid credentials, including username, password, and JDBC URL. [Details here](https://help.sap.com/docs/SAP_HANA_PLATFORM?locale=en-US).  
4. **Required Files**: Prepare the SAP HANA connector 
   1. **wheel.whl** [provided by us](contacts.md), containing the connector logic.
   2. **ngdbc.jar** [provided by SAP HANA](https://support.sap.com/en/index.html), for communication with the database.
5. **Cluster**: Ensure access to a suitable Databricks cluster.  

Once ready, proceed to the [setup and ingestion process](configuration).


