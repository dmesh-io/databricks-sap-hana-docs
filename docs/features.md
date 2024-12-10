# Features

## Incremental loading

We use incremental loading and store a checkpoint on a specified schema. The incremental loading is based on  $ROWID. More info [here](https://community.sap.com/t5/technology-blogs-by-members/rowid-column-in-hana/ba-p/13570323)

## High-performance data transfer with fault tolerance.

As we use Apache Spark we have this connector running in a distributed mode. Make sure not to make the cluster so big otherwise you will kill the SAP Hana

## Extensive configuration options.

See [configuration](usage) page

## Secure and compliant with industry standards.

We use Databricks Secrets to grab the proper username and password for connecting to SAP Hana. Se configuration on how to configure this. More info [here](https://learn.microsoft.com/en-us/azure/databricks/security/secrets/)

