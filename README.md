# capturing-logs-using-Log-Router-and-triggering-a-cloud-function
This repository helps you to capture logs using Log Router Sink and trigger a Cloud Function.

```
1. Create a Cloud SQL:MySQL instance:
--------------------------------------------
- Create a Cloud SQL:My SQL instane.
- Create a database and a simple table.
- Please refer this documentation to create a Cloud SQL instance: https://codelabs.developers.google.com/codelabs/connecting-to-cloud-sql-with-cloud-functions#1
```

```
2. Create a Cloud Function (function-1):
--------------------------------------------
- Create a cloud function 1st generation.
- Assign the role "Cloud SQL Client" to the default Compute Engine service account. It has the suffix "compute@developer.gserviceaccount.com".
- While creating the Cloud Function, expand the Runtime, Build and Connections Settings In Runtime service account, select a service account that has the Cloud SQL Client role.
- Click the NEXT button.
- Select Python for the runtime option.
- Select Inline editor for the source code option.
- In the source code editor windows, delete the existing content for both "requirements.txt" and "main.py", and replace them with your edited versions of the code above.
- The above code "main.py" will insert values into the table in the Cloud SQL database created in earlier step.
- Deploy the Cloud Function.
```

```
3. Create a Cloud Pub/Sub topic:
--------------------------------------------
- Follow this documentation to create a Cloud Pub/Sub topic: https://cloud.google.com/pubsub/docs/create-topic#create_a_topic
- You can also route logs to Cloud Bigquery or Cloud Storage bucket by creating a Sink in Log Router.
```

```
4. Create a Sink in Log Router:
------------------------------------------------------------------------------------
- Create a Sink in the Log Router and select Cloud Pub/Sub that is created in previous step as Sink destination. Refer this documentation to understand creating a sink in Log Router: https://cloud.google.com/logging/docs/export/configure_export_v2#creating_sink
- While creating the sink, add the appropriate log filters that would pick specific logs and export it to the destination. Refer this documentation to understand Logging Query Language that can be added into the field "Choose logs to include in sink": https://cloud.google.com/logging/docs/view/logging-query-language
- To capture the cloud SQL Logs, Please use the query in the field "Choose logs to include in sink" as:
resource.type="cloudsql_database"
resource.labels.database_id="PROJECT_ID:SQL_INSTANCE_ID"

**NOTE: While creating the Sink in Log Router, logs routed to Cloud Storage are written in hourly batches while other sink types are processed in real time.**
- Please copy the writerIdentity service agent and grant appropriate permissions in the sink destination. Refer this documentation to understand setting destination permissions: https://cloud.google.com/logging/docs/export/configure_export_v2#dest-auth
```

```
5. Create a Cloud Function (function-2):
--------------------------------------------
- Create a cloud function 1st generation.
- Configure the trigger and select the Cloud Pub/Sub topic created earlier as the trigger type. Refer this documentation to understand triggers in Cloud Function 1st Gen and 2nd Gen: https://cloud.google.com/functions/docs/calling
- Select "Python" runtime.
- Deploy the Cloud Function.
```

```
Check the logs of Cloud Function (function-2) that is deployed to get the Cloud SQL instance logs.

Any additional suggestions on this are accepted. Please reach out to me at pawansenapati1999@gmail.com
```
