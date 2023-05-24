---
description: >-
  Install PipeRider with the Athena connector and connect to a Athena data
  source.
---

# Athena Connector

## Installation

```
pip install 'piperider[athena]'
```

## Configuration (DBT)

Run the diagnose command in the dbt project

```
piperider diagnose
```

If you can successfully connect to Athena using dbt, PipeRider can also connect to Athena using the same profile settings. For details, please refer to the dbt [Athena adapter](https://docs.getdbt.com/reference/warehouse-setups/athena-setup) documentation.

## Configuration (Non-DBT)

{% hint style="warning" %}
non-dbt use case is deprecated since v0.25.0
{% endhint %}

The adapter utilizes AWS CLI/boto3 credential file to connect to Athena. You can provide

* `~/.aws/credentials`
* or `AWS_*` environment variables

Please refer to the [aws credential](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html)s document

### Initialize a new Athena PipeRider project

```shell-session
piperider init
[?] What is your data source name?: AthenaProject
[?] Which data source would you like to connect to?:
 > athena
```

Follow the prompts entering the required information.

### Required information

After selecting Athena as your data source, you will be promoted for the following information:

* **S3 location to store Athena query results and metadata**: Same as the query result location in the [query editor](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html).
* **AWS region name of your Athena instance**: e.g. us-east-1
* **Athena data catalog**: Same as the data source name in the [query editor](https://docs.aws.amazon.com/athena/latest/ug/data-sources-managing.html). Use `awsdatacatalog` for AWS Glue Data Catalog.
* **Athena database:** Same as the database name in the [query editor](https://docs.aws.amazon.com/athena/latest/ug/getting-started.html)

## Test connection settings

After configuring your connection settings, ensure that PipeRider can connect to your Athena data source.

```
piperider diagnose
```
