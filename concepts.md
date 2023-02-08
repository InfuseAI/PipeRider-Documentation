---
description: The concepts behind PipeRider
---

# Concepts

Ensuring the consistent quality of data used to be difficult. Missing values, schema changes, data drift (to name just a few), could be introduced to your data at any time. Without effective data quality tools these errors will affect downstream operations and result in countless lost hours to debugging, and missed revenue opportunities from unexpected downtime.

The concept of PipeRider is to enable you to better **understand your data** through [data profiling](data-profile-and-metrics/data-profile.md), and **ensure the quality of your data** by testing the [data profile metrics](data-profile-and-metrics/metrics.md) with [data assertions](cli/data-quality-assertions/assertion-configuration.md). It is through this process of profiling and testing that PipeRider provides a platform for data reliability.

PipeRider should work with your **existing data sources**, and require **minimal configuration** to set-up and use. This is demonstrated through PipeRider's automatic detection of dbt projects and recommended assertion generation.

The **command line tool** should be **useful for data engineers**, while the **reporting output** should be visually appealing and **useful to non-DE stakeholders**.

Read more about the back-story to why we created PipeRider:

{% embed url="https://blog.infuseai.io/data-reliability-automated-with-piperider-7a823521ef11" %}
