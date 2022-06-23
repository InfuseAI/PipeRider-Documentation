---
description: All of commands
---

# Command Reference

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

## Run profiler, assertions, report

The `run` command performs the following functions.

On first run:

* Analyzes the data source and generates a data profile.
* Offers to generate recommended assertions and check the data profile against these them. Assertion files will be generated in `.piperider/assertions`.
* Generates blank assertion templates if recommended assertions were not created.
* Generates a report in `.piperider/outputs`.

On subsequent runs:

* Analyzes the data source and generates a data profile.
* Checks the data profile against any existing assertions located in `.piperider/assertions`.
* Generates a report in `.piperider/outputs`.

{% hint style="info" %}
Regarding how to configure assertions, please check the [assertion configuration](data-quality-assertions/assertion-configuration.md) for the detail.
{% endhint %}

```shell
piperider run [OPTIONS]
```

| Option             | Argument | Description                                                    |
| ------------------ | -------- | -------------------------------------------------------------- |
| `--datasource`     | name     | Profile a specified data source                                |
| `--table`          | name     | Profile a specified table only                                 |
| `--output`         | path     | Specify the path for saving generated profiling `.json` files. |
| `--no-interaction` | none     | Generate assertion scaffoldings by default without a prompt.   |
| `--skip-report`    | none     | Don't generate reports                                         |
| `--debug`          | none     | Enable the debug mode                                          |
| `--help`           | none     | List command-line options                                      |

## Generate Assertions

Generate recommended assertion files in `.piperider/assertions/` .

```
piperider generate-assertions
```

## Generate Report

`generate-report` will generate static HTML reports in `.piperider/outputs/<run>` based on the profiling results of the latest `run` by default.

```shell
piperider generate-report [Options]
```

| Option    | Argument         | Description                                                          |
| --------- | ---------------- | -------------------------------------------------------------------- |
| `--input` | path/to/run.json | Generate a report referring to a specified profiling `run.json` file |
| `--debug` | none             | Enable the debug mode                                                |
| `--help`  | none             | List command-line options                                            |

## Compare Report

`compare-report` will compare two reports by the selection and generate a static HTML comparison report at `.piperider/comparisons/` for each comparison.

```shell
piperider compare-report [Options]
```

| Option    | Argument | Description                                          |
| --------- | -------- | ---------------------------------------------------- |
| `--base`  | path     | Specify a profiling `run.json` as the base           |
| `--input` | path     | Specify a profiling `run.json` comparing to the base |
| `--debug` | none     | Enable the debug mode                                |
| `--help`  | none     | List command-line options                            |

## Diagnose Project Configuration

`debug` shows connections statuses of data sources and checks the current configuration/assertions of the project.

```shell
piperider diagnose
```

| Option    | Argument | Description               |
| --------- | -------- | ------------------------- |
| `--debug` | none     | Enable the debug mode     |
| `--help`  | none     | List command-line options |
|           |          |                           |
