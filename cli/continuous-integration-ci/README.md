---
description: Using PipeRider with your CI process
---

# Continuous Integration (CI)

## What is Continuous Integration (CI)

Continuous integration (CI) is the practice of automating routine tasks on a software project. For instance, after pushing code changes to your project repository, CI can trigger building, testing, and linting functions to ensure the quality of your commit. This eases the process of contributing to a project by removing the need to run these stages manually.

### CI for a data project

Similar to a software project, a CI process is also useful for data projects, especially now that data projects contain code responsible for building the data pipeline and model transformations.

When used in a data project, a CI process can perform the model transformations, make modifications to databases after merging a pull request, and deploy the pipeline, among other actions.

### The benefit of using PipeRider with a CI process

When integrated with a CI process, **PipeRider can add a data profiling step after new models are transformed**. This data profile can then be compared with the data profile of the production environment, conveniently showing how the data has changed between environments.

PipeRider can:

* Provide full **data profiling reports** in the form of artifacts
* Generate a data profile comparison summary that **compares the production and modified environments**

The comparison summary is generated in Markdown and is **designed to be added to the pull request comment for easy review**. The summary contains useful metrics about the table schema and data, such as:

* Row count
* Column count
* Column type
* New or deleted columns
* Percentage of valid rows
* percentage of distinct rows

The following image show an example of a PipeRider comparison summary taken from our article on [detecting schema change](https://blog.infuseai.io/how-to-detect-schema-change-in-snowflake-6ffcd28c3f15):

<figure><img src="../../.gitbook/assets/data-profile-diff-fs8.png" alt=""><figcaption><p>Example of PipeRider data profile comparison added to pull request comment</p></figcaption></figure>



In the following section we demonstrate some suggested guidelines for integrating PipeRider into your data project's CI process.&#x20;

