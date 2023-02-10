---
description: PipeRider commands and related options.
---

# Command Reference

## Initialize project

```shell
piperider init [options]
```

`init` will lead you through configuring a new data project according to the type of the data source selected. Refer to [Supported Data Sources](supported-data-sources/) for details of available connectors.

Initializing a project will generate a `.piperider` directory containing project configuration files such as `.piperider/config.yml` , `.piperider/credentials.yml` and others according to user inputs under your project directory.

| Option               | Argument | Description                                                         |
| -------------------- | -------- | ------------------------------------------------------------------- |
| `--no-auto-search`   | N/A      | Disable the auto search for dbt projects                            |
| `--dbt-project-dir`  | Path     | Specify the path to the directory of a _dbt project_ configuration  |
| `--dbt-profiles-dir` | Path     | Specify the path to the directory of a _dbt profiles_ configuration |
| `--debug`            | N/A      | Enable the debug mode                                               |
| `--help`             | N/A      | List available commands                                             |

## Run profiler

```shell
piperider run [options]
```

The `run` command performs the following functions.

* Analyze the data source and generate a data profile.
* (If assertion files exist) Test the data profile against any existing assertions located in `.piperider/assertions`.
* Generate a report in `.piperider/outputs`.

{% hint style="info" %}
Check [assertion configuration](broken-reference) for more information on built-in assertions
{% endhint %}

| Option           | Argument   | Description                                                                                                                                         |
| ---------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `--datasource`   | Name       | Profile a specified data source                                                                                                                     |
| `-o`, `--output` | Path       | Specify the output directory for the generated report                                                                                               |
| `--open`         | N/A        | Automatically opens the report in your default browser. If the report was uploaded to PipeRider Cloud, then the PipeRider Cloud URL will be opened  |
| `--report-dir`   | Path       | Specify the path to read and write reports                                                                                                          |
| `--skip-report`  | N/A        | Don't generate reports                                                                                                                              |
| `--upload`       | N/A        | For PipeRider Cloud users - uploads the generated report to PipeRider Cloud (overrides the global profile.yml setting)                              |
| `--table`        | Table name | Profile a specified table only                                                                                                                      |
| `--debug`        | N/A        | Enable debugging output                                                                                                                             |
| `--help`         | N/A        | List command-line options                                                                                                                           |

## Generate assertions

```
piperider generate-assertions
```

Generate recommended assertion files in `.piperider/assertions/` . You may want to generate recommended once profiling results are changed.

| Option           | Argument                    | Description                                                                     |
| ---------------- | --------------------------- | ------------------------------------------------------------------------------- |
| `--no-recommend` | N/A                         | Generate skeleton assertion files, without recommended assertions               |
| `--input`        | Path to the `run.json` file | Generate a recommended assertions based on a specific profiling `run.json` file |
| `--table`        | Table name                  | Generate recommended assertions for a specific table                            |
| `--debug`        | N/A                         | Enable debugging mode                                                           |
| `--help`         | N/A                         | List command-line options                                                       |

## Generate report

```shell
piperider generate-report [options]
```

Generate static HTML reports in `.piperider/outputs/<run>`  based on the profiling results of the latest `run` by default.

| Option           | Argument                    | Description                                                     |
| ---------------- | --------------------------- | --------------------------------------------------------------- |
| `--input`        | Path to the `run.json` file | Generate a report based on a specific profiling `run.json` file |
| `-o`, `--output` | Path                        | Specify the output directory for the generated report           |
| `--report-dir`   | Path                        | Specify the path to read and write reports                      |
| `--debug`        | N/A                         | Enable debugging output                                         |
| `--help`         | N/A                         | List command-line options                                       |

## Compare reports

```shell
piperider compare-reports [options]
```

Compare two selected reports and generate the following output:

* Comparison report (HTML)
* Comparison summary (Markdown)

Comparison reports and summaries are stored in `.piperider/comparisons/<timestamp>`.

| Option           | Argument                                                  | Description                                                                                                           |
| ---------------- | --------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `--base`         | Path to the `run.json` file                               | Specify the profiling `run.json` to use as the base report                                                            |
| `--datasource`   | Data source name                                          | <p>Specify the data source to use for the report</p><p>comparison (defined in <code>.piperider/config.yml</code>)</p> |
| `--last`         | N/A                                                       | Compare the last two reports                                                                                          |
| `-o`, `--output` | Path                                                      | Specify the output directory for the generated report                                                                 |
| `--report-dir`   | Path                                                      | Specify the path to read and write reports                                                                            |
| `--tables-from`  | <p><code>base-only</code><br><code>target-only</code></p> | Specify to only compare tables that appear in either base or target reports                                           |
| `--target`       | Path to the `run.json` file                               | Specify the profiling `run.json` to compare with the base report                                                      |
| `--debug`        | N/A                                                       | Enable debugging output                                                                                               |
| `--help`         | N/A                                                       | List command-line options                                                                                             |

## PipeRider Cloud

PipeRider Cloud is currently in beta, for more information please refer to the [Get Started](../piperider-cloud/get-started.md) guide.

### Login

```
piperider cloud login
```

`cloud login` will lead you to signup/login the PipeRider Cloud. Using the command to have the authorization with a valid _Token_. You will receive an authorizing email by giving your email address. The _Token_ will be stored at `~/.pipeirder/profile.yml`.

| Option                  | Argument     | Description                |
| ----------------------- | ------------ | -------------------------- |
| `--token`               | Token string | Specify the API Token      |
| `--enable-auto-upload`  | N/A          | Enable the auto-uploading  |
| `--disable-auto-upload` | N/A          | Disable the auto-uploading |
| `--debug`               | N/A          | Enable debugging output    |
| `--help`                | N/A          | List command-line options  |

### Logout

```
piperider cloud logout
```

`cloud logout` will clear up the token string stored at `~/.piperider/profile.yml`.

| Option    | Argument | Description               |
| --------- | -------- | ------------------------- |
| `--debug` | N/A      | Enable debugging output   |
| `--help`  | N/A      | List command-line options |

### Upload report

```
piperider cloud upload-report
```

`upload-report` allows you to select single/multiple profiling reports and upload them to PipeRider Cloud.

| Option         | Argument                     | Description                                       |
| -------------- | ---------------------------- | ------------------------------------------------- |
| `--run`        | Path to the `run.json` file  | Specify a profiling result file                   |
| `--report-dir` | Path to the report directory | Use a different report directory from the default |
| `--datasource` | Data source name             | Specify a data source                             |
| `--debug`      | N/A                          | Enable debugging output                           |
| `--help`       | N/A                          | List command-line options                         |
|                |                              |                                                   |

### Compare Reports

```
piperider cloud compare-reports --base <base-report> --target <target-report>
```

`compare-reports` will compare the two specified reports (`base` and `target`) and generate a comparison report in your PipeRider Cloud account.&#x20;

| Option                | Argument                                       | Description                                        |
| --------------------- | ---------------------------------------------- | -------------------------------------------------- |
| `--base` (required)   | A report ID (numeric), or the data source name | The report to use as a base for the comparison     |
| `--target` (required) | A report ID (numeric), or the data source name | The report to use as the target for the comparison |
| `--summary-file`      | Desired filename for this file                 | Output the comparison summary as a Markdown file   |
| `--help`              | N/A                                            | List command-line options                          |
|                       |                                                |                                                    |

#### Examples

Compare reports with the ID `123` and `456` and save the Markdown comparison summary to a file named `summary.md`.

```
piperider cloud compare-reports --base 123 --target 456 --summary-file summary.md 
```

Compare the latest reports from the `jaffle-shop` and `jaffle-shop-dev` data sources  and save the Markdown comparison summary to a file named `summary.md`.

```
piperider cloud compare-reports --base datasource:jaffle-shop --target datasource:jaffle-shop-dev --summary-file summary.md   
```

### Diagnose project configuration

```shell
piperider diagnose
```

Show data source connection status and check the current configuration and data assertions format.

| Option    | Argument | Description               |
| --------- | -------- | ------------------------- |
| `--debug` | N/A      | Enable debugging output   |
| `--help`  | N/A      | List command-line options |
|           |          |                           |

## Submit feedback

```
piperider feedback
```

We love to hear feedback from the community. Help us improve PipeRider by sending your suggestions.
