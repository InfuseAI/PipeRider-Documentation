---
description: Zero-config Data Impact Assessment for dbt Projects
---

# Introduction

## What is `PipeRider`?

[PipeRider](https://github.com/infuseai/piperider) is a **data impact assessment tool for dbt data projects**.&#x20;

PipeRider compares the data in your dbt project from before and after making data modeling changes and generates **Impact Reports and Summaries**.  Use the generated reports to verify your changes and enable you to merge into prod confidently, without unexpected impact.

## What are `Impact Reports`?

Impact Reports provide details about the impact radius of data modeling changes and downstream impact.&#x20;

PipeRider surfaces impact in three main places.

### Impact Reports

An HTML report that contains a full breakdown of your data including:

* **Impact Summary** - An overview of impacted resources and the types of impact.
* **Data profile diff** - A detailed comparison of data profile statistics about your data.
* **Lineage Diff** - A visualization in the form of a directed acyclic graph (DAG) that shows the impact to the data pipeline after changes.
* **Metrics diff** - A graph-based comparison of how dbt metrics have been impacted.

### Pull Request Impact Summary

An overview of the data impact in a dbt project that is automatically added to the comments section of a pull request, and includes:

* **Impact Overview** - The number and types of impact that have occurred due to code changes in the pull request.
* **Resource Impact** - A list of models and an assessment of impact.
* **Metrics Impact** - Details of metrics impacted by the code changes.

### CLI Impact Summary

A surface-level summary that shows the main areas of impact when running PipeRider on the command line.

## Why use `PipeRider`?

* **See the scope of impact** that your dbt project code changes have on your data.
* **Improve your development process** by understanding your data and the impact of your changes.
* **Improve your code-review process** by verifying data impact before merging code changes.
* **Improve communication** between stakeholders and teammates by sharing and discussing Impact Reports.
* Keep a historical **record of data impact** reports for future reference.
* **Compare** the current state of your project **with any point in the past**.

## Getting started with `PipeRider`

PipeRider is a **zero-config installation for dbt projects**, no, _really_. In the majority of cases you can run PipeRider locally and generate an Impact Report in [just two commands](get-started/install-piperider.md).

1. Follow the [Quick Start guide](get-started/quick-start.md) to try out using PipeRider locally.
2. Sign up for [PipeRider Cloud](piperider-cloud/get-started.md) and upload and [share your first report](piperider-cloud/upload-data-reports.md).
3. Set up PipeRider in your CI process using the [PipeRider Compare GitHub Action](https://github.com/InfuseAI/piperider-compare-action).

## Try `PipeRider` without installing

It’s also possible to try our PipeRider without installing.

Head to [Quick Look](https://cloud.piperider.io/quick-look) on PipeRider Cloud and do one of the following:

* Paste a link to a pull request on your dbt project.
* Upload two dbt manifest.json files.

PipeRider Cloud will generate an Impact report with **schema change** and **Lineage Diff**.

{% hint style="info" %}
Note that Impact Reports generated from dbt manifest files and GitHub links are more lightweight than the full report and focus on schema change and Lineage Diff.
{% endhint %}

## Join the `PipeRider` community

* [Star PipeRider](https://github.com/InfuseAI/piperider) on GitHub.
* Join the [#tools-piperider](https://getdbt.slack.com/archives/C05C28V7CPP) channel on the [dbt Slack](https://www.getdbt.com/community/join-the-community).
* Join the [PipeRider Discord](https://piperider.io/discord) community
* Report a [bug or post a feature request](https://github.com/InfuseAI/piperider/issues/new/choose)

##
