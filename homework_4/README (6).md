# Homework 4: Apache Airflow

The source code "hw4_source_code.ipnyb" does the following: 
- generates BERT embeddings for all movies and uploads them to S3
- splits the ratings data into four partitions

The "movies_dag.py" file does the following: 
- has a schedule to run every 10 hours
- every hour, generates recommendations for a cold user and a top user, uploading the results to S3

## Running the Source Code
### How to run: 
Simply run each cell in the source code file. Most importantly, you call the functions to generate the embeddings and partitions by running the following lines: 
embeddings_main()
partitions_main()

### Expected output: 
If the embeddings have already been made and uploaded, you will see the output: 
- BERT embeddings have already been generated and are in S3.

If not, you should see the following outputs: 
- Downloading dataset.
- Uploading raw zip file to S3 bucket.
- Downloaded dataset.
- Extracting zip.
- Data extraction process is done.
- Loading and processing all movies from the data file
- Generating BERT embeddings for all movies in batches.
- Successfully processed and uploaded embeddings to S3.

After running partitions_main(), you should see the following output: 
- Loading ratings data.
- Partitioning data.
- Uploaded partition to S3
- Uploaded partition to S3
- Uploaded partition to S3
- Uploaded partition to S3
- All 4 partitions are generated and in S3. All processes done.
  
## Activating the DAG
Simply switch on the DAG in Airflow, and you will see the 4 recommendations JSON output files uploaded to the S3 bucket in a new folder called recommendations/. 

# AI Usage
## Batch generating the embeddings
While implementing the BERT embeddings generation portion of the source code file, the initial attempt was to encode all the movies in the dataset at once, which ended up causing RAM memory issues and errors. We consulted Claude to resolve this issue and suggest a solution, which was to process the embeddings in batches using a loop and a batch_size. It suggested a batch size of 64 as a reasonable default that was quick enough but fixed the memory issue. We verified this solution by runing the embeddings generation without any memory issues and confirming that the output was correct. 

## Using Xcom and data passing / type handling
While building the DAG file, there were significant challenges we ran into around passing data between tasks using Airflow's XCom. We used Claude to understand how to properly use ti.xcom.pull() to retrieve outputs from previous tasks and how to make sure the data was correctly stored and shaped. It also helped fix a big memory crash issue by passing the entire accumulated ratings dataframe as a JSON string through XCom. The solution was to save the dataframe to S3 as a CSV and pass that S3 path down through XCom to then be used by the downstream task to load the data. We verified that this fix was effective by confirming the tasks completed successfully and checking that the final recommendations files in S3 were correct. 