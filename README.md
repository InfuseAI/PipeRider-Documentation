---
description: PipeRider - Data Reliability, Automated.
---

# Home

## What is PipeRider

**PipeRider** is a data quality check tool for data providers (people who generate data) and data consumers (developer, data scientist, data analyst) who want to trust their data by defining the shape of data, so they can ensure the data quality will meet the expectation in the future, save time on debugging data, easily discover & discuss any data problems from upstream, and socially collaborate across teams on the dataset through data catalog and data insights.

### Supported Data Sources

* Sqlite (by default)
* Postgres (Optional)
* Snowflake (Optional)

## Getting Started

Follow our handy guides to get started on the basics as quickly as possible:

{% content-ref url="install-piperider.md" %}
[install-piperider.md](install-piperider.md)
{% endcontent-ref %}

{% content-ref url="quick-start.md" %}
[quick-start.md](quick-start.md)
{% endcontent-ref %}

## The Concepts behind PipeRider

Learn the fundamentals of PipeRider to get a deeper understanding of our main features:

### dbt Integration

[dbt](https://www.getdbt.com/) is a great tool for the data transformation, and yes, Piperider supports dbt projects. Extra manual configuration is not required. Piperider will search and load the `dbt_project.yml` file into `./piperider/config.yml` by default when `piperider-cli init` .&#x20;
