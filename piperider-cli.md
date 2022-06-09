---
description: All of commands
---

# PipeRider CLI

## Initialize Project

`init` will lead you to configure the data project according to the type of the data source you choose. Piperider CLI currently supports _Snowflake_, _Postgres_ and _Sqlite_.

It will generate a `.piperider` directory and a few configuration files inside such as `.piperider/config.yml` , `.piperider/credentials.yml` and others according to user inputs under your project directory.

```shell
piperider init [OPTIONS]
```

| Option               | Argument | Description                                                         |
| -------------------- | -------- | ------------------------------------------------------------------- |
| `--no-auto-search`   | none     | Disable the auto search for dbt projects; it is enabled by default  |
| `--dbt-project-dir`  | path     | Specify the path to the directory of a _dbt project_ configuration  |
| `--dbt-profiles-dir` | path     | Specify the path to the directory of a _dbt profiles_ configuration |
| `--debug`            | none     | Enable the debug mode                                               |
| `--help`             | none     | List command-line options                                           |

## Run profiler and Check Data Quality

`run` will profile tables/columns and save profiling results at `.piperider/outputs` for each run. Also, it will check the data quality by assertions at `.piperider/assertions`.

{% hint style="info" %}
Regarding how to configure assertions, please check the [assertion configuration](assertion-configuration.md) for the detail.
{% endhint %}

```shell
piperider run [OPTIONS]
```

| Option              | Argument | Description                                                                      |
| ------------------- | -------- | -------------------------------------------------------------------------------- |
| `--datasource`      | name     | Profile a specified data source                                                  |
| `--table`           | name     | Profile a specified table                                                        |
| `--output`          | path     | Specify the path for saving generated profiling `.json` files.                   |
| `--no-interaction`  | none     | Generate assertion scaffoldings by default without a prompt.                     |
| `--generate-report` | none     | Profile and generate static HTML reports at `.piperider/reports/` for each table |
| `--help`            | none     | List command-line options                                                        |

## Generate Report

`generate-report` will generate static HTML reports at `.piperider/reports/` based on, by default, profiling results of the latest `run`.

```shell
piperider generate-report [Options]
```

| Option    | Argument              | Description                                                    |
| --------- | --------------------- | -------------------------------------------------------------- |
| `--input` | path/to/\<table>.json | Generate a report referring to a specified profiling json file |
| `--debug` | none                  | Enable the debug mode                                          |
| `--help`  | none                  | List command-line options                                      |

## Compare Report

`compare-report` will compare two reports by the selection and generate a static HTML comparison report at `.piperider/comparisons/` for each comparison.

```shell
piperider compare-report [Options]
```

| Option    | Argument | Description                                    |
| --------- | -------- | ---------------------------------------------- |
| `--base`  | path     | Specify a profiling json as the base           |
| `--input` | path     | Specify a profiling json comparing to the base |
| `--debug` | none     | Enable the debug mode                          |
| `--help`  | none     | List command-line options                      |

## Diagnose Project Configuration

`debug` shows connections statuses of data sources and checks the current configuration/assertions of the project.

```shell
piperider diagnose
```
