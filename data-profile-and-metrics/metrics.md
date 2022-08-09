# Metrics

PipeRider analyzes the data source and produces a data profile that contains the following metrics.

## Table Metrics

Data profile metrics that describe data at the table level.

| Metric       | Description                        | Field       | Supported Since |
| ------------ | ---------------------------------- | ----------- | --------------- |
| Row count    | The number of rows in the table    | `row_count` | All             |
| Column count | The number of columns in the table | `col_count` | All             |
|              |                                    |             |                 |



## Column Metrics

Data profile metrics that describe the data at the column level. Depending on the column type, different metrics will be produced.

### Schema

In addition to logging the schema type of a column, PipeRider will also apply a generic schema type to a column that will determine how this column is treated by the profiler.

| Metric       | Description                                                                                                | Column Type | Field         | Supported Since |
| ------------ | ---------------------------------------------------------------------------------------------------------- | ----------- | ------------- | --------------- |
| Schema Type  | The column type defined in the data source                                                                 | All         | `schema_type` |                 |
| Generic Type | A generic type of schema type. It can be be `string`, `integer`, `numeric`, `datetime`, `boolean`, `other` | All         | `type`        |                 |
|              |                                                                                                            |             |               |                 |

The following metrics are produced based on the generic type that has been applied to the column.&#x20;

| Metric | Description | Column Type | Field | Since |
| --- | --- | --- | --- | --- |
| Missing count | The count of null values. | All types | `nulls` | 0.6.0 |
| Non null count | The count of non-null values. | All types | `non_nulls` |  |
| Invalid count | The count of values that does not match the schema type. For example, a string in a numeric column. It only happen in sqlite | All types | `invalids` | 0.6.0 |
| Valid count | The count of non-null and not invalid values | All types | `valids` | 0.6.0 |
| Zero count | The count of zero values | integer, numeric | `zeros` | 0.6.0 |
| Negative value count | The count of negative values | integer, numeric | `negatives` | 0.6.0 |
| Positive value count | The count of positive values | integer, numeric | `positives` | 0.6.0 |
| Zero length string count | The count of  empty strings | string | `zero_length` | 0.6.0 |
| Non zero length string count | The count of non empty strings | string | `non_zero_length` | 0.6.0 |
| True count | The count of true values | boolean | `trues` | 0.6.0 |
| False count | The count of false values | boolean | `falses` | 0.6.0 |

### General Statistic

The general statistic of a column

| Metric | Description | Column Type | Field | Since |
| --- | --- | --- | --- | --- |
| Min | The value of  the minimum item | integer, numeric, datetime | `min` |  |
| Max | The value of the maximum item | integer, numeric, datetime | `max` |  |
| Average | The average of a column. | integer, numeric | `avg` |  |
| Sum | The sum of a column. | integer, numeric | `sum` |  |
| Standard deviation | The standard deviation of a column. | integer, numeric, | `stddev` | 0.4.0 |

### Text Length Statistic

The text length statistic of a column.

| Metric | Description | Column Type | Field | Since |
| --- | --- | --- | --- | --- |
| Min length | The minimum text length of a string column | string | `min` | 0.6.0 |
| Max length | The maximum text length of a string column | string | `max` | 0.6.0 |
| Average length | The average text length of a string column | string | `avg` | 0.6.0 |
| Std. Deviation of length | The standard deviation text length of a string column | string | `stddev` | 0.6.0 |

### Uniqueness

Analyze the uniqueness of a column

- Distinct: Count of distinct items
- Duplicates: Count of recurring items

For example, if a dataset is `(NULL, a, a, b, b, c, d, e)`

- Distinct = 5, `(a, b, c, d, e)`
- Duplicates = 4, `(a, a, b, b)`
- Non-duplicates = 3, `(c, d, e)`
- Missing = 1

The total number = missing + duplicates + non-duplicates

![](assets/metrics-uniqueness.png)

| Metric | Description | Column Type | Field | Since |
| --- | --- | --- | --- | --- |
| Distinct | Count of distinct items | integer, string, datetime,  | `distinct` |  |
| Duplicates | Count of recurring items | integer, numeric, string, datetime | `duplicates` | 0.6.0 |
| Non duplicates | Count of non-recurring items | integer, numeric, string, datetime | `non_duplicates` | 0.6.0 |

### Quantiles

Calculate the quantiles of a numeric or integer column

| Metric | Description | Column Type | Field | Since |
| --- | --- | --- | --- | --- |
| min | min, 0th percentile | integer, numeric | `min` |  |
| 5th Percentile | 5th percentile | integer, numeric | `p5` | 0.4.0 |
| 25th Percentile | 25th percentile | integer, numeric | `p25` | 0.4.0 |
| Median | median, 50th percentile | integer, numeric | `p50` | 0.4.0 |
| 75th Percentile | 75th percentile | integer, numeric | `p75` | 0.4.0 |
| 95th Percentile | 95th percentile | integer, numeric | `p95` | 0.4.0 |
| max | max, 100th percentile | integer, numeric | `max` |  |

### Distribution

| Metric | Description | Column Type | Field | Since |
| --- | --- | --- | --- | --- |
| Top K | The top n frequent items and counts | integer, string | `topk` | 0.6.0 |
| histogram | The evenly-split bins. Calculate the counts for each bin. | integer, numeric | `histogram` | 0.6.0 |
| Text length histogram | The evenly-split bins for text length. Calculate the counts for each bin. | string | `histogram` | 0.6.0 |
| Date histogram | The histogram of date, month, or year. Depends on the data min/max range. | datetime | `histogram` | 0.6.0 |
