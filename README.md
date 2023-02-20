---
description: PipeRider - Data Reliability Automated.
---

# Introduction

## Code review for data in dbt

**PipeRider** is an open-source data quality toolkit for data professionals.

PipeRider automatically compares your data to highlight the difference in impacted downstream dbt models so you can merge your Pull Requests with confidence.

## How it works:

1. Easy to connect your data source -> PipeRider leverages the [connection profiles in your dbt project](https://docs.getdbt.com/docs/get-started/connection-profiles) to connect to the data warehouse
2. Generate profiling statistics of your models to get a high-level overview of your data
3. Compare local changes with the main branch in an HTML report
4. Post a quick summary of the data changes to your PR, so others can be confident too
5. Integrate PipeRider in your CI/CD process through GitHub actions or using PipeRider Cloud

## Core concepts

* **Easy to install**: Leveraging dbt's configuration settings, PipeRider can be installed within 2 minutes
* **Fast comparison**: by collecting profiling statistics (e.g. uniqueness, averages, quantiles, histogram) and metric queries, comparing downstream data impact takes little time, speeding up your team's review time
* **Valuable insights**: various profiling statistics displayed in the HTML report give fast insights into your data

## Getting Started

Use our [Quick Start](get-started/quick-start.md) tutorial and learn how to connect your first data source and get started checking the quality of your data.

{% content-ref url="get-started/quick-start.md" %}
[quick-start.md](get-started/quick-start.md)
{% endcontent-ref %}
