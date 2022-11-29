---
description: PipeRider commands and related options.
---

# Command Reference

## Initialize project

```shell
piperider init [OPTIONS]
```

`init` will lead you through configuring a new data project according to the type of the data source selected. Refer to [Supported Data Sources](supported-data-sources/) for details of available connectors.

Initializing a project will generate a `.piperider` directory containing project configuration files such as `.piperider/config.yml` , `.piperider/credentials.yml` and others according to user inputs under your project directory.

| Option               | Argument | Description                                                         |
| -------------------- | -------- | ------------------------------------------------------------------- |
| `--no-auto-search`   |          | Disable the auto search for dbt projects                            |
| `--dbt-project-dir`  | path     | Specify the path to the directory of a _dbt project_ configuration  |
| `--dbt-profiles-dir` | path     | Specify the path to the directory of a _dbt profiles_ configuration |
| `--debug`            |          | Enable the debug mode                                               |
| `--help`             |          | List available commands                                             |

## Run profiler

```shell
piperider run [OPTIONS]
```

The `run` command performs the following functions.

* Analyze the data source and generate a data profile.
* (If assertion files exist) Test the data profile against any existing assertions located in `.piperider/assertions`.
* Generate a report in `.piperider/outputs`.

{% hint style="info" %}
Check [assertion configuration](data-quality-assertions/assertion-configuration.md) for more information on built-in assertions
{% endhint %}

| Option           | Argument | Description                                           |
| ---------------- | -------- | ----------------------------------------------------- |
| `--datasource`   | name     | Profile a specified data source                       |
| `--debug`        |          | Enable debugging output                               |
| `--help`         |          | List command-line options                             |
| `-o`, `--output` | path     | Specify the output directory for the generated report |
| `--report-dir`   | path     | Specify the path to read and write reports            |
| `--skip-report`  |          | Don't generate reports                                |
| `--table`        |          | Profile a specified table only                        |

## Generate assertions

```
piperider generate-assertions
```

Generate recommended assertion files in `.piperider/assertions/` . You may want to generate recommended once profiling results are changed.

| Option           | Argument         | Description                                                                     |
| ---------------- | ---------------- | ------------------------------------------------------------------------------- |
| `--no-recommend` |                  | Generate skeleton assertion files, without recommended assertions               |
| `--input`        | path/to/run.json | Generate a recommended assertions based on a specific profiling `run.json` file |
| `--table`        | table name       | Generate recommended assertions for a specific table                            |
| `--debug`        |                  | Enable debugging mode                                                           |
| `--help`         |                  | List command-line options                                                       |

## Generate report

```shell
piperider generate-report [Options]
```

Generate static HTML reports in `.piperider/outputs/<run>`  based on the profiling results of the latest `run` by default.

| Option           | Argument         | Description                                                     |
| ---------------- | ---------------- | --------------------------------------------------------------- |
| `--debug`        |                  | Enable debugging output                                         |
| `--help`         |                  | List command-line options                                       |
| `--input`        | path/to/run.json | Generate a report based on a specific profiling `run.json` file |
| `-o`, `--output` | path             | Specify the output directory for the generated report           |
| `--report-dir`   | path             | Specify the path to read and write reports                      |

## Compare reports

```shell
piperider compare-reports [Options]
```

Compare two selected reports and generate the following output:

* Comparison report (HTML)
* Comparison summary (Markdown)

Comparison reports and summaries are stored in `.piperider/comparisons/<timestamp>`.

| Option           | Argument                                                  | Description                                                                                                           |
| ---------------- | --------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `--base`         | path                                                      | Specify the profiling `run.json` to use as the base report                                                            |
| `--datasource`   | data source name                                          | <p>Specify the data source to use for the report</p><p>comparison (defined in <code>.piperider/config.yml</code>)</p> |
| `--debug`        |                                                           | Enable debugging output                                                                                               |
| `--help`         |                                                           | List command-line options                                                                                             |
| `--last`         |                                                           | Compare the last two reports                                                                                          |
| `-o`, `--output` | path                                                      | Specify the output directory for the generated report                                                                 |
| `--report-dir`   | path                                                      | Specify the path to read and write reports                                                                            |
| `--tables-from`  | <p><code>base-only</code><br><code>target-only</code></p> | Specify to only compare tables that appear in either base or target reports                                           |
| `--target`       | path                                                      | Specify the profiling `run.json` to compare with the base report                                                      |

## PipeRider Cloud

PipeRider Cloud is currently in beta, for more information please refer to the [Get Started](../cloud-beta/get-started.md) guide.

### Login

```
piperider cloud login
```

`cloud login` will lead you to signup/login the PipeRider Cloud. Using the command to have the authorization with a valid _Token_. You will receive an authorizing email by giving your email address. The _Token_ will be stored at `~/.pipeirder/profile.yml`.

| Option                  | Argument     | Description                |
| ----------------------- | ------------ | -------------------------- |
| `--token`               | token string | Specify the API Token      |
| `--enable-auto-upload`  |              | Enable the auto-uploading  |
| `--disable-auto-upload` |              | Disable the auto-uploading |
| `--debug`               |              | Enable debugging output    |
| `--help`                |              | List command-line options  |

### Logout

```
piperider cloud logout
```

`cloud logout` will clear up the token string stored at `~/.piperider/profile.yml`.

| Option    | Argument | Description               |
| --------- | -------- | ------------------------- |
| `--debug` |          | Enable debugging output   |
| `--help`  |          | List command-line options |
|           |          |                           |

### Upload report

```
piperider cloud upload-report
```

`upload-report` will lead you to select single/multiple profiling reports and upload them to the PipeRider Cloud.

| Option         | Argument                 | Description                                       |
| -------------- | ------------------------ | ------------------------------------------------- |
| `--run`        | path/to/run.json         | Specify a profiling result file                   |
| `--report-dir` | path/to/report directory | Use a different report directory from the default |
| `--datasource` | data source name         | Specify a data source                             |
| `--debug`      |                          | Enable debugging output                           |
| `--help`       |                          | List command-line options                         |
|                |                          |                                                   |

## Diagnose project configuration

```shell
piperider diagnose
```

Show data source connection status and check the current configuration and data assertions format.

| Option    | Argument | Description               |
| --------- | -------- | ------------------------- |
| `--debug` |          | Enable debugging output   |
| `--help`  |          | List command-line options |
|           |          |                           |

## Submit feedback

```
piperider feedback
```

We love to hear feedback from the community. Help us improve PipeRider by sending your suggestions.
