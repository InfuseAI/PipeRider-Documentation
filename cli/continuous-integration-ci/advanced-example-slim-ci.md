---
description: Use dbt state to only build and test the models that have changed
---

# Advanced Example (Slim CI)

Assume you have a data project with hundreds of models, but you only change a few of them in your pull request. It may cost more time and computation than its real requirement. &#x20;

In this Advanced CI workflow setup, we take the advantage of dbt state to prevent unnecessary transformations. Please refer to the[ dbt documentation](https://docs.getdbt.com/guides/legacy/best-practices#run-only-modified-models-to-test-changes-slim-ci) for more information on Slim CI.

Like the [Basic Example](basic-example.md), the Slim CI version has a Production and PR workflow, however, this time dbt 'state' is used to ensure only changed models are transformed and profiled.



<figure><img src="../../.gitbook/assets/piperider_slim-ci_process.png" alt=""><figcaption><p>PipeRider Slim CI Workflows</p></figcaption></figure>

## Production Job

It’s the daily job to transform source data into transformed models. And PipeRider takes dbt state into consideration to decide the profiling subject.

If you didn’t use dbt cloud service, there’s a good article show how to integrate Google Cloud Storage to keep the dbt state. In the same, you can put PipeRider Report in your own storage.[https://www.vantage-ai.com/blog/how-to-use-slim-ci-with-dbt-core](https://www.vantage-ai.com/blog/how-to-use-slim-ci-with-dbt-core)

If you a dbt Cloud and PIpeRider Cloud user, follow Option 1 in the steps below to utilize storage from these services. Otherwise, follow Option 2 to use your own storage.

### 1. Run dbt on production environment and upload state

#### Option 1 - Use dbt Cloud

```bash
dbt-cloud job run --wait --file response.json
run_id=$(cat response.json | jq -r '.data.id')
```

#### Option 2 - Use your own script to upload to your storage

Run `dbt build` and store the state locally.

```bash
dbt build --target prod --target-path target/
upload-prod-dbt-state.sh target/
```

### Run PipeRider and upload report

#### Option 1 - use PipeRider Cloud

Get the artifacts from the dbt job.&#x20;

```
mkdir -p target
dbt-cloud run get-artifact --run-id $run_id --path manifest.json -f target/manifest.json
dbt-cloud run get-artifact --run-id $run_id --path run_results.json -f target/run_results.json
```

Log into PipeRider Cloud, run PipeRider against the downloaded dbt target, then upload the report to PipeRider Cloud.

```
piperider cloud login --token <PIPERIDER_CLOUD_TOKEN> --disable-auto-upload
piperider run \
  --datasource jaffle_shop \
  --dbt-state target \
  -o /tmp/piperider
piperider cloud upload-report --run /tmp/piperider/run.json
```

#### Option 2 - Use your own storage

Run PipeRider against the locally stored dbt state, and then upload the report to your own storage.

```bash
piperider run \
  --data-source jaffle_shop  \
  --dbt-state target/ \
  -o /tmp/piperider
upload-prod-report.sh /tmp/piperider
```

### View the Production Slim CI job on GitHub

{% embed url="https://github.com/InfuseAI/jaffle_shop/blob/main/.github/workflows/production-daily-job-slim-ci.yml" %}
Production Slim CI Job on Github
{% endembed %}



