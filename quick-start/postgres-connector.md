# Postgres Connector

## Installation

It will install the necessary Postgres connector for Piperider CLI.

```
pip install piperider[postgres]
```

## Prepare Credentials of Postgres

Initiate a project with Postgres as the data source by `piperider init`, it requires credentials for Postgres connection.

These information as below are required/optional,&#x20;

* Host URL
* Port: 5432 (default)
* Username
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
Account:
Username:
Password:
Role (optional):
Database:
Warehouse:
Schema:
```

{% hint style="info" %}
If you see the message `Please run 'pip install piperider[postgres]' to get the postgres connector`, it means there is no installed connector. Please run the command to install it.
{% endhint %}

