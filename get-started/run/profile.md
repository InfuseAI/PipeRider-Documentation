---
description: Profile statistics
---

# Profiling

PipeRider helps you understand of your data by providing profile statistics and data distribution information about the table and columns in your data source. When paired with data assertions, the data profile provides a way to check the quality and reliability of your data.

{% hint style="warning" %}
Assertion is deprecated since v0.25.0
{% endhint %}

## Enable Profiling

Profiling is supported for dbt models, seeds, and sources. To enable the profiling, please add `piperider` tag on the corresponding resources.

{% tabs %}
{% tab title="Config Block" %}
```diff
--- models/staging/stg_customers.sql
+{{ config(
+    tags=["piperider"]
+)}}

select ...

```
{% endtab %}

{% tab title="Project file" %}
```diff
# dbt_project.yml
models:
  jaffle_shop:
+     +tags: [piperider]
      materialized: table
      staging:
        materialized: view
seeds:
  jaffle_shop:
+   +tags: [piperider]
```
{% endtab %}

{% tab title="Config property" %}
```diff
# models/resources.yml
version: 2

models:
  - name: customers
    description: This table has basic information about a customer, as well as some derived facts based on a customer's orders
+   config:
+      tags: [piperider]
```
{% endtab %}
{% endtabs %}

and run the command to check if it is configured correectly.

```
 dbt list -s tag:piperider
```



## Table statistics

| Profile Field           | Description                                                                     |
| ----------------------- | ------------------------------------------------------------------------------- |
| `row_count`             | The number of rows in the table                                                 |
| `col_count`             | The number of columns in the table                                              |
| `samples`               | The number of rows profiled                                                     |
| `samples_p`             | The percentage of rows profiled                                                 |
| `bytes` \*              | The volume size of the table in bytes                                           |
| `created` \*            | The time that the table was created, including time zone, in ISO 8601 format    |
| `last_altered` \*       | The last time the table was modified, including time zone, in ISO 8601 format   |
| `freshness` \*          | The time differentiation between the current time and table's last altered time |
| `duplicate_rows` \*\*   | The number of duplicate rows in the table                                       |
| `duplicate_rows_p` \*\* | The percentage of duplicate rows in the table                                   |

{% hint style="info" %}
\* These statistics are only available for certain data sources. Please refer to the **platform dependent statistics** table below for availability information.\
\
\*\* Table-level duplicate row are not enabled by default. To enable this settings please refer to the [profiler settings](../../reference/project-structure/config.md).
{% endhint %}

| Profile Field  | Snowflake | BigQuery | Redshift |
| -------------- | --------- | -------- | -------- |
| `bytes`        | ✔         | ✔        | ✔        |
| `created`      | ✔         | ✔        |          |
| `last_altered` | ✔         | ✔        |          |
| `freshness`    | ✔         | ✔        |          |

## Column statistics

Column statistics are profiling statistics of a column. Some statistics are only avaialble on certain generic type. There are six generic types

* string
* integer
* numeric
* datetime
* boolean
* other

### Schema

In addition to logging the **schema type** of a column as defined in the data source, PipeRider will also apply a **generic type** to a column that will determine how this column is treated by the PipeRider profiler.

| Profile Field | Description                                                                                | Column Type | PipeRider Version |
| ------------- | ------------------------------------------------------------------------------------------ | ----------- | ----------------- |
| `schema_type` | The column type defined in the data source                                                 | All         | All               |
| `type`        | A generic schema type of `string`, `integer`, `numeric`, `datetime`, `boolean`, or `other` | All         | All               |

The following statistics are produced based on the generic type that has been applied to the column.

### Data composition

The composition of the data contained within a column.

<figure><img src="../../.gitbook/assets/metrics-composition.png" alt=""><figcaption><p>The generic type of a column determines the available statistics</p></figcaption></figure>

| Profile Field       | Description                                                                                                          | Column Type      | Assertion Available |
| ------------------- | -------------------------------------------------------------------------------------------------------------------- | ---------------- | ------------------- |
| `total`             | The number of rows in the table                                                                                      | All              | ✔                   |
| `samples`           | The number of rows profiled                                                                                          | All              | ✔                   |
| `samples_p`         | The percentage of rows profiled                                                                                      | All              | ✔                   |
| `nulls`             | The number of null values                                                                                            | All              | ✔                   |
| `nulls_p`           | The percentage of null values                                                                                        | All              | ✔                   |
| `non_nulls`         | The number of non-null values                                                                                        | All              | ✔                   |
| `non_nulls_p`       | The percentage of non-null values                                                                                    | All              | ✔                   |
| `invalids`          | The number of values that do not match the column's schema type. E.g. A string in a numeric column (**SQLite only**) | All              | ✔                   |
| `invalids_p`        | The percentage of invalid values (**SQLite only**)                                                                   | All              | ✔                   |
| `valids`            | The count of non-null values minus invalid values                                                                    | All              | ✔                   |
| `valids_p`          | The percentage of non-null values, minus invalid values                                                              | All              | ✔                   |
| `zeros`             | The number of zeros                                                                                                  | integer, numeric | ✔                   |
| `zeros_p`           | The percentage of zeros                                                                                              | integer, numeric | ✔                   |
| `negatives`         | The number of negative values                                                                                        | integer, numeric | ✔                   |
| `negatives_p`       | The percentage of negative values                                                                                    | integer, numeric | ✔                   |
| `positives`         | The number of positive values                                                                                        | integer, numeric | ✔                   |
| `positives_p`       | The percentage of positive values                                                                                    | integer, numeric | ✔                   |
| `zero_length`       | The number of empty strings                                                                                          | string           | ✔                   |
| `zero_length_p`     | The percentage of empty strings                                                                                      | string           | ✔                   |
| `non_zero_length`   | The number of non-empty strings                                                                                      | string           | ✔                   |
| `non_zero_length_p` | The percentage of non-empty strings                                                                                  | string           | ✔                   |
| `trues`             | The number of true values                                                                                            | boolean          | ✔                   |
| `trues_p`           | The percentage of true values                                                                                        | boolean          | ✔                   |
| `falses`            | The number of false values                                                                                           | boolean          | ✔                   |
| `falses_p`          | The percentage of false values                                                                                       | boolean          | ✔                   |

### General statistics

The general statistical information of a column.

| Profile Field | Description                      | Column Type                | Assertion Available | PipeRider Version |
| ------------- | -------------------------------- | -------------------------- | ------------------- | ----------------- |
| `min`         | The minimum value                | integer, numeric, datetime | ✔                   | All               |
| `max`         | The maximum value                | integer, numeric, datetime | ✔                   | All               |
| `avg`         | The column average               | integer, numeric           | ✔                   | All               |
| `sum`         | The column sum                   | integer, numeric           | ✔                   | All               |
| `stddev`      | The standard deviation of values | integer, numeric,          | ✔                   | 0.4.0             |

### Text length statistics

The text length statistics of a column.

| Profile Field   | Description                             | Column Type | Assertion Available |
| --------------- | --------------------------------------- | ----------- | ------------------- |
| `min_length`    | The minimum string length               | string      | ✔                   |
| `max_length`    | The maximum string length               | string      | ✔                   |
| `avg_length`    | The average string length               | string      | ✔                   |
| `stddev_length` | The standard deviation of string length | string      | ✔                   |

### Uniqueness

The uniqueness of a column.

<figure><img src="../../.gitbook/assets/metrics-uniqueness.png" alt=""><figcaption><p>Column uniqueness</p></figcaption></figure>

| Profile Field      | Description                           | Column Type                        | Assertion Available |
| ------------------ | ------------------------------------- | ---------------------------------- | ------------------- |
| `distinct`         | The number of distinct items          | integer, string, datetime          | ✔                   |
| `distinct_p`       | The percentage of distinct items      | integer, string, datetime          | ✔                   |
| `duplicates`       | The number of recurring items         | integer, numeric, string, datetime | ✔                   |
| `duplicates_p`     | The percentage of duplicate items     | integer, numeric, string, datetime | ✔                   |
| `non_duplicates`   | The number of non-recurring items     | integer, numeric, string, datetime | ✔                   |
| `non_duplicates_p` | The percentage of non-duplicate items | integer, numeric, string, datetime | ✔                   |

For example, the following dataset `(NULL, a, a, b, b, c, d, e)` would be categorized as so:

* Distinct count = 5, `(a, b, c, d, e)`
* Duplicate count = 4, `(a, a, b, b)`
* Non-duplicate count = 3, `(c, d, e)`
* Missing values (nulls) = 1

Therefore, the total number of rows for a table = missing (nulls) + duplicates + non-duplicates.

### Quantiles

The calculated quantiles of a numeric or integer column.

| Profile Field | Description      | Column Type      | Assertion Available | PipeRider Version |
| ------------- | ---------------- | ---------------- | ------------------- | ----------------- |
| `min`         | 0th percentile   | integer, numeric | ✔                   | All               |
| `p5`          | 5th percentile   | integer, numeric | ✔                   | 0.4.0             |
| `p25`         | 25th percentile  | integer, numeric | ✔                   | 0.4.0             |
| `p50`         | 50th percentile  | integer, numeric | ✔                   | 0.4.0             |
| `p75`         | 75th percentile  | integer, numeric | ✔                   | 0.4.0             |
| `p95`         | 95th percentile  | integer, numeric | ✔                   | 0.4.0             |
| `max`         | 100th percentile | integer, numeric | ✔                   | All               |

### Distribution

| Profile Field      | Description                                                                   | Column Type      |
| ------------------ | ----------------------------------------------------------------------------- | ---------------- |
| `topk`             | The most frequently occurring n items and and counts                          | integer, string  |
| `histogram`        | Evenly-split bins for **numerical columns** and counts for each bin           | integer, numeric |
| `histogram_length` | Evenly-split bins for text length and counts for each bin                     | string           |
| `histogram`        | Histogram of **date, month, or year**. Bin split depends on the min/max range | datetime         |
