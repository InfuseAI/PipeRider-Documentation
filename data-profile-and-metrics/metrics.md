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

| Metric                        | Description                                                                     | Profile Field      | Assertion Available | PipeRider Version |
| ----------------------------- | ------------------------------------------------------------------------------- | ------------------ | ------------------- | ----------------- |
| Row count                     | The number of rows in the table                                                 | `row_count`        | ✔                   | All               |
| Column count                  | The number of columns in the table                                              | `col_count`        |                     | All               |
| Sample count                  | The number of rows profiled                                                     | `samples`          |                     | 0.10.0            |
| Sample percentage             | The percentage of rows profiled                                                 | `samples_p`        |                     | 0.11.0            |
| Volume size \*                | The volume size of the table in bytes                                           | `bytes`            | ✔                   | 0.8.0             |
| Created time \*               | The time that the table was created, including time zone, in ISO 8601 format    | `created`          |                     | 0.8.0             |
| Last altered time \*          | The last time the table was modified, including time zone, in ISO 8601 format   | `last_altered`     |                     | 0.8.0             |
| Freshness \*                  | The time differentiation between the current time and table's last altered time | `freshness`        | ✔                   | 0.8.0             |
| Duplicate row count \*\*      | The number of duplicate rows in the table                                       | `duplicate_rows`   | ✔                   | 0.10.0            |
| Duplicate row percentage \*\* | The percentage of duplicate rows in the table                                   | `duplicate_rows_p` | ✔                   | 0.11.0            |

{% hint style="info" %}
\* \_\_ These metrics are only available for certain data sources. Please refer to the **platform dependent metrics** table below for availability information.\
\
\*\* Table-level duplicate row metrics are now enabled by default. To enable this settings please refer to the [Profiler Settings](../project-structure/config.yml.md#profiler-settings).
{% endhint %}

### Platform dependent metrics

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

| Metric       | Description                                                                                | Profile Field | Column Type | PipeRider Version |
| ------------ | ------------------------------------------------------------------------------------------ | ------------- | ----------- | ----------------- |
| Schema Type  | The column type defined in the data source                                                 | `schema_type` | All         | All               |
| Generic Type | A generic schema type of `string`, `integer`, `numeric`, `datetime`, `boolean`, or `other` | `type`        | All         | All               |

The following metrics are produced based on the generic type that has been applied to the column.

### Data composition

The composition of the data contained within a column.

<figure><img src="../.gitbook/assets/metrics-composition.png" alt=""><figcaption><p>The generic type of a column determines the available metrics</p></figcaption></figure>

| Metric                            | Description                                                                                                          | Profile Field       | Column Type      | Assertion Available | PipeRider Version |
| --------------------------------- | -------------------------------------------------------------------------------------------------------------------- | ------------------- | ---------------- | ------------------- | ----------------- |
| Row count                         | The number of rows in the table                                                                                      | `total`             | All              | ✔                   | All               |
| Sample count                      | The number of rows profiled                                                                                          | `samples`           | All              | ✔                   | 0.10.0            |
| Sample percentage                 | The percentage of rows profiled                                                                                      | `samples_p`         | All              | ✔                   | 0.11.0            |
| Missing count                     | The number of null values                                                                                            | `nulls`             | All              | ✔                   | 0.6.0             |
| Missing percentage                | The percentage of null values                                                                                        | `nulls_p`           | All              | ✔                   | 0.11.0            |
| Non-null count                    | The number of non-null values                                                                                        | `non_nulls`         | All              | ✔                   | All               |
| Non-null percentage               | The percentage of non-null values                                                                                    | `non_nulls_p`       | All              | ✔                   | 0.11.0            |
| Invalid count                     | The number of values that do not match the column's schema type. E.g. A string in a numeric column (**SQLite only**) | `invalids`          | All              | ✔                   | 0.6.0             |
| Invalid percentage                | The percentage of invalid values (**SQLite only**)                                                                   | `invalids_p`        | All              | ✔                   | 0.11.0            |
| Valid count                       | The count of non-null values minus invalid values                                                                    | `valids`            | All              | ✔                   | 0.6.0             |
| Valid percentage                  | The percentage of non-null values, minus invalid values                                                              | `valids_p`          | All              | ✔                   | 0.11.0            |
| Zero count                        | The number of zeros                                                                                                  | `zeros`             | integer, numeric | ✔                   | 0.6.0             |
| Zero percentage                   | The percentage of zeros                                                                                              | `zeros_p`           | integer, numeric | ✔                   | 0.11.0            |
| Negative value count              | The number of negative values                                                                                        | `negatives`         | integer, numeric | ✔                   | 0.6.0             |
| Negative value percentage         | The percentage of negative values                                                                                    | `negatives_p`       | integer, numeric | ✔                   | 0.11.0            |
| Positive value count              | The number of positive values                                                                                        | `positives`         | integer, numeric | ✔                   | 0.6.0             |
| Positive value percentage         | The percentage of positive values                                                                                    | `positives_p`       | integer, numeric | ✔                   | 0.11.0            |
| Zero length string count          | The number of empty strings                                                                                          | `zero_length`       | string           | ✔                   | 0.6.0             |
| Zero length string percentage     | The percentage of empty strings                                                                                      | `zero_length_p`     | string           | ✔                   | 0.11.0            |
| Non-zero length string count      | The number of non-empty strings                                                                                      | `non_zero_length`   | string           | ✔                   | 0.6.0             |
| Non-zero length string percentage | The percentage of non-empty strings                                                                                  | `non_zero_length_p` | string           | ✔                   | 0.11.0            |
| True count                        | The number of true values                                                                                            | `trues`             | boolean          | ✔                   | 0.6.0             |
| True percentage                   | The percentage of true values                                                                                        | `trues_p`           | boolean          | ✔                   | 0.11.0            |
| False count                       | The number of false values                                                                                           | `falses`            | boolean          | ✔                   | 0.6.0             |
| False percentage                  | The percentage of false values                                                                                       | `falses_p`          | boolean          | ✔                   | 0.11.0            |

### General statistics

The general statistical information of a column.

| Metric             | Description                      | Column Type                | Profile Field | Assertion Available | PipeRider Version |
| ------------------ | -------------------------------- | -------------------------- | ------------- | ------------------- | ----------------- |
| Min                | The minimum value                | integer, numeric, datetime | `min`         | ✔                   | All               |
| Max                | The maximum value                | integer, numeric, datetime | `max`         | ✔                   | All               |
| Average            | The column average               | integer, numeric           | `avg`         | ✔                   | All               |
| Sum                | The column sum                   | integer, numeric           | `sum`         | ✔                   | All               |
| Standard deviation | The standard deviation of values | integer, numeric,          | `stddev`      | ✔                   | 0.4.0             |

### Text length statistics

The text length statistics of a column.

| Metric             | Description                             | Column Type | Profile Field | Assertion Available | PipeRider Version |
| ------------------ | --------------------------------------- | ----------- | ------------- | ------------------- | ----------------- |
| Min length         | The minimum string length               | string      | `min`         | ✔                   | 0.6.0             |
| Max length         | The maximum string length               | string      | `max`         | ✔                   | 0.6.0             |
| Average length     | The average string length               | string      | `avg`         | ✔                   | 0.6.0             |
| Standard deviation | The standard deviation of string length | string      | `stddev`      | ✔                   | 0.6.0             |

### Uniqueness

The uniqueness of a column.

<figure><img src="../.gitbook/assets/metrics-uniqueness.png" alt=""><figcaption><p>Column uniqueness</p></figcaption></figure>

| Metric                   | Description                           | Profile Field      | Column Type                        | Assertion Available | PipeRider Version |
| ------------------------ | ------------------------------------- | ------------------ | ---------------------------------- | ------------------- | ----------------- |
| Distinct count           | The number of distinct items          | `distinct`         | integer, string, datetime          | ✔                   | All               |
| Distinct percentage      | The percentage of distinct items      | `distinct_p`       | integer, string, datetime          | ✔                   | 0.11.0            |
| Duplicate count          | The number of recurring items         | `duplicates`       | integer, numeric, string, datetime | ✔                   | 0.6.0             |
| Duplicate percentage     | The percentage of duplicate items     | `duplicates_p`     | integer, numeric, string, datetime | ✔                   | 0.11.0            |
| Non-duplicate count      | The number of non-recurring items     | `non_duplicates`   | integer, numeric, string, datetime | ✔                   | 0.6.0             |
| Non-duplicate percentage | The percentage of non-duplicate items | `non_duplicates_p` | integer, numeric, string, datetime | ✔                   | 0.11.0            |

For example, the following dataset `(NULL, a, a, b, b, c, d, e)` would be categorized as so:

* Distinct count = 5, `(a, b, c, d, e)`
* Duplicate count = 4, `(a, a, b, b)`
* Non-duplicate count = 3, `(c, d, e)`
* Missing values (nulls) = 1

Therefore, the total number of rows for a table = missing (nulls) + duplicates + non-duplicates.

### Quantiles

The calculated quantiles of a numeric or integer column.

| Metric          | Description      | Profile Field | Column Type      | Assertion Available | PipeRider Version |
| --------------- | ---------------- | ------------- | ---------------- | ------------------- | ----------------- |
| Minimum         | 0th percentile   | `min`         | integer, numeric | ✔                   | All               |
| 5th Percentile  | 5th percentile   | `p5`          | integer, numeric | ✔                   | 0.4.0             |
| 25th Percentile | 25th percentile  | `p25`         | integer, numeric | ✔                   | 0.4.0             |
| Median          | 50th percentile  | `p50`         | integer, numeric | ✔                   | 0.4.0             |
| 75th Percentile | 75th percentile  | `p75`         | integer, numeric | ✔                   | 0.4.0             |
| 95th Percentile | 95th percentile  | `p95`         | integer, numeric | ✔                   | 0.4.0             |
| Maximum         | 100th percentile | `max`         | integer, numeric | ✔                   | All               |

### Distribution

| Metric                | Description                                                               | Profile Field | Column Type      | PipeRider Version |
| --------------------- | ------------------------------------------------------------------------- | ------------- | ---------------- | ----------------- |
| Top K                 | The most frequently occurring n items and and counts                      | `topk`        | integer, string  | 0.6.0             |
| Histogram             | Evenly-split bins for numerical columns and counts for each bin           | `histogram`   | integer, numeric | 0.6.0             |
| Text length histogram | Evenly-split bins for text length and counts for each bin                 | `histogram`   | string           | 0.6.0             |
| Date histogram        | Histogram of date, month, or year. Bin split depends on the min/max range | `histogram`   | datetime         | 0.6.0             |
