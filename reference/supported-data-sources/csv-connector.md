---
description: Install PipeRider with the CSV connector and load the CSV file .
---

# CSV Connector

{% hint style="warning" %}
non-dbt use case is deprecated since v0.25.0
{% endhint %}

## Installation

Under the hood, PipeRider loads CSV files by DuckDB, so please install PipeRider with DuckDB.

```
pip install 'piperider[csv]'
```

## Configuration

### Example initialization steps

```
$ piperider init
Initialize piperider to path /path/to/your/project/.piperider
[?] What is your project name? (alphanumeric only): dataproject
[?] Which data source would you like to connect to?: csv
 > csv
```

Enter the path to your CSV file.

```
Please enter the following fields for csv
[?] Path of csv file:
```

## Test connection settings

After configuring your connection settings, ensure that PipeRider can connect to your CSV file.

```
piperider diagnose
```
