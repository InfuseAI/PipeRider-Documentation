---
description: Install PipeRider with the BigQuery connector and connect to a data source.
---

# BigQuery Connector

## Installation

Ensure you have already installed the [gcloud CLI](https://cloud.google.com/sdk/docs/install), and then install PipeRider with the BigQuery connector.

```
pip install 'piperider[bigquery]'
```

## Configure connection settings

Initialize a new PipeRider project using `piperider init` and when prompted select BigQuery as the data source.

The following information is required.

* GCP Project ID
* Authentication method (oauth or service-account)
* BigQuery dataset name

### Example initialization steps

```
$ piperider init
Initialize piperider to path /path/to/your/project/.piperider
[?] What is your project name? (alphanumeric only): dataproject
[?] Which data source would you like to connect to?: bigquery
   snowflake
 > bigquery
   postgres
   sqlite
[?] Authentication Methods:
 > oauth
   service-account
```

#### Oauth steps

```
Please enter the following fields for bigquery
[?] GCP Project ID: your-id
[?] Authentication Methods: oauth
 > oauth
   service-account

[?] Pick a dataset: {"project": "xx_projet-id_xx", "dataset": "yy_dataset_yy"}
```

{% hint style="warning" %}
If you don't see the expected project-id from the list, please modify _`~/.config/gcloud/application_default_credentials.json`_ and replace **quota\_project\_id** with \*\*\*\* the one you expect\*\*.\*\*
{% endhint %}

#### Service-account step

```
[?] The path of GCP Service Account Key File: /path/to/key/file
[?] The name of BigQuery DataSet: dataset-name
```

## Test connection settings

After configuring your connection settings, ensure that PipeRider can connect to your BigQuery data source.

```
piperider diagnose
```

### gcloud authentication

<<<<<<< HEAD
If you have not previously authenticated with gcloud, the output of `diagnose` will prompt you to execute a gcloud command to authenticate. This will open a new browser window and you will be prompted to authenticate with your Google account.
=======
If you have not previously authenticated with gcloud, the output of `diagnose` will prompt you to execute a gcloud command to authenticate. This will open a new browser window  and you will be prompted to authenticate with your Google account.
>>>>>>> main
