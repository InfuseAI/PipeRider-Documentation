# Run

Run is the single invocation of the piperider on the dbt project. It produces observed results of the dbt project such as profiling statistic, metric query results, and test results (assertions)

## Execute a run

To execute, use the `run` command&#x20;

```
piperider run
```

It will

* Connect to the data warehouse by the default target in your dbt profile
* Profile all table models in the dbt project and produce the schema information and profiling statistic
* Query the dbt metrics
* Assert profiling statistic to check if the value fulfill certain rule

### Select data source

Just as [dbt target](https://docs.getdbt.com/reference/dbt-jinja-functions/target), you can change the connection target for PipeRider to execute. In PipeRider, we call it **data source**. we can change the data source by `--datasource` option&#x20;

```
piperider run --datasource <datasource>
```

Data source is explicitly defined in the [config.yml](../../reference/project-structure/config.yml.md). If you run PipeRider on dbt project, the data sources is automatically derived from the [dbt profile](https://docs.getdbt.com/reference/profiles.yml) settings. You can use the command to check the current available data sources

```
 piperider config list-datasource
```

### Select models to profile

By default, PipeRider provide all the table models in your dbt project. However, profiling is an expansive and time consuming operation.  PipeRider provides several mechanism to control the models to profile.

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

**Use tag to mark selected models**

dbt allows to add [tag](https://docs.getdbt.com/reference/resource-configs/tags) on dbt resources. You can configure to add tag on the dbt config

```yaml
#.piperider/config.yml
dataSources: []
dbt:
  projectDir: .
  tag: 'piperider'
```

Once the `dbt.tag` is set, PipeRider profile only models with the specified tag

```
#models/staging/stg_payments.sql
{{ config(
    tags=["piperider"]
) }}

select ...

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

A use case for the dbt run results integration is leverage the [dbt state and deferral](https://docs.getdbt.com/docs/deploy/about-state)

```
# Run only the new and changed models
dbt run --select result:<status>+ state:modified+ --defer --state ./<dbt-artifact-path>

# Run only the excuted models by latest dbt run
piperider run --run-results
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

## Advanced: Query metrics

Metrics&#x20;

## Advanced: Define assertions

To be added





