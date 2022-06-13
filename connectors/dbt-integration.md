---
description: How to use PipeRider with dbt
---

# dbt integration

### dbt Integration

[dbt](https://www.getdbt.com/) is a great tool for the data transformation, and yes, Piperider supports dbt projects. Extra manual configuration is not required.

Piperider will search and load the `dbt_project.yml` file into `./piperider/config.yml` by default when `piperider-cli init` .&#x20;

You should see the dbt-related context of the config.yml like below

e.g.

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

dbt-related fields are

| Field       | Description                                          | Required                      |
| ----------- | ---------------------------------------------------- | ----------------------------- |
| profile     | which profile to use                                 | Yes                           |
| target      | which target to use for the given profile            | Yes                           |
| projectDir  | the path to the directory where `dbt_project.yml` is | Yes                           |
| profilesDir | the path to the directory where `profiles.yml` is    | `~/.dbt` by default; optional |

### Under the hood

`piperider run` will automatically

1. Use the corresponding credentials in `~/.dbt/profiles` to make the connection.
2. Parse `./target/catalog.json` and look for the available tables.
3. Profile and test against these tables.

### Profile/Target Pair

To have different profile/target pair, you can define two data sources and use `--datasource` to specify a data source for a run.

```shell
piperider run --datasource <name of the data source>
```

e.g.

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
