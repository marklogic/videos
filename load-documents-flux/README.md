# Load Date with MarkLogic Flux

## Example data

- [presidents.jsonl](presidents.jsonl)
- [people.xml](people.xml)
- [example.rdf.zip](example.rdf.zip)

## Load JSON in JSON Lines format (MaxOS/Linux)

```
./bin/flux import-aggregate-json-files \
    --json-lines \
    --path PATH/TO/presidents.jsonl \
    --connection-string "db-user:p4ssw0rd@localhost:8000" \
    --permissions rest-reader,read,rest-writer,update \
    --collections president
```

## Load JSON in JSON Lines format (Windows)

TO COME

## Load separate XML documents from an XML file based on an element (MaxOS/Linux)

```
./bin/flux import-aggregate-xml-files \
    --path PATH/TO/people.xml \
    --element person \
    --namespace org:example \
    --connection-string "db-user:p4ssw0rd@localhost:8000" \
    --permissions rest-reader,read,rest-writer,update
```

## Load semantic triples from a compressed RDF file (MaxOS/Linux)

```
./bin/flux import-rdf-files \
    --path PATH/TO/example.rdf.zip \
    --connection-string "db-user:p4ssw0rd@localhost:8000" \
    --permissions rest-reader,read,rest-writer,update \
    --compression ZIP
```

## Query the loaded JSON or XML data with the MarkLogic Search API

```
curl --digest -u "db-user:p4ssw0rd" http://localhost:8000/v1/search?q=George
```
