---
description: Install PipeRider with the DuckDB connector and connect to a data source .
---

# DuckDB Connector

## Installation

Install PipeRider with the DuckDB connector.

```
pip install 'piperider[duckdb]'
```

## Configure connection settings

### Example initialization steps

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
