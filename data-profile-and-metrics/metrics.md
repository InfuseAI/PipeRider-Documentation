---
description: >-
  PipeRider analyzes the data source and produces a data profile that contains
  the following metrics.
---

# Metrics

Data profile metrics are divided between table and column level, and column is further analyzed depending on the schema type and generic type (see [Schema](metrics.md#schema) below).

Built-in assertions are not availabe for every metric. When a built-in assertion becomes available for a metric, a check mark (✔) will be added the to `Assertion Available` column.

If a metric is not available, please ensure you are using the latest version of PipeRider. Check the `PipeRider Version` column to see when a metric was introduced.

## Table metrics

Data profile metrics that describe the tables in a data source.

| Metric                       | Description                                                                     | Profile Field      | Assertion Available | PipeRider Version |
| ---------------------------- | ------------------------------------------------------------------------------- | ------------------ | ------------------- | ----------------- |
| Row count                    | The number of rows in the table                                                 | `row_count`        | ✔                   | All               |
| Column count                 | The number of columns in the table                                              | `col_count`        |                     | All               |
| Sample count                 | The number of rows profiled                                                     | `samples`          |                     | 0.10.0            |
| Sample percentage            | The percentage of rows profiled                                                 | `samples_p`        |                     | 0.11.0            |
| Volume size_\*_              | The volume size of the table in bytes                                           | `bytes`            | ✔                   | 0.8.0             |
| Created time_\*_             | The time that the table was created, including time zone, in ISO 8601 format    | `created`          |                     | 0.8.0             |
| Last altered time_\*_        | The last time the table was modified, including time zone, in ISO 8601 format   | `last_altered`     |                     | 0.8.0             |
| Freshness_\*_                | The time differentiation between the current time and table's last altered time | `freshness`        | ✔                   | 0.8.0             |
| Duplicate row count\*\*      | The number of duplicate rows in the table                                       | `duplicate_rows`   | ✔                   | 0.10.0            |
| Duplicate row percentage\*\* | The percentage of duplicate rows in the table                                   | `duplicate_rows_p` | ✔                   | 0.11.0            |

{% hint style="info" %}
\* __ These metrics are only available for certain data sources.  Please refer to the **platform dependent metrics** table below for availability information.\
\
\*\* Table-level duplicate row metrics are now enabled by default. To enable this settings please refer to the [Profiler Settings](../project-structure/config.yml.md#profiler-settings).
{% endhint %}

### Platform dependent metrics&#x20;

| Metric            | Snowflake | BigQuery | Redshift | Others |
| ----------------- | --------- | -------- | -------- | ------ |
| Bytes             | ✔         | ✔        | ✔        |        |
| Created time      | ✔         | ✔        |          |        |
| Last altered time | ✔         | ✔        |          |        |
| Freshness         | ✔         | ✔        |          |        |

## Column metrics

Data profile metrics that describe data at the column level. Depending on the column type, different metrics will be produced.

### Schema

In addition to logging the **schema type** of a column as defined in the data soruce, PipeRider will also apply a **generic type** to a column that will determine how this column is treated by the PipeRider profiler.

| Metric       | Description                                                                                 | Profile Field | Column Type | PipeRider Version |
| ------------ | ------------------------------------------------------------------------------------------- | ------------- | ----------- | ----------------- |
| Schema Type  | The column type defined in the data source                                                  | `schema_type` | All         | All               |
| Generic Type | A generic schema type of  `string`, `integer`, `numeric`, `datetime`, `boolean`, or `other` | `type`        | All         | All               |

The following metrics are produced based on the generic type that has been applied to the column.

### Data composition

The composition of the data contained within a column.

<figure><img src="../.gitbook/assets/metrics-composition.png" alt=""><figcaption><p>The generic type of a column determines the available metrics</p></figcaption></figure>

| Metric                       | Description                                                                                                           | Column Type      | Assertion Available | Profile Field     | PipeRider Version |
| ---------------------------- | --------------------------------------------------------------------------------------------------------------------- | ---------------- | ------------------- | ----------------- | ----------------- |
| Row count                    | The number of rows in the table                                                                                       | All              | ✔                   | `total`           | All               |
| Sample count                 | The number of rows profiled                                                                                           | All              | ✔                   |                   |                   |
| Sample percentage            | The percentage of rows profiled                                                                                       | All              | ✔                   |                   |                   |
| Missing count                | The number of null values                                                                                             | All              | ✔                   | `nulls`           | 0.6.0             |
| Non-null count               | The number of non-null values                                                                                         | All              | ✔                   | `non_nulls`       | All               |
| Invalid count                | The number of values that do not match the column's schema type. E.g. A string in a numeric column. (**SQLite only**) | All              | ✔                   | `invalids`        | 0.6.0             |
| Valid count                  | The count of non-null values minus invalid values                                                                     | All              | ✔                   | `valids`          | 0.6.0             |
| Zero count                   | The number of zeros                                                                                                   | integer, numeric | ✔                   | `zeros`           | 0.6.0             |
| Negative value count         | The number of negative values                                                                                         | integer, numeric | ✔                   | `negatives`       | 0.6.0             |
| Positive value count         | The number of positive values                                                                                         | integer, numeric | ✔                   | `positives`       | 0.6.0             |
| Zero length string count     | The number of empty strings                                                                                           | string           | ✔                   | `zero_length`     | 0.6.0             |
| Non-zero length string count | The number of non-empty strings                                                                                       | string           | ✔                   | `non_zero_length` | 0.6.0             |
| True count                   | The number of true values                                                                                             | boolean          | ✔                   | `trues`           | 0.6.0             |
| False count                  | The number of false values                                                                                            | boolean          | ✔                   | `falses`          | 0.6.0             |

### General statistics

The general statistical information of a column.

| Metric             | Description                      | Column Type                | Profile Field | Supported Since |
| ------------------ | -------------------------------- | -------------------------- | ------------- | --------------- |
| Min                | The minimum value                | integer, numeric, datetime | `min`         | All             |
| Max                | The maximum value                | integer, numeric, datetime | `max`         | All             |
| Average            | The column average               | integer, numeric           | `avg`         | All             |
| Sum                | The column sum                   | integer, numeric           | `sum`         | All             |
| Standard deviation | The standard deviation of values | integer, numeric,          | `stddev`      | 0.4.0           |

### Text length statistics

The text length statistics of a column.

| Metric             | Description                             | Column Type | Profile Field | Since |
| ------------------ | --------------------------------------- | ----------- | ------------- | ----- |
| Min length         | The minimum string length               | string      | `min`         | 0.6.0 |
| Max length         | The maximum string length               | string      | `max`         | 0.6.0 |
| Average length     | The average string length               | string      | `avg`         | 0.6.0 |
| Standard deviation | The standard deviation of string length | string      | `stddev`      | 0.6.0 |

### Uniqueness

The uniqueness of a column.

![How PipeRider considers a row when determining uniqueness metrics](../.gitbook/assets/metrics-uniqueness-fs8.png)

| Metric              | Description                       | Column Type                        | Profile Field    | Since |
| ------------------- | --------------------------------- | ---------------------------------- | ---------------- | ----- |
| Distinct count      | The number of distinct items      | integer, string, datetime          | `distinct`       | All   |
| Duplicate count     | The number of recurring items     | integer, numeric, string, datetime | `duplicates`     | 0.6.0 |
| Non-duplicate count | The number of non-recurring items | integer, numeric, string, datetime | `non_duplicates` | 0.6.0 |

For example, the following dataset `(NULL, a, a, b, b, c, d, e)` would be categorized as so:

* Distinct count = 5, `(a, b, c, d, e)`
* Duplicate count = 4, `(a, a, b, b)`
* Non-duplicate count = 3, `(c, d, e)`
* Missing values (nulls) = 1

Therefore, the total number of rows for a table = missing (nulls) + duplicates + non-duplicates.

### Quantiles

The calculated quantiles of a numeric or integer column.

| Metric          | Description      | Column Type      | Profile Field | Supported Since |
| --------------- | ---------------- | ---------------- | ------------- | --------------- |
| Minimum         | 0th percentile   | integer, numeric | `min`         | All             |
| 5th Percentile  | 5th percentile   | integer, numeric | `p5`          | 0.4.0           |
| 25th Percentile | 25th percentile  | integer, numeric | `p25`         | 0.4.0           |
| Median          | 50th percentile  | integer, numeric | `p50`         | 0.4.0           |
| 75th Percentile | 75th percentile  | integer, numeric | `p75`         | 0.4.0           |
| 95th Percentile | 95th percentile  | integer, numeric | `p95`         | 0.4.0           |
| Maximum         | 100th percentile | integer, numeric | `max`         | All             |

### Distribution

| Metric                | Description                                                               | Column Type      | Profile Field | Supported Since |
| --------------------- | ------------------------------------------------------------------------- | ---------------- | ------------- | --------------- |
| Top K                 | The most frequently occurring n items and and counts                      | integer, string  | `topk`        | 0.6.0           |
| Histogram             | Evenly-split bins for numerical columns and counts for each bin           | integer, numeric | `histogram`   | 0.6.0           |
| Text length histogram | Evenly-split bins for text length and counts for each bin                 | string           | `histogram`   | 0.6.0           |
| Date histogram        | Histogram of date, month, or year. Bin split depends on the min/max range | datetime         | `histogram`   | 0.6.0           |
