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

### Initialize a new Snowflake PipeRider project

```shell-session
$ piperider init
[?] What is your data source name?: SnowflakeProject
[?] Which data source would you like to connect to?:
 > snowflake
```

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
