# Run

"Run" is a single execution of PipeRider on a dbt project. It generates observed results of the dbt project, such as profiling statistics, metric query results, and test results (assertions).

## Execute a run

To execute, use the `run` command&#x20;

```
piperider run
```

It will

* Connect to the data warehouse using the default target specified in your dbt profile.
* Profile all models (with the `piperider` tag) and generate schema information and profiling statistics.
* Query the dbt metrics (with the `piperider` tag) using the default query logic.
* Assert the profiling statistics to check if their values satisfy certain rules.

### Select data source

Just as [dbt target](https://docs.getdbt.com/reference/dbt-jinja-functions/target), you can change the connection target for PipeRider to execute. In PipeRider, we call it **data source**. we can change the data source by `--datasource` option&#x20;

```
piperider run --datasource <datasource>
```

Data source is explicitly defined in the [config.yml](../../reference/project-structure/config.md). If you run PipeRider on dbt project, the data sources is automatically derived from the [dbt profile](https://docs.getdbt.com/reference/profiles.yml) settings. You can use the command to check the current available data sources

```
 piperider config list-datasource
```

### Select models to profile

**Use 'piperider' tag to mark selected models**

By default, PipeRider profiles all models with the `piperider` tag  in your dbt project. Here is an example of how to add the `piperider` tag to a model

```
--models/staging/stg_customers.sql
{{ config(
    tags=["piperider"]
) }}

select ...

```

You can also add it in [profile file or config yaml](https://docs.getdbt.com/reference/resource-configs/tags). Then, check if the model is well-configured. The following command would list the model you just modified.

```
 dbt list -s tag:piperider --resource-type model       
```

Afterwards, when running `piperider run`, all models with the `piperider` tag will be profiled by default.

**Profile one model**

The simplest way is use the `--table <model>` option

```
piperider run --table <you-model-name>
```

**Use dbt list output to select models**

`dbt list` is a dbt subcommand which allows to select dbt resources by [node selection](https://docs.getdbt.com/reference/node-selection/syntax).

```
dbt list --select <selector> | piperider run --dbt-list
```

### Integrate with run results

Dbt expose the information of the latest run by the [run results](https://docs.getdbt.com/reference/artifacts/run-results-json). Use the option `--run-results`&#x20;

```
dbt build
piperider run --dbt-run-results
```

PipeRider can also integrate this information, the run will

* Run only the executed models
* Integrate the test result of dbt

If the `dbt.tag` is configured in the PipeRider config. PipeRider only run the tagged models which is executed in the latest dbt run.

A use case for the dbt run results integration is to leverage the [dbt state and deferral](https://docs.getdbt.com/docs/deploy/about-state). Dbt can run only the new and changed model then PipeRider profile only on these changed models.

```bash
# Run only the new and changed models
dbt run \
   --select result:<status>+ state:modified+ \
   --defer \
   --state ./<dbt-artifact-path>

# Run only the excuted models by latest dbt run
piperider run --dbt-run-results
```

## Run artifacts

After the execution of a run, two artifacts are generated under the output directory

* JSON run result (`run.json`)
* HTML report (`index.html`)

The default output directory is located at `.piperider/outputs/<datasource>-<datetime>/` . For ease of use, the latest run would also be sym-linked by `.piperider/outputs/latest`

You can use the `--output` to change the output directory

```
piperider run --output /tmp/myrun
```

## Advanced: Query the metrics

PipeRider is more than a profiler. In a dbt project, especially for analytics purpose project, it's common to have several metrics defined for visualization. (e.g. revenue, active users). PipeRider can query the metrics are visualize it in the run report. For more detail, please see [Metrics](metrics.md).







