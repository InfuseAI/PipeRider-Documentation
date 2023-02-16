---
description: Integrate PipeRider with your dbt project in 5 mins
---

# Quick Start

### Install PipeRider

Navigate to your dbt folder, and install PipeRider.

```bash
pip install 'piperider[<connector>]'
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

{% hint style="info" %}
PipeRider require python 3.7+
{% endhint %}

### Initialize a PipeRider project

Go to your dbt project, and initalize PipeRider.

```bash
piperider init
```

PipeRider will automatically find the [dbt project file](https://docs.getdbt.com/reference/dbt\_project.yml) `dbt_project.yml` and initiate PipeRider.

{% hint style="info" %}
The `init` command creates a `.piperider` directory inside the current directory. This is where all of the [piperider project files](../reference/project-structure/) will be stored, including data source configuration, data quality assertions, data profiling information, and generated report files.
{% endhint %}

After initialization, you can verify the configuration by running `piperider diagnose`. It will use the [dbt profile file](https://docs.getdbt.com/reference/profiles.yml) `profiles.yml` to connect to the data warehouse.

### Run PipeRider

Collect profiling statistics by using

```
piperider run
```

The `run` command will generate profiling statistics for your table models, such as `row_count`, `non_nulls`, `min`, `max`, `distinct`, quantiles, `topk` and more.

### Compare two branches

PipeRider is designed for code review. You can initiate the comparison in your local environment.

1.  **Run in the base branch.** Usually, it's the main branch\


    ```
    git switch main
    dbt build
    piperider run
    ```
2.  **Run in the target branch.** Usually, it's the PR branch for code review.\


    ```
    git switch features/my-awesome-feature
    dbt build
    piperider run
    ```
3.  **Generate the comparison report**. You then can compare the branch of your new Pull Request against the main branch and explore the impact of your changes by opening the generated HTML comparison report\


    ```bash
    piperider compare-reports --last
    ```

    \
    The `--last` option automatically selects the last two data profiles for comparison. Omit this option to manually select the profiles you would like to compare.
4. **Post the markdown summary to the PR comment.** Aside from an HTML report, PipeRider generate a Markdown summary. You can add this summary of the data changes to your Pull Request comment so that your reviewer can review with impact information and merge with confidence :tada:

### What's next

* Integrate [dbt metrics](run/metrics.md)
* Specify which models to profile
* Try [PipeRider Cloud](piperider-cloud/get-started.md)
