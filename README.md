# Setting up Snowflake to BigQuery dataflow job using JDBC to BigQuery template

## Create a Service Account
1. Create a service account (e.g., dataflow-runner)
2. Give the service account the following roles:
  - BigQuery Admin
  - DataFlow Worker

## Setup GCS
1. Create a GCS bucket to house the dataflow artifacts (e.g., gs://dataflow-demo)
2. Create a temp directory in gs://dataflow-demo (e.g., gs://dataflow-demo/temp)
3. Create another temp directory in gs://dataflow-demo/temp (e.g., gs://dataflow-demo/temp/tempfiles)
4. Give the service account bucket owner role on gs://dataflow-demo
5. Upload the latest JDBC driver [https://repo1.maven.org/maven2/net/snowflake/snowflake-jdbc/] to gs://dataflow-demo

## Setup BigQuery
1. Create a dataset and table with the same schema as the source schema of the table in Snowflake

## Setup VPC (if needed)
The default VPC may already have a firewall rule that allows internal ingress
1. Create a VPC or use the default VPC
2. Create a firewall rule with source and target tags set to 'dataflow' and allow ingress on TCP port: 12345-12346

## Set up DataFlow
1. Go to DataFlow and then to the Jobs sub-menu
2. Click 'CREATE JOB FROM TEMPLATE'
3. Supply a job name, select region endpoint, and choose 'Jdbc to BigQuery' dataflow template
4. Fill in the parameters
  - Jdbc connection URL (e.g., jdbc:snowflake://abcdef.us-central1.gcp.snowflakecomputing.com)
  - Jdbc driver class name (net.snowflake.client.jdbc.SnowflakeDriver)
  - Jdbc source SQL query (e.g., SELECT * FROM <i>table name</i>)
  - BigQuery output table (e.g., your-project:your-dataset.your-table-name)
  - GCS path for Jdbc drivers (gs://dataflow-demo/snowflake-jdbc-3.13.20.jar)
  - Temp directory for BQ (gs://dataflow-demo/temp)
  - Temp location (gs://dataflow-demo/temp/tempfiles)
  - Jdbc connection property string (e.g., warehouse=COMPUTE_WH;db=DEMO_DB;schema=public)
  - Jdbc connection username (snowflake username)
  - Jdbc connection password (snowflake password)
  - Service account email (service account you created above)
  - Network (network you created above or the default VPC)
5. Run Job