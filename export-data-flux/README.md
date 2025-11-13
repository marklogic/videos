Copyright (c) 2025 Progress Software Corporation and/or its subsidiaries or affiliates. All Rights Reserved.

# Export Data with MarkLogic Flux

## Example data

- [presidents.jsonl](presidents.jsonl)

### Notes:

* You can load the example data using the Flux import command listed below.
* A TDE template is also required to run these examples. An example script for loading a TDE template is listed below. For more information, see [Template Driven Extraction (TDE)](https://docs.progress.com/bundle/marklogic-server-develop-server-side-apps-12/page/topics/TDE.html).

### Load Example Data 
#### (MaxOS/Linux)
The following Flux command will load the example data in to a MarkLogic database. Run it from your MarkLogic Flux root directory with the `--path` and `--connection-string` values edited for your system.
```
./bin/flux import-aggregate-json-files \
    --json-lines \
    --path PATH/TO/presidents.jsonl \
    --connection-string "username:password@localhost:port" \
    --permissions rest-reader,read,rest-writer,update \
    --collections president
```
#### (Windows Powershell)
```
./bin/flux import-aggregate-json-files `
    --json-lines `
    --path PATH/TO/presidents.jsonl `
    --connection-string "username:password@localhost:port" `
    --permissions rest-reader,read,rest-writer,update `
    --collections president
```
### Load TDE Template (XQuery)
The following script will load a TDE template for the example data into your schemas database. Run it in Query Console with the Database menu set to your content database and the Query Type menu set to XQuery.
```
xquery version "1.0-ml"; 

import module namespace tde = "http://marklogic.com/xdmp/tde" 
       at "/MarkLogic/tde.xqy";

let $presidents :=
<template xmlns="http://marklogic.com/xdmp/tde">
  <context>/</context>
  <collections>
    <collection>president</collection>
  </collections>
  <rows>
    <row>
      <schema-name>main</schema-name>
      <view-name>presidents</view-name>
      <columns>
        <column>
          <name>FirstName</name>
          <scalar-type>string</scalar-type>
          <val>firstName</val>
        </column>
        <column>
          <name>LastName</name>
          <scalar-type>string</scalar-type>
          <val>lastName</val>
        </column>
        <column>
          <name>Party</name>
          <scalar-type>string</scalar-type>
          <val>party</val>
        </column>
        <column>
          <name>DateOfBirth</name>
          <scalar-type>date</scalar-type>
          <val>dob</val>
        </column>
        <column>
          <name>StateOfBirth</name>
          <scalar-type>string</scalar-type>
          <val>birthplace/state</val>
        </column>
       </columns>
    </row>
  </rows>
</template>

return tde:template-insert("/presidents.xml", $presidents)
```


## Export to CSV 
In the example, replace these values:
  * `username` - Your MarkLogic username
  * `password` - Your MarkLogic password
  * `port` - The MarkLogic port for your app server
#### (MaxOS/Linux)
```
./bin/flux export-delimited-files \
    --connection-string "username:password@localhost:port" \
    --query "op.fromView('main', 'presidents')" \
    --path destination

```
#### (Windows Powershell)
```
./bin/flux export-delimited-files  `
    --connection-string "username:password@localhost:port"  `
    --query "op.fromView('main', 'presidents')"  `
    --path destination
```
### Export to S3
In the example, replace these values:
  * `username` - Your MarkLogic username
  * `password` - Your MarkLogic password 
  * `port` - The MarkLogic port for your app server
  *    `s3a://ml-peter-bucket/csv/` - The address of your S3 storage
#### (MaxOS/Linux)
```
./bin/flux export-delimited-files \
    --connection-string "username:password@localhost:port" \
    --query "op.fromView('main', 'presidents').select(['LastName', 'Party', 'DateOfBirth', 'StateOfBirth']).where(op.eq(op.col('Party'), 'Republican'))" \
    --path "s3a://ml-peter-bucket/csv/" \
    --s3-add-credentials \
    --Pcompression=gzip \
    --file-count 1
```
```
#### (Windows Powershell)
./bin/flux export-delimited-files `
    --connection-string "username:password@localhost:port" `
    --query "op.fromView('main', 'presidents').select(['LastName', 'Party', 'DateOfBirth', 'StateOfBirth']).where(op.eq(op.col('Party'), 'Republican'))" `
    --path "s3a://ml-peter-bucket/csv/" `
    --s3-add-credentials `
    -Pcompression=gzip `
    --file-count 1
```

### Export to JDBC-accessible database
In the example, replace these values:
  * `username` - Your MarkLogic username
  * `password` - Your MarkLogic password
  * `port` - The MarkLogic port for your app server
  * `postgresql://localhost/example?user=postgres&password=postgres` - The url of your jdbc-accessible database
  * `org.postgresql.Driver` - The driver for your database
  * `presidents` - The name of the destination table
  
#### (MaxOS/Linux)
  ```
./bin/flux export-jdbc \
    --connection-string "username:password@localhost:port" \
    --query "op.fromView('main', 'presidents').select(['LastName', 'Party', 'DateOfBirth', 'StateOfBirth']).where(op.eq(op.col('Party'), 'Republican')).where(op.eq(op.col('StateOfBirth'), 'Ohio'))" \
    --jdbc-url "jdbc:postgresql://localhost/example?user=postgres&password=postgres" \
    --jdbc-driver "org.postgresql.Driver" \
    --table "presidents"
```
