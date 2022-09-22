---
description: The data profile provides data about your data
---

# Data Profile

The data profiler is at the core of how PipeRider works. PipeRider helps you understand the structure of your data by providing statistical metrics and data distribution information about the table and columns in your data source. When paired with [data assertions](../data-quality-assertions/assertion-configuration.md), the data profile provides a way to check the quality and reliability of your data.

Each time you run PipeRider, a new data profile is created and stored in the folder for that run. E.g. `.piperider/outputs/<run-name>/run.json`

The data profile in each `run.json` is used to create the PipeRider report for that specific run; and also by the assertions engine, which tests the profile against any [built-in](../data-quality-assertions/assertion-configuration.md) or [custom](../data-quality-assertions/custom-assertions.md) assertions.

### Example data profile

The following is a truncated data profile to show the type of metrics you can expect to find. Please refer to [Metrics](metrics.md) for complete information about profile metrics.

```json
{
    "tables": {
        "month": {
            "name": "month",
            "row_count": 203882,
            "col_count": 8,
            "columns": {
                "passenger_count": {
                    "name": "passenger_count",
                    "type": "integer",
                    "schema_type": "INTEGER",
                    "total": 203882,
                    "non_nulls": 203882,
                    "nulls": 0,
                    "valids": 203882,
                    "invalids": 0,
                    "zeros": 0,
                    "negatives": 0,
                    "positives": 203882,
                    "distinct": 5,
                    "min": 1,
                    "max": 5,
                    "sum": 341589,
                    "avg": 1.6754250007357196,
                    "stddev": 1.225311911788547,
                    "duplicates": 203882,
                    "non_duplicates": 0,
                    "histogram": {
                        "labels": [
                            "1",
                            "2",
                            "3",
                            "4",
                            "5"
                        ],
                        "counts": [
                            138179,
                            34272,
                            8986,
                            4317,
                            18128
                        ],
                        "bin_edges": [
                            1,
                            2,
                            3,
                            4,
                            5,
                            6
                        ]
                    },
                    "p5": 1,
                    "p25": 1,
                    "p50": 1,
                    "p75": 2,
                    "p95": 5,
                    "topk": {
                        "values": [
                            "1",
                            "2",
                            "5",
                            "3",
                            "4"
                        ],
                        "counts": [
                            138179,
                            34272,
                            18128,
                            8986,
                            4317
                        ]
                    },
                    "distribution": {
                        "type": "histogram",
                        "labels": [
                            "1",
                            "2",
                            "3",
                            "4",
                            "5"
                        ],
                        "counts": [
                            138179,
                            34272,
                            8986,
                            4317,
                            18128
                        ],
                        "bin_edges": [
                            1,
                            2,
                            3,
                            4,
                            5,
                            6
                        ]
                    },
                    "profile_duration": "0.16",
                    "elapsed_milli": 162,
                    "description": "Description: N/A"
                }
            },
```
