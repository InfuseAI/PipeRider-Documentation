---
description: >-
  Install PipeRider with the Snowflake connector and connect to a Snowflake data
  source.
---

# Snowflake Connector

## Installation

```
pip install 'piperider[snowflake]'
```

## Configuration (DBT)

Run the initialization command in the dbt project

```
piperider init
```

If you can successfully connect to Snowflake using dbt, PipeRider can also connect to Snowflake using the same profile settings. For details, please refer to the dbt [Snowflake adapter](https://docs.getdbt.com/reference/warehouse-setups/snowflake-setup) documentation.

## Configuration (Non-DBT)

### Initialize a new Snowflake PipeRider project

```shell-session
piperider init
[?] What is your data source name?: SnowflakeProject
[?] Which data source would you like to connect to?:
 > snowflake
```

Follow the prompts entering the required information.

### Required information

After selecting Snowflake as your data source, you will be promoted for the following information:

* Account
* Username
* Authentication method
  * Password
  * Keypair (private key path, optional passphrase)
  * SSO ('externalbrowser' or a valid Okta URL)
* Role (optional)
* Database
* Warehouse
* Schema

### Multi-Factor Authentication (MFA)

If multi-factor authentication (MFA) is enabled on your Snowflake account, you will receive a Duo push-notification prompting you to allow access to the data source when executing the following PipeRider functions:

* `piperider diagnose`
* `piperider run`

### MFA Token Caching

To reduce the number of prompts you receive, Snowflake allows users to [cache MFA tokens](https://docs.snowflake.com/en/user-guide/security-mfa.html#using-mfa-token-caching-to-minimize-the-number-of-prompts-during-authentication-optional) by enabling a setting at the account level.&#x20;

If MFA is enabled on your Snowflake account, please ensure the following.

* The following package is installed:\
  &#x20;`pip install 'snowflake-connector-python[secure-local-storage]'`
*   For non-dbt projects:

    Edit _.piperider/credentials.yml_ and add `authenticator: username_password_mfa`
*   For dbt projects:

    Edit `~/.dbt/profiles` and add the following line to the corresponding profile:\
    `authenticator: username_password_mfa`&#x20;

## Test connection settings

After configuring your connection settings, ensure that PipeRider can connect to your Snowflake data source.

```
piperider diagnose
```
