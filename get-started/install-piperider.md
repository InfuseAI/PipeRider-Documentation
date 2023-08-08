---
description: How to install PipeRider with basic usage examples
---

# Install PipeRider

### Installing PipeRider in your dbt project

Install PipeRider with the relevant data source connector using the following command.

```
pip install -U 'piperider[<connector>]'
```

The following connectors are supported:

{% tabs %}
{% tab title="Athena" %}
```bash
pip install -U 'piperider[athena]'
```
{% endtab %}

{% tab title="BigQuery" %}
```bash
pip install -U 'piperider[bigquery]'
```
{% endtab %}

{% tab title="Databricks" %}
```bash
pip install -U 'piperider[databricks]'
```
{% endtab %}

{% tab title="DuckDB" %}
```bash
pip install -U 'piperider[duckdb]'
```
{% endtab %}

{% tab title="Postgres" %}
```bash
pip install -U 'piperider[postgres]'
```
{% endtab %}

{% tab title="Redshift" %}
```bash
pip install -U 'piperider[redshift]'
```
{% endtab %}

{% tab title="Snowflake" %}
```bash
pip install -U 'piperider[snowflake]'
```
{% endtab %}
{% endtabs %}



## Zero-config dbt usage

PipeRider does not require any further configuration to use inside a dbt project.

### Create a Data Impact Report (Comparison)

If you are working on feature branch of your dbt project and want to compare your current code changes to merge base, run `piperider compare` passing `state:modified+:`

```
piperider compare --select state:modified+
```

The `compare` command compares the before-and-after impact of your current code changes and generates a Data Impact Report containing an impact summary and side-by-side data profiling results.&#x20;

{% hint style="info" %}
Running `piperider compare` _without_ passing a resource selector will **only detect schema changes**, _unless_ you have already tagged resources. Find out more about [specifying resources to profile](specify-resources-to-profile.md).&#x20;
{% endhint %}

###

