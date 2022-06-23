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

| Option             | Argument | Description                                                   |
| ------------------ | -------- | ------------------------------------------------------------- |
| `--datasource`     | name     | Profile a specified data source                               |
| `--table`          | name     | Profile a specified table only                                |
| `--output`         | path     | Specify the path for saving generated profiling `.json` files |
| `--no-interaction` | none     | Generate assertion templates by default without a prompt      |
| `--skip-report`    | none     | Don't generate reports                                        |
| `--debug`          | none     | Enable debugging output                                       |
| `--help`           | none     | List command-line options                                     |

## Generate Assertions

Generate recommended assertion files in `.piperider/assertions/` . You may want to generate recommended once profiling results are changed.

```
piperider generate-assertions
```

| Option    | Argument         | Description                                                                     |
| --------- | ---------------- | ------------------------------------------------------------------------------- |
| `--input` | path/to/run.json | Generate a recommended assertions based on a specific profiling `run.json` file |
| `--debug` | none             | Enable debugging mode                                                           |
| `--help`  | none             | List command-line options                                                       |

## Generate Report

`generate-report` will generate static HTML reports in `.piperider/outputs/<run>` based on the profiling results of the latest `run` by default.

```shell
piperider generate-report [Options]
```

| Option    | Argument         | Description                                                     |
| --------- | ---------------- | --------------------------------------------------------------- |
| `--input` | path/to/run.json | Generate a report based on a specific profiling `run.json` file |
| `--debug` | none             | Enable debugging output                                         |
| `--help`  | none             | List command-line options                                       |

## Compare Report

`compare-report` will compare two reports by the selection and generate a static HTML comparison report at `.piperider/comparisons/` for each comparison.

```shell
piperider compare-report [Options]
```

| Option    | Argument | Description                                                    |
| --------- | -------- | -------------------------------------------------------------- |
| `--base`  | path     | Specify a profiling `run.json` as the base report              |
| `--input` | path     | Specify a profiling `run.json` to compare with the base report |
| `--debug` | none     | Enable debugging output                                        |
| `--help`  | none     | List command-line options                                      |

## Diagnose Project Configuration

`debug` shows connections statuses of data sources and checks the current configuration/assertions of the project.

```shell
piperider diagnose
```

| Option    | Argument | Description               |
| --------- | -------- | ------------------------- |
| `--debug` | none     | Enable debugging output   |
| `--help`  | none     | List command-line options |
|           |          |                           |
