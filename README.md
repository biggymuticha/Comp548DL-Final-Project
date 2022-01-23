# Comp548DL-Final-Project

## Background
The final project for the Big Data Management and Processing course was meant to ingest data from the New York City Taxi rides Pub/Sub public dataset, aggregate the streaming  data using  an Apache Beam pipeline before writing the aggregations into a BigQuery  table. However, after realising of a known _send_signal(signal.SIGINT)_ error which caused the script job to terminate (a Windows 10 laptop was used for the project), I decided to ingest the Pub/Sub topic using a Dataflow job and write the data as is into a BigQuery table. Aggregations were then done after writing the streaming data into BigQuery and presented as a graph using Data Studio. However, the Apache Beam code is also included in this repo should anyone want to try to run the same code from a Mac or linux computer.

The sql_taxi.py code is taken from the Apache Beam example codes found here.
- https://github.com/apache/beam/tree/master/sdks/python/apache_beam/examples

Information about the known issues when running an Apache Beam job that uses SQLTransform on Windows 10 computer is found on below links.
- https://issues.apache.org/jira/browse/BEAM-12501 
- https://lists.apache.org/thread/4yqxt2m8qd4x8o7vmzd6gth9t7dg7vkp


## Creating a Dataflow job that ingests data from  Pub/Sub topic into BigQuery

1. Enable the following APIs on Google cloud Platform
- Dataflow API
- Cloud Pub/Sub API

2. Create a BigQuery table with the following schema
 ![table schema](https://github.com/biggymuticha/Comp548DL-Final-Project/blob/main/table_schema.png)

3. Create a bucket and folder inside the bucket called _temp_
4. Create a Dataflow job using the Cloud Shell console, replacing the text in <> with specific names that suit your environment. You can also change the maximum number of workers and number of workers that should start running the job. Changing these options will help appreciating the autoscaling capabilities of Google's managed cloud platform and services.

 _gcloud dataflow jobs run <job_name> --gcs-location gs://dataflow-templates-us-central1/latest/PubSub_to_BigQuery --region <region_name> --max-workers 10 --num-workers 1  --staging-location <bucket_name>/temp --parameters inputTopic=projects/pubsub-public-data/topics/taxirides-realtime,outputTableSpec=<project_name>:<dataset_name>.<bigquery_table_name>_
  <br/><br/>
5. Write aggregation queries against the BigQuery table and visualize the results using Google Studio
  
  ![rides graph](https://github.com/biggymuticha/Comp548DL-Final-Project/blob/main/rides_graph.png)
  


