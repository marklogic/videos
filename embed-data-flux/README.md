Copyright (c) 2025 Progress Software Corporation and/or its subsidiaries or affiliates. All Rights Reserved.

# Embed Data with MarkLogic Flux

Vector embeddings are numerical representations of text that capture semantic meaning, allowing AI systems to understand and compare content based on context rather than just keywords. MarkLogic Flux can generate these vector embeddings for your documents, enabling you to build powerful AI applications like semantic search, recommendation systems, and Retrieval-Augmented Generation (RAG) using MarkLogic Server's vector query capabilities.

These commands will generate embeddings and import data into MarkLogic.

## Common Parameters

`--path`: The path to the file to be processed.

`--connection-string`: Update this with your username, password, hostname, and port.

`--splitter-json-pointer`: This splits the designated field into chunks. The examples in this repository split the `description` field.  

`--splitter-max-chunk-size`: This specifies the maximum size of a chunk. In this repository, the maximum chunk size is 500 characters.  

`--splitter-sidecar-max-chunks`: This indicates that a maximum of one chunk should be in each sidecar (separate) document.

`--splitter-sidecar-collections`: Specifies the collection where the documents are stored. 


`--embedder`: Specifies the embedding model. The examples in this repository use the MiniLM embedder model. The MiniLM model is a lightweight, efficient transformer-based model optimized for generating high-quality text embeddings with reduced computational overhead.


## Example data

- [presidents.jsonl](presidents.jsonl)

## Import Documents and Generate Embeddings with the MiniLM model
This example:
* Imports the `presidents.jsonl` document.
* Splits the description field into chunks with a maximum size of 500 characters.
* Stores the chunks and embeddings in additional or sidecar documents.
* Stores the documents in the `chunks` collection.
* Uses the MiniLM model to generate embeddings.

### macOS/Linux

```
./bin/flux import-files \
  --path "presidents.jsonl" \
  --connection-string "user:password@localhost:8000" \
  --splitter-json-pointer "/description" \
  --splitter-max-chunk-size 500 \
  --splitter-sidecar-max-chunks 1 \
  --splitter-sidecar-collections chunks \
  --embedder minilm
```

### Windows

```
.\bin\flux import-files ^
  --path "presidents.jsonl" ^
  --connection-string "user:password@localhost:8000" ^
  --splitter-json-pointer "/description" ^
  --splitter-max-chunk-size 500 ^
  --splitter-sidecar-max-chunks 1 ^
  --splitter-sidecar-collections chunks ^
  --embedder minilm
```

## Import Documents and Generate Embeddings with Base64 Encoding

This example uses Base64 encoding to encode the vector embeddings. The resulting documents are stored in the `base64-chunks` collection.

Base64 encoding converts the binary vector data into a text-based format, which can be useful for storage optimization or when working with systems that require text-based data representations.

### macOS/Linux

```
./bin/flux import-files \
--path "presidents.jsonl" \
--connection-string "user:password@localhost:8000" \
--splitter-json-pointer "/description" \
--splitter-max-chunk-size 500 \
--splitter-sidecar-max-chunks 1 \
--splitter-sidecar-collections base64-chunks \
--embedder minilm \
--embedder-base64-encode
```

### Windows

```
.\bin\flux import-files ^
  --path "presidents.jsonl" ^
  --connection-string "user:password@localhost:8000" ^
  --splitter-json-pointer "/description" ^
  --splitter-max-chunk-size 500 ^
  --splitter-sidecar-max-chunks 1 ^
  --splitter-sidecar-collections base64-chunks ^
  --embedder minilm ^
  --embedder-base64-encode
```

## Import Documents and Generate Embeddings with a Prompt
A prompt tells the embedding model how to interpret information and what the vectors should represent.  Prompts are useful to help define domain-specific information.  

### macOS/Linux
```
./bin/flux import-files \
--path "presidents.jsonl" \
--connection-string "user:password@localhost:8000" \
--splitter-json-pointer "/description" \
--splitter-max-chunk-size 500 \
--splitter-sidecar-max-chunks 1 \
--splitter-sidecar-collections add-prompt-chunks \
--embedder minilm \
--embedder-prompt "Represent this passage about US Presidential history for retrieval:"
```
### Windows
```
.\bin\flux import-files ^
--path "presidents.jsonl" ^
--connection-string "user:password@localhost:8000" ^
--splitter-json-pointer "/description" ^
--splitter-max-chunk-size 500 ^
--splitter-sidecar-max-chunks 1 ^
--splitter-sidecar-collections add-prompt-chunks ^
--embedder minilm ^
--embedder-prompt "Represent this passage about US Presidential history for retrieval:"