# Troubleshooting
## Debugging
Enable debug mode for detailed logging and a better understanding of potential errors.  
To activate debug mode, set the `--loglevel` parameter to `DEBUG` when creating the Databricks task.  
For more information, see [here](configuration.md).

## Limited Resources
While your job is running, check the Spark logs to monitor how much data each partition is processing.  
If one partition is pulling significantly more data (e.g., 10 GB) compared to others (e.g., 100 MB), your job may experience queuing.  
This can lead to performance issues or even cause the job to run out of memory. In such cases, you may need to adjust your data partitioning strategy or optimize resource allocation.
For further help [see](contacts.md)

