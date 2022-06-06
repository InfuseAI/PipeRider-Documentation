---
description: All of commands
---

# PipeRider CLI

## Initialize Project

`init` will lead you to configure the project according to the type of the data source you choose. Piperider currently support _snowflak_, _postgres_ and _sqlite_.

It will generate a `.piperider` directory under your project directory with relevant configuration files.

```shell
piperider-cli init [OPTIONS]
```

| Option               | Argument | Description                                                        |
| -------------------- | -------- | ------------------------------------------------------------------ |
| `--no-auto-search`   | none     | Disable the auto search for dbt projects; it is enabled by default |
| `--dbt-project-dir`  | path     | Associate to the directory of a dbt project configuration          |
| `--dbt-profiles-dir` | path     | Associate to the directory of a dbt profiles configuration         |
| `--debug`            | none     | Enable the debug mode                                              |
| `--help`             | none     | List command-line options                                          |

## Run profiler and Check Data Quality

`run` will profile tables/columns and save profiling results at `.piperider/outputs` for each run. Also, it will check the data quality by assertions at `.piperider/assertions`.

```shell
piperider-cli run [OPTIONS]
```

| Option              | Argument     | Description                                                                      |
| ------------------- | ------------ | -------------------------------------------------------------------------------- |
| `--datasource`      | name         | Profile a specified data source                                                  |
| `--table`           | name         | Profile a specified table                                                        |
| `--output`          | path/to/save | Generate profiling `json` files for each table at the path                       |
| `--no-interaction`  | none         | ?                                                                                |
| `--generate-report` | none         | Profile and generate static HTML reports at `.piperider/reports/` for each table |
| `--help`            | none         | List command-line options                                                        |

## Generate Report

`generate-report` will generate static HTML reports at `.piperider/reports/` based on, by default, profiling results of the last `run`.

```shell
piperider-cli generate-report [Options]
```

| Option    | Argument              | Description                                           |
| --------- | --------------------- | ----------------------------------------------------- |
| `--input` | path/to/\<table>.json | Generate a report of a specified profiling json file  |
| `--debug` | none                  | Shows where the profiling results of the last run are |
| `--help`  | none                  | List command-line options                             |

## Compare Report

```shell
piperider-cli compare-report [Options]
```

| Option    | Argument | Description                                    |
| --------- | -------- | ---------------------------------------------- |
| `--base`  | path     | Specify a profiling json as the base           |
| `--input` | path     | Specify a profiling json comparing to the base |
| `--debug` | ?        | ?                                              |
| `--help`  | none     | List command-line options                      |

## Diagnose Project Configuration

`debug` shows connections statuses of data sources and checks the current configuration/assertions of the project.

```shell
piperider-cli debug
```
