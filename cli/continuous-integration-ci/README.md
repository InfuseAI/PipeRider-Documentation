---
description: Using PipeRider with your CI process
---

# Continuous Integration (CI)

## What is Continuous Integration (CI)

Continuous integration (CI) is a practice of automating routine tasks on a software project. You may have already been familiar with such procedures that after pushing code change into your code repository, build, testing, and linting will be triggered to ensure the quality of your commit. It eases the efforts that all contributors need to run the whole procedure manually.

### CI for a data project

In a data project, there is code that is responsible for data pipeline and model transformation.  Similar to a software project, a CI process can help deploy the new pipeline, transform the new models, and make these modifications to the corresponding database after developers merge their changes.

### The benefit of using PipeRider with a CI process

With PipeRider CI integration, you can add a data profiling step to the CI process after new models are transformed. **PipeRider can provide data profiling reports** in the form of artifacts, and further **generate a summary comparing the data to its original form**.

Using this method, **developers can observe what the affect of these changes will be before they are merged into the project.**

In the following section we demonstrate some suggested guidelines for integrating PipeRider into your data project's CI process.&#x20;

