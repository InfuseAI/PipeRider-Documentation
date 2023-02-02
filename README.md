---
description: PipeRider - Data Reliability Automated.
---

# Introduction

## What is PipeRider?

**PipeRider** is an open-source data quality toolkit for data professionals.

By coupling data profiling with data assertions, PipeRider provides a platform for both better understanding your data, and also defining what you expect your data to be.

PipeRider has the following main features:

* [Generate an HTML Report](how-to-guides/generate-report.md) featuring your data profile and data assertion test results ([interactive sample](https://piperider-github-readme.s3.ap-northeast-1.amazonaws.com/run-0.18.0/index.html))
* [Compare two reports](how-to-guides/compare-reports.md) to understand how your data has changed over time ([interactive sample](https://piperider-github-readme.s3.ap-northeast-1.amazonaws.com/comparison-0.18.0/index.html))
* Test your data with data assertions:
  * Built-in [data assertions](cli/data-quality-assertions/assertion-configuration.md)
  * Extensible through [custom assertions](cli/data-quality-assertions/custom-assertions.md)
  * Auto-generated data assertions
* Currently supports [Postgres](cli/supported-data-sources/postgres-connector.md), [Snowflake](cli/supported-data-sources/snowflake-connector.md), SQLite, [BigQuery](cli/supported-data-sources/bigquery-connector.md), [Redshift](cli/supported-data-sources/redshift-connector.md), [DuckDB](cli/supported-data-sources/duckdb-connector.md), [CSV](cli/supported-data-sources/csv-connector.md), and [Parquet](cli/supported-data-sources/parquet-connector.md).
* Zero-config [support for dbt](cli/dbt-integration.md) projects
* Automation through [GitHub Actions](broken-reference), [save reports in S3](broken-reference)

Read more about the [concepts](concepts.md) behind PipeRider:

{% content-ref url="concepts.md" %}
[concepts.md](concepts.md)
{% endcontent-ref %}

{% embed url="https://www.youtube.com/watch?v=48l8Tg0aCTE" %}
PipeRider: Data Reliability, Automated.
{% endembed %}

## Getting Started

Use our [Quick Start](cli/quick-start.md) tutorial and learn how to connect your first data source and get started checking the quality of your data.

{% content-ref url="cli/install-piperider.md" %}
[install-piperider.md](cli/install-piperider.md)
{% endcontent-ref %}

{% content-ref url="cli/quick-start.md" %}
[quick-start.md](cli/quick-start.md)
{% endcontent-ref %}
