---
description: Install PipeRider with the Parquet connector and load the Parquet file .
---

# Parquet Connector

{% hint style="warning" %}
non-dbt use case is deprecated since v0.25.0
{% endhint %}

## Installation

Under the hood, PipeRider loads Parquet files by DuckDB, so please install PipeRider with DuckDB.

```
pip install 'piperider[parquet]'
```

## Configuration

### Example initialization steps

```
$ piperider init
Initialize piperider to path /path/to/your/project/.piperider
[?] What is your project name? (alphanumeric only): dataproject
[?] Which data source would you like to connect to?: parquet
 > parquet
```

Enter the path or URL to your Parquet file.

{% hint style="info" %}
The local file path, Http URL and S3 are supported.
{% endhint %}

```
Please enter the following fields for parquet
[?] Path of the Parquet file. (Support: file path, http and s3):
```

## Test connection settings

After configuring your connection settings, ensure that PipeRider can connect to your Parquet file.

```
piperider diagnose
```
