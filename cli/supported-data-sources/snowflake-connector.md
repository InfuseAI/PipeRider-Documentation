---
description: >-
  Install PipeRider with the Snowflake connector and connect to a Snowflake data
  source.
---

# Snowflake Connector

### Installation

Install PipeRider with the Snowflake connector.

```
pip install 'piperider[snowflake]'
```

### Prepare Credentials of Snowflake

Initialize a new PipeRider project using `piperider init` and when promoted select Snowflake as the data source.

The following information is required.

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
If you see the message **`Please run pip install piperider[snowflake] to get the snowflake connector`** this means that the Snowflake connector is not installed. Please follow the [installation instructions](snowflake-connector.md#installation) above.
{% endhint %}

### Multi-Factor Authentication (MFA)

If multi-factor authentication (MFA) is enabled on your Snowflake account, you will receive a Duo push-notification prompting you to allow access to the data source when executing the following PipeRider functions:

* `piperider diagnose`
* `piperider run`

#### MFA Token Caching

To reduce the number of prompts you receive, Snowflake allows users to [cache MFA tokens](https://docs.snowflake.com/en/user-guide/security-mfa.html#using-mfa-token-caching-to-minimize-the-number-of-prompts-during-authentication-optional) by enabling a setting at the account level.&#x20;

If MFA is enabled on your Snowflake account, please ensure the following.

* The following package is installed:\
  &#x20;`pip install 'snowflake-connector-python[secure-local-storage]'`
*   For non-dbt projects:

    Edit _.piperider/credentials.yml_ and add `authenticator: username_password_mfa`
*   For dbt projects:

    Edit `~/.dbt/profiles` and add the following line to the corresponding profile:\
    `authenticator: username_password_mfa`&#x20;
