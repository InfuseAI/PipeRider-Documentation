---
description: How to specify which resources PipeRider should profile
---

# Specify resources to profile

To use the data profiling and comparison features of PipeRider, you will need to specify which resources PipeRider will run on. Without doing this, PipeRider will only detect schema change.

This can be done in the following ways:

* Tag resources
* Use the `--specify` option
* use the `--table` option (deprecated, for non-dbt projects)

## Tag resources

By default PipeRider looks for then 'piperider' tag when discovering resources tom profile. Tags can be added to dbt resources in various ways, such as tagging resources individually using a config block:

{% code title="model.sql" %}
```
{{ config(
    tags=['piperider']
) }}
```
{% endcode %}

Tags can also be applied in your project file or as a config property. Refer to the [dbt documentation](https://docs.getdbt.com/reference/resource-configs/tags) for more information on tags configuration.

## Use the `--specify` option

The `--specify` option can be used to select individual resources or groups of resources, and can be used with both `piperider run` and `piperider compare`.&#x20;

### Specify a single resource  &#x20;

Run PipeRider on a single model.&#x20;

```
piperider run --select path/to/model
```

### Specify a group of resources

Run PipeRider on all models in the staging folder.

```
piperider compare --select 'models/staging'
```

## Use the `--table` option (deprecated)

The `--table` option is used for profiling individual tables in non-dbt projects. It now deprecated in favor of the `--select` option.

Select an individual table to profile.

```
piperider run --table <table>
```

## Examples



