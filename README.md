---
description: >-
  PipeRider CLI automates data quality management to provide reliable data
  products for your users.
---

# Home

## What PipeRider CLI is

**PipeRider CLI** is a data quality check tool for data providers (people who generate data) and data consumers (developer, data scientist, data analyst) who want to trust their data by defining the shape of data, so they can ensure the data quality will meet the expectation in the future, save time on debugging data, easily discover & discuss any data problems from upstream, and socially collaborate across teams on the dataset through data catalog and data insights.

### Supported Data Sources

* Sqlite (by default)
* Postgres (Optioinal)
* Snowflake (Optional)

## What PipeRider CLI isn't

## Getting Started

**Got 2 minutes?** Check out a video overview of our product:

### Guides: Get stuck in

Follow our handy guides to get started on the basics as quickly as possible:

{% content-ref url="install-piperider.md" %}
[install-piperider.md](install-piperider.md)
{% endcontent-ref %}

{% content-ref url="quick-start/" %}
[quick-start](quick-start/)
{% endcontent-ref %}

{% hint style="info" %}
**Good to know:** your product docs aren't just a reference of all your features! use them to encourage folks to perform certain actions and discover the value in your product.
{% endhint %}

## The Concepts behind PipeRider

Learn the fundamentals of PipeRider to get a deeper understanding of our main features:

{% hint style="info" %}
**Good to know:** Splitting your product into fundamental concepts, objects, or areas can be a great way to let readers deep dive into the concepts that matter most to them. Combine guides with this approach to 'fundamentals' and you're well on your way to great documentation!
{% endhint %}

### dbt Integration

[dbt](https://www.getdbt.com/) is a great tool for the data transformation, and yes, Piperider CLI supports dbt projects. Extra manual configuration is not required. Piperider CLI will search and load the `dbt_project.yml` file into `./piperider/config.yml` by default when `piperider-cli init` .&#x20;
