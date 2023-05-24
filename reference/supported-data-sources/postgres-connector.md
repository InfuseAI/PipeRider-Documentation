---
description: >-
  Install PipeRider with the Postgres connector and connect to a Postgres data
  source.
---

# Postgres Connector

## Installation

Install PipeRider with the Postgres connector.

```
pip install 'piperider[postgres]'
```

## Configuration (DBT)

Run the diagnose command in the dbt project

```
piperider diagnose
```

If you can successfully connect to Postgres using dbt, PipeRider can also connect to Postgres using the same profile settings. For details, please refer to the dbt [Postgres adapter](https://docs.getdbt.com/reference/warehouse-setups/postgres-setup) documentation.

## Configuration (Non-DBT)

{% hint style="warning" %}
non-dbt use case is deprecated since v0.25.0
{% endhint %}

Initialize a new PipeRider project using `piperider init` and when promoted select Postgres as the data source.

The following information is required.

* Host URL
* Port: 5432 (default)
* Username:
* Password
* Database
* Schema (optional)

```shell-session
piperider init
What data source would you like to connect to?:
> postgres
```

```
Please enter the following fields for postgres
Host URL:
Username:
Password:
Database:
Schema (optional):
```

{% hint style="info" %}
If you see the message **`Please run 'pip install piperider[postgres]' to get the postgres connector`**, this means that the Postgres connector is not installed. Please follow the [installation instructions](postgres-connector.md#installation) above.
{% endhint %}

## Test connection settings

After configuring your connection settings, ensure that PipeRider can connect to your Postgres data source.

```
piperider diagnose
```
