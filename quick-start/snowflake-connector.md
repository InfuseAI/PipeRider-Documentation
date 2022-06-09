# Snowflake Connector

## Installation

It will install the necessary Snowflake connector for Piperider CLI.

```
pip install piperider[snowflake]
```

## Prepare Credentials of Snowflake

Initiate a project with Snowflake as the data source by `piperider init`, it requires credentials for Snowflake connection.

These information as below are required,&#x20;

* Account
* Username
* Password
* Role (optional)
* Database
* Warehouse
* Schema

```shell-session
piperider init
What data source would you like to connect to?:
> snowflake
```

```
Please enter the following fields for snowflake
Account:
Username:
Password:
Role (optional):
Database:
Warehouse:
Schema:
```

{% hint style="info" %}
If you see the message `Please run pip install piperider[snowflake] to get the snowflake connector`,  it means there is no installed connector. Please run the command to install it.
{% endhint %}

