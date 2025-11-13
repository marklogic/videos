Copyright (c) 2025 Progress Software Corporation and/or its subsidiaries or affiliates. All Rights Reserved.

# Export Data with MarkLogic Flux
With MarkLogic Flux, you can quickly export documents, rows, and triples from MarkLogic into a variety of destinations like local filesystems, Amazon S3, Azure storage, JDBC-accessible databases, and many more.  

You can download MarkLogic and MarkLogic Flux for free.   

YouTube video: https://youtu.be/Xf42vmFIDqo

Code Examples: https://github.com/marklogic/videos/tree/main/export-data-flux

Download MarkLogic Flux: https://github.com/marklogic/flux/releases 

View MarkLogic Flux Documentation: https://marklogic.github.io/flux 

Download MarkLogic: https://developer.marklogic.com/products/marklogic-server

View MarkLogic Documentation: https://docs.progress.com/category/marklogic-content-hub

View TDE Templates Documentation: https://docs.progress.com/bundle/marklogic-server-develop-server-side-apps-12/page/topics/TDE.html

View Optic API Documentation: https://docs.progress.com/bundle/marklogic-server-develop-server-side-apps-12/page/topics/OpticAPI.html

#marklogic #marklogicflux

## Example data
Save [presidents.jsonl](presidents.jsonl) to a directory on your system.  The directory will be used in the `--path` value in [Load Example Data](#load-example-data).



### Load Example Data 
#### (MacOS/Linux)
The following Flux command will load the example data in to a MarkLogic database. Run it from your MarkLogic Flux root directory with the `--path` and `--connection-string` values edited for your system.
```
./bin/flux import-aggregate-json-files \
    --json-lines \
    --path PATH/TO/presidents.jsonl \
    --connection-string "username:password@localhost:port" \
    --permissions rest-reader,read,rest-writer,update \
    --collections president
```
#### (Windows PowerShell)
```
./bin/flux import-aggregate-json-files `
    --json-lines `
    --path PATH/TO/presidents.jsonl `
    --connection-string "username:password@localhost:port" `
    --permissions rest-reader,read,rest-writer,update `
    --collections president
```
### Load TDE Template (XQuery)
This script loads a TDE template for the example data into your schemas database. The TDE template is required to run the examples on this page. Run it in Query Console with the Database menu set to your content database and the Query Type menu set to XQuery.
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
#### (MacOS/Linux)
```
./bin/flux export-delimited-files \
    --connection-string "username:password@localhost:port" \
    --query "op.fromView('main', 'presidents')" \
    --path destination
```
#### (Windows PowerShell)
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
#### (MacOS/Linux)
```
./bin/flux export-delimited-files \
    --connection-string "username:password@localhost:port" \
    --query "op.fromView('main', 'presidents').select(['LastName', 'Party', 'DateOfBirth', 'StateOfBirth']).where(op.eq(op.col('Party'), 'Republican'))" \
    --path "s3a://ml-peter-bucket/csv/" \
    --s3-add-credentials \
    --Pcompression=gzip \
    --file-count 1
```
#### (Windows PowerShell)
```
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
  
#### (MacOS/Linux)
  ```
./bin/flux export-jdbc \
    --connection-string "username:password@localhost:port" \
    --query "op.fromView('main', 'presidents').select(['LastName', 'Party', 'DateOfBirth', 'StateOfBirth']).where(op.eq(op.col('Party'), 'Republican')).where(op.eq(op.col('StateOfBirth'), 'Ohio'))" \
    --jdbc-url "jdbc:postgresql://localhost/example?user=postgres&password=postgres" \
    --jdbc-driver "org.postgresql.Driver" \
    --table "presidents"
```
#### (Windows PowerShell)
  ```
./bin/flux export-jdbc `
    --connection-string "username:password@localhost:port" `
    --query "op.fromView('main', 'presidents').select(['LastName', 'Party', 'DateOfBirth', 'StateOfBirth']).where(op.eq(op.col('Party'), 'Republican')).where(op.eq(op.col('StateOfBirth'), 'Ohio'))" `
    --jdbc-url "jdbc:postgresql://localhost/example?user=postgres&password=postgres" `
    --jdbc-driver "org.postgresql.Driver" `
    --table "presidents"
```
