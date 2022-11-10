---
description: PipeRider commands and related options.
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

| Option             | Argument | Description                                              |
| ------------------ | -------- | -------------------------------------------------------- |
| `--datasource`     | name     | Profile a specified data source                          |
| `--dbt-build`      | none     | Run dbt build prior to profiling                         |
| `--dbt-test`       | none     | Run dbt test prior to profiling                          |
| `--debug`          | none     | Enable debugging output                                  |
| `--help`           | none     | List command-line options                                |
| `--no-interaction` | none     | Generate assertion templates by default without a prompt |
| `-o`, `--output`   | path     | Specify the output directory for the generated report    |
| `--report-dir`     | path     | Specify the path to read and write reports               |
| `--skip-recommend` | none     | Don't generate recommended assertions                    |
| `--skip-report`    | none     | Don't generate reports                                   |
| `--table`          | name     | Profile a specified table only                           |

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

| Option           | Argument         | Description                                                     |
| ---------------- | ---------------- | --------------------------------------------------------------- |
| `--debug`        | none             | Enable debugging output                                         |
| `--help`         | none             | List command-line options                                       |
| `--input`        | path/to/run.json | Generate a report based on a specific profiling `run.json` file |
| `-o`, `--output` | path             | Specify the output directory for the generated report           |
| `--report-dir`   | path             | Specify the path to read and write reports                      |

## Compare Reports

`compare-reports` will compare two reports by the selection and generate a static HTML comparison report at `.piperider/comparisons/` for each comparison.

```shell
piperider compare-reports [Options]
```

| Option           | Argument         | Description                                                                                                           |
| ---------------- | ---------------- | --------------------------------------------------------------------------------------------------------------------- |
| `--base`         | path             | Specify the profiling `run.json` to use as the base report                                                            |
| `--datasource`   | data source name | <p>Specify the data source to use for the report</p><p>comparison (defined in <code>.piperider/config.yml</code>)</p> |
| `--debug`        | none             | Enable debugging output                                                                                               |
| `--help`         | none             | List command-line options                                                                                             |
| `--last`         | none             | Compare the last two reports                                                                                          |
| `-o`, `--output` | path             | Specify the output directory for the generated report                                                                 |
| `--report-dir`   | path             | Specify the path to read and write reports                                                                            |
| `--target`       | path             | Specify the profiling `run.json` to compare with the base report                                                      |

## Access Cloud

`piperider cloud` is a set of commands dedicated to the PipeRider Cloud service.

### login

`cloud login` will lead you to signup/login the PipeRider Cloud. Using the command to have the authorization with a valid _Token_. You will receive an authorizing email by giving your email address. The _Token_ will be stored at `~/.pipeirder/profile.yml`.

```
piperider cloud login
```

| Option                  | Argument            | Description                |
| ----------------------- | ------------------- | -------------------------- |
| `--token`               | a string of a token | Specify the API Token      |
| `--enable-auto-upload`  | none                | Enable the auto-uploading  |
| `--disable-auto-upload` | none                | Disable the auto-uploading |
| `--debug`               | none                | Enable debugging output    |
| `--help`                | none                | List command-line options  |

### logout

`cloud logout` will clear up the token string stored at `~/.piperider/profile.yml`.

```
piperider cloud logout
```

| Option    | Argument | Description               |
| --------- | -------- | ------------------------- |
| `--debug` | none     | Enable debugging output   |
| `--help`  | none     | List command-line options |
|           |          |                           |

### upload-report

`upload-report` will lead you to select single/multiple profiling reports and upload them to the PipeRider Cloud.

```
piperider cloud upload-report
```

| Option         | Argument                 | Description                                       |
| -------------- | ------------------------ | ------------------------------------------------- |
| `--run`        | path/to/run.json         | Specify a profiling result file                   |
| `--report-dir` | path/to/report directory | Use a different report directory from the default |
| `--datasource` | datasource name          | Specify a datasource                              |
| `--debug`      | none                     | Enable debugging output                           |
| `--help`       | none                     | List command-line options                         |
|                |                          |                                                   |

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

## Love Your Feedback

We love to hear feedbacks from the community. Please don't hesitate to send the feedback that will help improve PipeRider and assure it on the right direction.

```
piperider feedback
```
