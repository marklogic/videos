Copyright (c) 2025 Progress Software Corporation and/or its subsidiaries or affiliates. All Rights Reserved.

#  How to Split Data for AI Applications with MarkLogic Flux 
MarkLogic Flux provides a unified, scalable, and optimized data pipeline for processing documents and generating the necessary components—specifically chunks and vector embeddings—required to build effective Retrieval-Augmented Generation (RAG) systems that leverage MarkLogic's vector query capabilities.

These commands will split and import data into MarkLogic.

## Common Parameters
`--path`: The path to the file to be split.

`--connection-string`:  Update this with your username, password, and port.

`--splitter-json-pointer`: Sets the field to split. The examples split the `description` field. Update this reference to split a different field.

`--splitter-max-chunk-size`: Sets the max chunk size (in characters). 

`--splitter-max-overlap-size`: The overlap size. This helps prevent loss of context.  A value of 50 specifies that up to 50 characters from the end of one chunk will be repeated at the beginning of the next chunk.

`--collections`: The name of the collection in MarkLogic.

`--permissions`: Sets the permissions.




### Character-based Splitting with the Default Splitter
This example splits the description field after every 500 characters at the nearest word boundary.  It also specifies a maximum overlap of 50 characters between the end of one chunk and the beginning of the next chunk. 


### macOS/Linux


```
 ./bin/flux import-files \
  --path "presidents.jsonl" \
  --connection-string "user:password@localhost:8000" \
  --splitter-json-pointer "/description" \
  --splitter-max-chunk-size 500 \
  --splitter-max-overlap-size 50 \
  --collections presidents_split\
  --permissions rest-reader,read,rest-writer,update
  ```
### Windows
```
.\bin\flux import-files ^
  --path "presidents.jsonl" ^
  --connection-string "user:password@localhost:8000" ^
  --splitter-json-pointer "/description" ^
  --splitter-max-chunk-size 500 ^
  --splitter-max-overlap-size 50 ^
  --collections presidents_split_windowscmd ^
  --permissions rest-reader,read,rest-writer,update
  ```

  ### RegEx Splitter
  This command splits data based on a Regular Expression (RegEx)

  `--splitter-regex`: This specifies the RegEx pattern used to split the data.  


  #### macOS/Linux
  ```
  ./bin/flux import-files \
  --path "presidents.jsonl" \
  --connection-string "user:password@localhost:8000" \
  --splitter-json-pointer "/description" \
  --splitter-regex "[.!?]\s+" \
  --splitter-max-chunk-size 1000 \
  --collections presidents_regex_multi \
  --permissions rest-reader,read,rest-writer,update
```
  #### Windows 
  ```
.\bin\flux import-files ^
  --path "presidents.jsonl" ^
  --connection-string "user:password@localhost:8000" ^
  --splitter-json-pointer "/description" ^
  --splitter-regex "[.!?]\s+" ^
  --splitter-max-chunk-size 1000 ^
  --collections presidents_regex_multi ^
  --permissions rest-reader,read,rest-writer,update
```
### Custom Splitter
You can create custom splitters. A good use case for using a custom splitter with Flux is when you need to implement advanced, sophisticated, or domain-specific text-splitting strategies that go beyond the capabilities of Flux's native default or RegEx options. For example, splitting medical records by diagnosis.

####  macOS/Linux

```
./bin/flux import-files \
  --path "presidents.jsonl" \
  --connection-string "admin:admin@localhost:8000" \
  --splitter-json-pointer "/description" \
  --splitter-custom-class "com.example.presidential.PresidentialBiographySplitter" \
  -SmaxChunkSize=1000 \
  -SaddMetadata=true \
  --collections presidents_custom_split \
  --permissions rest-reader,read,rest-writer,update
```
#### Windows
```
.\bin\flux import-files ^
  --path "presidents.jsonl" ^
  --connection-string "admin:admin@localhost:8000" ^
  --splitter-json-pointer "/description" ^
  --splitter-custom-class "com.example.presidential.PresidentialBiographySplitter" ^
  -SmaxChunkSize=1000 ^
  -SaddMetadata=true ^
  --collections presidents_custom_split ^
  --permissions rest-reader,read,rest-writer,update
  ```