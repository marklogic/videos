Copyright (c) 2025 Progress Software Corporation and/or its subsidiaries or affiliates. All Rights Reserved.

# Load and Classify Data with MarkLogic Flux and Semaphore

With MarkLogic Flux, you can load data into MarkLogic using a Command Line Interface (CLI) and classify that data using Semaphore. This video shows how to compose your Executable, Command Name, and Options, use Query Console to review your loaded data, and configure your Flux command to leverage Semaphore for classification.

## Example data

- [presidents.jsonl](presidents.jsonl)

## Code for importing data into MarkLogic using Flux 
### macOS/Linux
```
./Documents/Flux/bin/flux import-aggregate-json-files \
  --json-lines \
  --path Documents/Flux/presidents.jsonl \
  --connection-string "user:password@localhost:8000" \
  --permissions rest-reader,read,rest-writer,update \
  --collections president
```
### Windows
```
.\Documents\Flux\bin\flux import-aggregate-json-files ^
  --json-lines ^
  --path Documents/Flux/presidents.jsonl ^
  --connection-string "user:password@localhost:8000" ^
  --permissions rest-reader,read,rest-writer,update ^
  --collections president
  ```

## Code for configuring Flux for Semaphore classification 
### macOS/Linux
```
./Documents/Flux/bin/flux import-aggregate-json-files \
  --json-lines \
  --path /Documents/Flux/presidents.jsonl \
  --connection-string "user:password@localhost:8000/Documents" \
  --permissions rest-reader,read,rest-writer,update \
  --collections president \
  --classifier-host semaphore-server \
  --classifier-port 8443 \
  --classifier-protocol https \
  --classifier-token-path /api/auth/token \
  --classifier-path /api/classify
```
### Windows
```
.\Documents\Flux\bin\flux import-aggregate-json-files ^
  --json-lines ^
  --path \Documents\Flux\presidents.jsonl ^
  --connection-string "user:password@localhost:8000/Documents" ^
  --permissions rest-reader,read,rest-writer,update ^
  --collections president ^
  --classifier-host semaphore-server ^
  --classifier-port 8443 ^
  --classifier-protocol https ^
  --classifier-token-path /api/auth/token ^
  --classifier-path /api/classify
```
