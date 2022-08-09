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

