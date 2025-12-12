Copyright (c) 2025 Progress Software Corporation and/or its subsidiaries or affiliates. All Rights Reserved.

# Enable TDE-Driven Data Analytics with MarkLogic Flux

## Example: Import CSV Data as JSON Documents and Generate a TDE Template for the Imported Data

### macOS/Linux
```
./bin/flux import-delimited-files \
  --path ../data/employees.csv.gz \
  --connection-string "flux-example-user:password@localhost:8000" \
  --collections employees \
  --permissions flux-example-role,read,flux-example-role,update \
  --tde-schema hr \
  --tde-view employees \
  --tde-collections employees \
  --tde-permissions flux-example-role,read,flux-example-role,update
```

### Windows PowerShell
```
bin\flux import-delimited-files ^
  --path C:\Flux\employees.csv ^
  --connection-string "ajayk:Bananas7@localhost:8000" ^
  --collections employees ^
  --permissions flux-example-role,read,flux-example-role,update ^
  --tde-schema hr ^
  --tde-view employees ^
  --tde-collections employees ^
  --tde-permissions flux-example-role,read,flux-example-role,update
```
