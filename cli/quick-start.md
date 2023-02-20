---
description: Get PipeRider up and running quickly with this sample data quality project.
---

# Quick Start

### Before you start

* Python 3.7+

Ensure you have the following installed:

### 1. Install PipeRider

Navigate to your dbt folder, and install pipeirder.

```bash
pip install piperider
```

PipeRider supports the following data connectors

| connectors | install                              |
| ---------- | ------------------------------------ |
| snowflake  | `pip install 'piperider[snowflake]'` |
| postgres   | `pip install 'piperider[postgres]'`  |
| bigquery   | `pip install 'piperider[bigquery]'`  |
| redshift   | `pip install 'piperider[redshift]'`  |
| parquet    | `pip install 'piperider[parquet]'`   |
| csv        | `pip install 'piperider[csv]'`       |
| duckdb     | `pip install 'piperider[duckdb]'`    |

### 2. Initialize PipeRider

Go to your dbt project, and initalize PipeRider.

```bash
piperider init
```

PipeRider will automatically fetch the data source connections from `dbt_project.yml` file. These can be overwritten in the `.piperider/config.yml`

{% hint style="info" %}
The `init` command creates a `.piperider` directory inside the current directory. This is where all of the [project files](project-structure/) will be stored, including data source configuration, data quality assertions, data profiling information, and generated report files.
{% endhint %}

After initialization, you can verify the configuration by running `piperider diagnose`

### 3. Run PipeRider

Collect profiling statistics by using

```
piperider run
```

The `run` command will analyze the data source and generate profiling statics \[LINK TO PAGE] such as `row_count`, `non_nulls`, `min`, `max`, `distinct`, quantiles, `topk` and more.

The results are stored in `.piperider/outputs/<run-name>/run.json`

### 4. Run PipeRider in another branch

Go to another branch (e.g. the `main` branch) to compare your local changes. Then, run the following:

```
dbt build
piperider run --open
```

### 5. Compare your changes

You then can compare the branch of your new Pull Request against the main branch and explore the impact of your changes by opening the generated HTML comparison report

```bash
piperider compare-reports --last
```

`\\The --last option automatically selects the last two reports for comparison. Omit this option to manually select the reports you would like to compare.\\`

You can then view the downstream impact of your new model changes in the HTML report.

![](https://i.imgur.com/jXQVTpk.png)

### 6. Add a markdown summary

Aside from an HTML report, PipeRider creates a Markdown summary. You can add this summary of the data changes to your Pull Request so that your reviewer can merge with confidence.

Markdown summaries and reports are stored in `.piperider/comparisons/<timestamp>`

### More things you can do

* Make use of dbt features
  * metrics
  * custom schema
  * custom database
* Automate PipeRider in your CI/CD using GitHub Actions or PipeRider Cloud

### We'd love to hear from you

We're constantly striving to make PipeRider meet your needs. If you have feedback that could help make it better, we'd love to hear from you.

To send your feedback, use the following command to submit feedback via the CLI.

```
piperider feedback
```
