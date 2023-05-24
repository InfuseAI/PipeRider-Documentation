---
description: Install PipeRider with the SQLite connector and connect to a data source .
---

# SQLite connector

{% hint style="warning" %}
non-dbt use case is deprecated since v0.25.0
{% endhint %}

## Installation

Install PipeRider. SQLite connector is enabled by default in PipeRider

```
pip install 'piperider'
```

## Configuration

```
$ piperider init
Initialize piperider to path /path/to/your/project/.piperider
[?] What is your project name? (alphanumeric only): dataproject
[?] Which data source would you like to connect to?: sqlite
 > sqlite
```

Enter the path to the directory where the database files are.

```
Please enter the following fields for sqlite
[?] Path of database file:
```

## Test connection settings

After configuring your connection settings, ensure that PipeRider can connect to your sqlite.

```
piperider diagnose
```
