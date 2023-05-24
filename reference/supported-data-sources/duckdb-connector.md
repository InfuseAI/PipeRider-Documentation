---
description: Install PipeRider with the DuckDB connector and connect to a data source .
---

# DuckDB Connector

## Installation

Install PipeRider with the DuckDB connector.

```
pip install 'piperider[duckdb]'
```

## Configuration (DBT)

Run the diagnose command in the dbt project

```
piperider diagnose
```

If you can successfully connect to DuckDB using dbt, PipeRider can also connect to DuckDB using the same profile settings. For details, please refer to the dbt [DuckDB adapter](https://docs.getdbt.com/reference/warehouse-setups/duckdb-setup) documentation.

## Configuration (Non-DBT)

{% hint style="warning" %}
non-dbt use case is deprecated since v0.25.0
{% endhint %}

```
$ piperider init
Initialize piperider to path /path/to/your/project/.piperider
[?] What is your project name? (alphanumeric only): dataproject
[?] Which data source would you like to connect to?: duckdb
 > duckdb
```

Enter the path to the directory where the database files are.

```
Please enter the following fields for duckdb
[?] Path of database file:
```

## Test connection settings

After configuring your connection settings, ensure that PipeRider can connect to your DuckDB.

```
piperider diagnose
```
