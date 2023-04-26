---
description: Integrate PipeRider with your dbt project in 5 mins
---

# Quick Start

## Install PipeRider

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
| athena     | `pip install 'piperider[athena]'`    |
| parquet    | `pip install 'piperider[parquet]'`   |
| csv        | `pip install 'piperider[csv]'`       |
| duckdb     | `pip install 'piperider[duckdb]'`    |

{% hint style="info" %}
PipeRider requires python 3.7+
{% endhint %}

## Initialize a project

Go to your dbt project, and initialize PipeRider.

```bash
piperider init
```

PipeRider will automatically find the [dbt project file](https://docs.getdbt.com/reference/dbt\_project.yml) `dbt_project.yml` and initiate PipeRider.

{% hint style="info" %}
The `init` command creates a `.piperider` directory inside the current directory. This is where all of the [piperider project files](../reference/project-structure/) will be stored, including project configuration file, and generated report files.
{% endhint %}

After initialization, you can verify the configuration by running `piperider diagnose`. It will use the [dbt profile file](https://docs.getdbt.com/reference/profiles.yml) `profiles.yml` to connect to the data warehouse.

## Run PipeRider

To profile models in your project, you need to specify which models to run on. This can be done in the following ways.

### Specify single model via CLI parameter

The fastest way to run PipeRider is to generate a report for a single model using the **`--table <model-name>`** option. Here is an example:

```bash
piperider run --table stg_customers
```

PipeRider will perform profiling on the **`stg_customers`** model and generate a html report. You can find the report in the last line of the output.

### Configure multiple models via dbt tags

Of course, we don't want to specify the table to run every time. In this case, we can manually specify which models should be profiled by PipeRider using [dbt tags](https://docs.getdbt.com/reference/resource-configs/tags).

To enable profiling for a specific model, add the `piperider` tag to your model. This will instruct PipeRider to perform profiling on this model.

```yaml
-- models/staging/stg_customers.sql

{{config(tags='piperider')}}

select ...
```

Check if the tag is well-configured. The following command will list the models that you have just modified.

```
dbt list -s tag:piperider --resource-type model
```

Now, it will profile your model by default

```
piperider run
```

## Compare two branches

PipeRider is designed for code review. You can initiate the comparison in your local environment.

1.  **Run in the base branch.** Usually, it's the main branch.

    ```
    git switch main
    dbt build
    piperider run
    ```
2.  **Run in the target branch.** Usually, it's the PR branch for code review.

    ```
    git switch features/my-awesome-feature
    dbt build
    piperider run
    ```
3.  **Generate the comparison report**. You then can compare the branch of your new Pull Request against the main branch and explore the impact of your changes by opening the generated HTML comparison report\\

    ```bash
    piperider compare-reports --last
    ```

    \
    The `--last` option automatically selects the last two data profiles for comparison. Omit this option to manually select the profiles you would like to compare.
4. **Post the markdown summary to the PR comment.** Aside from an HTML report, PipeRider generate a Markdown summary. You can add this summary of the data changes to your Pull Request comment so that your reviewer can review with impact information and merge with confidence :tada:

## What's next

* Integrate [dbt metrics](run/metrics.md)
* Specify which models to profile
* Try [PipeRider Cloud](../piperider-cloud/get-started.md)
