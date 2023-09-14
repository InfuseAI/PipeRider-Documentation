---
description: Zero-config Data Impact Assessment  for dbt Projects
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

PipeRider does not require any configuration to use inside a dbt project.

### Create a Data Impact Report (Comparison)

If you are working on feature branch of your dbt project, use the following command to compare your current code changes to merge base:&#x20;

```
$ piperider compare
```

The `compare` command compares the before-and-after impact of your current code changes and generates a Data Impact Report containing an impact summary and side-by-side data profiling results.&#x20;

{% hint style="info" %}
By default, `piperider compare` assesses modified resources and children (`state:modified+).`Read more about [specifying resources to profile](specify-resources-to-profile.md).&#x20;
{% endhint %}

###

