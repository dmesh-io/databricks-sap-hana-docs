# Examples

### Create Job by using the default yaml file

To add a YAML file for a Databricks job, navigate to the **New Job** section. Click on the three dots ( **...** ) in the interface, and select the option to import the YAML configuration. Paste the following YAML file into the provided input field:
> **Note** Adjust the file so it fulfills your requirements
<details>
  <summary>Click to view the full YAML file</summary>

```yaml
resources:
  jobs:
    New_Job_2024:
      name: New Job 2024
      tasks:
        - task_key: test
          python_wheel_task:
            package_name: databricks-sap-connector
            entry_point: sap_table
            parameters:
              - --partitions
              - "40"
              - --table-name
              - SAP_TABLE_NAME
              - -tpf
              - '"SAPSR3"."/CUSTOM/SAP_TABLE_NAME"'
              - -sc
              - dev_mabras.checkpointschema
              - -td
              - dev_mabras.example_sap.adplapsa2
              - -c
              - PLANT,RECORDMODE,BPARTNER,COUNTY_CDE,COUNTRY,REGION
              - -pk
              - PLANT
              - -if
              - ROW_IDENTIFIER
              - -url
              - jdbc:sap://test_url.example_sap.adplapsa2
              - -s
              - sharedsecretvault
              - --user-key
              - hana-username
              - --password-key
              - hana-password
              - --database-name
              - DB123
          job_cluster_key: test_cluster
          libraries:
            #This should be the databricks path the wheel.whl file
            - whl: /Workspace/Users/felix.fuchs@dmesh.io/SAP_Connector_New/databricks_sap_connector-0.0.1+20241122.130359-py3-none-any.whl
      job_clusters:
        #These are the Informations for your cluster
        - job_cluster_key: test_cluster
          new_cluster:
            spark_version: 15.4.x-scala2.12
            azure_attributes:
              first_on_demand: 1
              availability: ON_DEMAND_AZURE
              spot_bid_max_price: -1
            node_type_id: Standard_D4ds_v5
            spark_env_vars:
              PYSPARK_PYTHON: /databricks/python3/bin/python3
            enable_elastic_disk: true
            data_security_mode: SINGLE_USER
            runtime_engine: PHOTON
            num_workers: 8
      queue:
        enabled: true

```
</details>