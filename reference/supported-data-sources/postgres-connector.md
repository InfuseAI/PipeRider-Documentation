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

## Prepare Credentials of Postgres

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
