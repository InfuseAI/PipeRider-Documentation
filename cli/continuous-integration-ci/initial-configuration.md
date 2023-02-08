---
description: dbt and PipeRider project configuration for CI
---

# Initial Configuration

The concept of using PipeRider with your CI process is based on there being a PR (pull request) environment (schema and data source), in addition to the usual 'dev' and 'prod' environments.

The following CI examples use dbt ([Jaffle Shop](https://github.com/dbt-labs/jaffle\_shop) project) and Snowflake, and the configuration files for dbt and PipeRider are configured as follows.

## dbt profile

The dbt profile contains three [environments](https://docs.getdbt.com/docs/collaborate/environments), dev, pr, and prod.

{% code title="~/.dbt/profiles.yml" %}
```yaml
# dbt profile
jaffle_shop:
  target: dev
  outputs:
    dev:
      type: snowflake
      schema: DEV
      account: <your snowflake setup>
      user: <your snowflake setup>
      password: <your snowflake setup>
      role: <your snowflake setup>
      database: <your snowflake setup>
      warehouse: <your snowflake setup>
      threads: <your snowflake setup>
    pr:
      type: snowflake
      schema: PR
      account: <your snowflake setup>
      user: <your snowflake setup>
      password: <your snowflake setup>
      role: <your snowflake setup>
      database: <your snowflake setup>
      warehouse: <your snowflake setup>
      threads: <your snowflake setup>
    prod:
      type: snowflake
      schema: PUBLIC
      account: <your snowflake setup>
      user: <your snowflake setup>
      password: <your snowflake setup>
      role: <your snowflake setup>
      database: <your snowflake setup>
      warehouse: <your snowflake setup>
      threads: <your snowflake setup>
```
{% endcode %}

View this example file on GitHub:

{% embed url="https://github.com/InfuseAI/jaffle_shop/blob/main/profiles.yml" %}
example dbt profiles.yml for multiple data sources/schema
{% endembed %}

As mentioned above, in addition to the production schema,  there is another 'PR' schema that will be used  as a staging schema to run the new model transformation in the pull request.

### PipeRider project config

Corresponding to the dbt profile setup, PipeRider also needs additional data sources to link to the separate dbt profiles, dev, pr, and prod.&#x20;

For initial dbt project configuration please refer to the [dbt integration](../dbt-integration.md) page. Additional data sources must be manually added to the PipeRider config:

{% code title=".piperider/config.yml" %}
```yaml
# piperider config
dataSources:
- name: jaffle_shop_dev
  type: snowflake
  dbt:
    profile: jaffle_shop
    target: dev
    projectDir: .
- name: jaffle_shop_pr
  type: snowflake
  dbt:
    profile: jaffle_shop
    target: pr
    projectDir: .
- name: jaffle_shop
  type: snowflake
  dbt:
    profile: jaffle_shop
    target: prod
    projectDir: .
```
{% endcode %}

View this example file on GitHub:

{% embed url="https://github.com/InfuseAI/jaffle_shop/blob/main/.piperider/config.yml" %}
Example PipeRider config.yml for multiple data sources
{% endembed %}
