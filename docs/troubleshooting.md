## Troubleshooting
### Common Issues
- **Error: Connection timed out**  
  Ensure the source and destination are reachable and network rules allow access.

- **Error: Invalid configuration file**  
  Check the syntax of your `config.yaml`.

### Logs
Enable debug logs for more insights:
```bash
etl-connector --config config.yaml --log-level DEBUG
```
