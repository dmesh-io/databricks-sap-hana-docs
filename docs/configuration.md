## Configuration
### Basic Configuration
```yaml
source:
  type: kafka
  brokers: ["localhost:9092"]
  topic: "source-topic"

destination:
  type: redshift
  connectionString: "jdbc:redshift://example-cluster:5439/database"
  table: "target_table"
```

### Environment Variables
- `CONNECTOR_SOURCE`: Specifies the source system.
- `CONNECTOR_DESTINATION`: Specifies the target system.
