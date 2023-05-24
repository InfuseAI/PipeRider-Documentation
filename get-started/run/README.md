# Run

"**Run**" is a single execution of PipeRider on a dbt project. It generates observed results of the dbt project, such as table schema, profiling statistics, metric query results. They are used as the basis for comparison.

The `run` command execute

* **Collect Metadata**: To collect the column names and types for all models, sources, and seeds.
* **Profile statistics**: To obtain statistics for a model/seed/source and its columns, including row counts, null values, sum, average, text length, and other information, in order to gain insights into the data distribution of the model. By default, this feature is disabled since it can be resource-intensive. To enable profiling, you need to manually activate it. Please see the [profiling](profile.md) document
* **Query metric:** A metric represents the computation of a specific column within a time interval, typically calculated on a daily or monthly basis. For example, daily revenue is a simple example of a metric. Metric queries allow you to perform basic queries on a DBT metric, such as retrieving the daily reports for the past 30 days or the monthly reports for the last 12 months. Similar to model profiling, you need to manually enable metric queries. Please see the [metric](metrics.md) document

## Execute a run

To execute, use the `run` command&#x20;

```
piperider run
```

After the execution of a run, two artifacts are generated under the output directory

* JSON run result (`run.json`)
* HTML report (`index.html`)

The default output directory is located at `.piperider/outputs/<datasource>-<datetime>/` . For ease of use, the latest run would also be sym-linked by `.piperider/outputs/latest`

You can use the `--output` to change the output directory

```
piperider run --output /tmp/myrun
```

## Run with profiling statistics

To profile a model, source, or seed, add the `piperider` [tag](https://docs.getdbt.com/reference/resource-configs/tags) to your resource. Then, check if the resource is configured correctly.&#x20;

{% code fullWidth="false" %}
```diff
--- models/staging/stg_customers.sql
+{{ config(
+    tags=["piperider"]
+)}}

select ...

```
{% endcode %}

The following command would list the model you just modified.

```
 dbt list -s tag:piperider
```

Afterwards, when running `piperider run`, all models with the `piperider` tag will be profiled by default.

For more detail, please see [Profiling](profile.md)

## Run with metric queries

In a dbt project, especially for analytics purpose project, it's common to have several metrics defined for visualization. (e.g. revenue, active users). PipeRider can query the metrics are visualize it in the run report.&#x20;

To enable a metric query, there are two steps

1. Define a [dbt metric](https://docs.getdbt.com/docs/build/metrics)
2. Add `piperider` tag on this metric

Here is an metric example

```diff
metrics:
  - name: active_users
    label: Active Users
    model: ref('stg_events')
    description: "The active user"

    calculation_method: count_distinct
    expression: user_id 

    timestamp: event_time
    time_grains: [day, week, month, year]
+   tags: ['piperider']
```

For more detail, please see [Metrics](metrics.md).

## Run with selection

By default, PipeRider profiles models, sources, and seeds with `piperider` tag, and query metrics with `piperider` tag. However, you can also use additional options to select the specific resources that should be processed.

**Use dbt list to select resources**

`dbt list` is a dbt subcommand which allows to select dbt resources by [node selection](https://docs.getdbt.com/reference/node-selection/syntax).

```
dbt list --select <selector> | piperider run --dbt-list
```

select a model by file path

```
 dbt list -s models/customers.sql| piperider run --dbt-list  
```

select resource with tag `piperider-dev`&#x20;

```
dbt list -s 'tag:piperider-dev' | piperider run --dbt-list 
```

**Select the resource to profile**

You can also use `--table` to profile specific resource.

```
piperider run --table <resource_name>
```

## Advanced: Select data source (The dbt target)

Just as [dbt target](https://docs.getdbt.com/reference/dbt-jinja-functions/target), you can change the connection target for PipeRider to execute. In PipeRider, we call it **data source**. we can change the data source by `--datasource` option&#x20;

```
piperider run --datasource <dbt-tareget>
```

Data source is explicitly defined in the [config.yml](../../reference/project-structure/config.md). If you run PipeRider on dbt project, the data sources is automatically derived from the [dbt profile](https://docs.getdbt.com/reference/profiles.yml) settings. You can use the command to check the current available data sources

```
 piperider config list-datasource
```





