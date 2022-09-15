# AWS - Canvas Live Events Connector

## Project goals
  - Develop an integration solution for Canvas Live Events that is easy to install, maintain, includes relevant supporting data and ensures that data is available in a timely manner
  - Leverage AWS AppFlow for custom integration of Canvas API data
  - Provide a basic data product which demonstrates usage of the integrated data sets

## AWS S3
  - Seven buckets storing event and entity data
    - Raw data partitioned by date
    - Latest cleaned copy of the full dataset
  - One bucket storing the merged data product
    - Event data joined with user, course and assignment
    - Stores only the latest copy of the merged dataset

## AWS Lambda
  - CanvasLiveEventsLambda
    - Java Lambda function
    - Triggered by SQSEvent (events on the queue)
    - Writes batches of events to S3; batch size configurable
  - CanvasAppFlowLamba
    - Java Lambda function
    - Triggered by AppFlow
    - Retrieves entity data from Canvas via Canvas APIs

## AWS AppFlow
  - Three separate flows; one for each entity type
  - Can run independently or be orchestrated as a workflow (users -> courses -> assignments)
  - Each invocation of a flow handles a single batch of data; batch size will be configurable

## AWS Glue
  - Four crawlers - one for each data type
  - Single data catalog
    - Five tables in the catalog - one for each data type and one for the data product
  - Crawlers can be run independently or orchestrated

## AWS DataBrew
  - Seven datasets and projects
    - Raw data for entities and events
    - Cleansed data for entities
  - Four jobs
    - Cleansing of entity data (deduplication, column renaming, etc)
    - Data product creation (joining of event and entity data, data cleansing)

## AWS CloudFormation
  - Cloudformation templates available for the majority of resources
    - IAM, S3, Lambda
    - AppFlow, Glue, DataBrew
  - Exceptions (what will need to configured manually)
    - Any scheduling / orchestration (e.g., Glue crawlers)
    - Glue Data Catalog tables (crawlers will create these automatically)
    - Networking

#### Need more information or further assistance? Please visit [Unicon - AWS Canvas Connector](https://www.unicon.net/aws-canvas-connector).
