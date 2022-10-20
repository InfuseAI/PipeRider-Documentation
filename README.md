---
description: PipeRider - Data Reliability Automated.
---

# Home

## What is PipeRider?

**PipeRider** is an open-source data quality toolkit for data professionals.

By coupling data profiling with data assertions, PipeRider provides a platform for both better understanding your data, and also defining what you expect your data to be.

PipeRider has the following main features:

* [Generate an HTML Report](how-to-guides/generate-report.md) featuring your data profile and data assertion test results ([interactive sample](https://piperider-github-readme.s3.ap-northeast-1.amazonaws.com/run-0.11.0/index.html))
* [Compare two reports](how-to-guides/compare-reports.md) to understand how your data has changed over time ([interactive sample](https://piperider-github-readme.s3.ap-northeast-1.amazonaws.com/comparison-0.11.0/index.html))
* Test your data with data assertions:
  * Built-in [data assertions](data-quality-assertions/assertion-configuration.md)
  * Extensible through [custom assertions](data-quality-assertions/custom-assertions.md)
  * Auto-generated data assertions
* Currently supports [Postgres](data-sources/supported-data-sources/postgres-connector.md), [Snowflake](data-sources/supported-data-sources/snowflake-connector.md), SQLite, [BigQuery](data-sources/supported-data-sources/bigquery-connector.md), [Redshift](data-sources/supported-data-sources/redshift-connector.md), [DuckDB](data-sources/supported-data-sources/duckdb-connector.md), [CSV](data-sources/supported-data-sources/csv-connector.md), and [Parquet](data-sources/supported-data-sources/parquet-connector.md).
* Zero-config [support for dbt](dbt-integration/) projects
* Automation through [GitHub Actions](how-to-guides/github-action.md), [save reports in S3](how-to-guides/aws-s3-+-github-ci.md)

Read more about the [concepts](concepts.md) behind PipeRider:

{% content-ref url="concepts.md" %}
[concepts.md](concepts.md)
{% endcontent-ref %}

{% embed url="https://www.youtube.com/watch?v=48l8Tg0aCTE" %}
PipeRider: Data Reliability, Automated.
{% endembed %}

## Getting Started

Use our [Quick Start](quick-start.md) tutorial and learn how to connect your first data source and get started checking the quality of your data.

{% content-ref url="install-piperider.md" %}
[install-piperider.md](install-piperider.md)
{% endcontent-ref %}

{% content-ref url="quick-start.md" %}
[quick-start.md](quick-start.md)
{% endcontent-ref %}
