---
description: How to use PipeRider with dbt
---

# dbt Integration

Data transformations are an integral part of the modern data stack. Through PipeRider's integration with [dbt](https://www.getdbt.com/) you can run data quality checks against your transformed data and ELT pipeline. No extra configuration is required.&#x20;

When [initializing](../piperider-cli.md#initialize-project) a new project, PipeRider will search the current folder for your `dbt_project.yaml` and create a `.piperider/config.yml` file according to your dbt settings.

Example `.piperider/config.yml` with dbt connection settings:

```yaml
dataSources:
- name: my_dbt_project
  type: snowflake
  dbt:
    profile: my_dbt_project 
    target: dev #which target to load for the given profile
    projectDir: . #the path to the directory where dbt_project.yml is
    profilesDir: ~/.dbt #the path to the direcotry where profiles.yml is
```

The dbt-related fields are:

| Field       | Description                                          | Required                                                                                                                              |
| ----------- | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| profile     | which profile to use                                 | Yes                                                                                                                                   |
| target      | which target to use for the given profile            | Yes                                                                                                                                   |
| projectDir  | the path to the directory where `dbt_project.yml` is | Yes                                                                                                                                   |
| profilesDir | the path to the directory where `profiles.yml` is    | <p><code>~/.dbt</code> by default; optional<br>It can be overwritten by the environmental variable <code>DBT_PROFILES_DIR</code>.</p> |

### Under the hood

`piperider run` will automatically:

1. Use the corresponding credentials in `~/.dbt/profiles` to make the connection.
2. Parse `./target/catalog.json` and look for the available tables.
3. Profile and check the data in these tables.

### Specify dbt data source

If you have more than one dbt project, such as a develop and production data source, you can specify the name of the datasource with the  `--datasource` option.

```shell
piperider run --datasource <name of the data source>
```

Example dbt profile with multiple data sources:

```yaml
dataSources:
- name: my_dbt_project # for development
  type: snowflake
  dbt:
    profile: my_dbt_project
    target: dev
    projectDir: .
- name: my_dbt_project_prod # for productionm
  type: snowflake
  dbt:
    profile: my_dbt_project
    target: prod
    projectDir: .    
```

Specify the data source to use:

```
piperider run --datasource my_dbt_project_prod
```
