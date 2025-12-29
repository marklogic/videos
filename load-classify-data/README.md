Copyright (c) 2025 Progress Software Corporation and/or its subsidiaries or affiliates. All Rights Reserved. <br>

<b>Load and Classify Data with MarkLogic Flux and Semaphore</b> <br>

With MarkLogic Flux, you can load data into MarkLogic using a Command Line Interface (CLI). This video shows how to compose your Executable, Command Name and Options CLI and use the Query Console to setup the classification configuration of Semaphore to get you ready for a future project in data enrichment. <br>

<b>Code for file importing into MarkLogic using Flux</b><br>

./Documents/Flux/bin/ import-aggregate-json-files \ <br>
  --json-lines \ <br>
  --path Documents/Flux/presidents.jsonl \  <br>
  --connection-string "user:password@localhost:8000" \  <br>
  --permissions rest-reader,read,rest-writer,update \  <br>
  --collections president  <br>

<b>Classification Configuration of Semaphore during Flux import process</b><br>

/Documents/Flux/bin/flux import-aggregate-json-files \  <br>
  --json-lines \  <br>
  --path /Documents/Flux/presidents.jsonl \  <br>
  --connection-string "user:password@localhost:8000/Documents" \  <br>
  --permissions rest-reader,read,rest-writer,update \
  --collections president \  <br>
  --classifier-host semaphore-server \  <br>
  --classifier-port 8443 \  <br>
  --classifier-protocol https \  <br>
  --classifier-token-path /api/auth/token \  <br>
  --classifier-path /api/classify  <br>

<b>Example Code before any classification is performed</b><br>

{"id": "doc001", "text": "President Lincoln delivered the Gettysburg Address in 1863."}  <br>
{"id": "doc002", "text": "President Roosevelt introduced the New Deal during the Great Depression."}  <br>

<b>Example Code after Semaphore classication has been performed</b><br>
{
  "id": "doc001", <br>
  "text": "President Lincoln delivered the Gettysburg Address in 1863.",  <br>
  "classification": {  <br>
    "rulebaseId": "historical-events-v1",  <br>
    "concepts": [  <br>
      {  <br>
        "conceptId": "US_PRESIDENT",  <br>
        "label": "U.S. President",  <br>
        "score": 0.98,  <br>
        "explanations": [  <br>
          {  <br>
            "ruleId": "r-0101",  <br>
            "matched": "President Lincoln",  <br>
            "window": "exact match"  <br>
          }  <br>
        ]  <br>
      }  <br>
}  <br>

 
          
       
        
    
   