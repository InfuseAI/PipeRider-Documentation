---
description: How to use PipeRider with dbt.
---

# dbt Integration

Data transformations are an integral part of the modern data stack. Through PipeRider's integration with [dbt](https://www.getdbt.com/) you can run data quality checks against your transformed data and ELT pipeline. PipeRider auto-detects your dbt data warehouse settings, so no extra configuration is required.

When [initializing](../piperider-cli.md#initialize-project) a new project, PipeRider will search the current path for your `dbt_project.yml` and create a `.piperider/config.yml` file according to your dbt settings. If multiple dbt projects exist in the current path, you will be promoted to select a dbt project. Data warehouse connection settings will be sourced from `~/.dbt/profiles.yml` unless specified.

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

The dbt-related data source fields are:

| Field       | Description                                             | Required                                                                                                                   |
| ----------- | ------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| profile     | dbt profile to use                                      | Yes,                                                                                                                       |
| target      | Target to use for the given profile                     | Yes                                                                                                                        |
| projectDir  | Path to the directory where `dbt_project.yml` is stored | Yes                                                                                                                        |
| profilesDir | Path to the directory where `profiles.yml` is stored    | <p>No, defaults to <code>~/.dbt</code><br>Can be overwritten by the <code>DBT_PROFILES_DIR</code> environment variable</p> |

### Specify dbt data source

If you have more than one dbt project, such as a dev and production data source, you can specify the name of the data source with the `--datasource` option.

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
