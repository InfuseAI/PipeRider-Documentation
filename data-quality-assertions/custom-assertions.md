---
description: How to create custom assertions to check the quality of your data.
---

# Custom Assertions

Piperider provides a few [built-in assertions](assertion-configuration.md) and also supports custom assertions as _plugins_ which can satisfy the data quality check on your demand. Here you will learn the magic and create your first custom assertion.

## How to Create an Assertion Function

### Plugins

Piperider, by default, will load python files under `.piperider/plugins` as custom assertion functions automatically. `.piperider/plugins` is created by `piperider init` with a scaffolding of a custom assertion function, `customized_assertions.py`. You can rename the file or create assertion functions in other python file there.

{% hint style="info" %}
The search path to `plugins/` can be overwritten by the environment variable **`PIPERIDER_PLUGINS`**. Define your path by setting the variable.
{% endhint %}

### Metrics

`piperider run` will generate profiling results containing a plenty of metrics that your assertions could refer to those metrics for the data quality measurement. Metrics are saved in the `.piperider/outputs/<run>/run.json` and the context are categorized into tables/columns according to data types, _String_, _Datetime_, and _Numeric_.

Here is an example of these types of profiling metrics. Table PRICE contains SYMBOL(string), DATE(datetime) and OPEN(numeric).

<details>

<summary>The sample of run.json</summary>

```
{
  "tables": {
    "ACTION": {
      "name": "ACTION",
      "row_count": 1960,
      "samples": 1960,
      "col_count": 4,
      "columns": {
        "SYMBOL": {
          "name": "SYMBOL",
          "type": "string",
          "schema_type": "VARCHAR(16777216)",
          "total": 1960,
          "samples": 1960,
          "non_nulls": 1960,
          "nulls": 0,
          "valids": 1960,
          "invalids": 0,
          "zero_length": 0,
          "non_zero_length": 1960,
          "distinct": 396,
          "min": 1,
          "max": 5,
          "sum": 5986,
          "avg": 3.054081632653061,
          "stddev": 0.6634432075983672,
          "duplicates": 1955,
          "non_duplicates": 5,
          "topk": {
            "values": [
              "O",
              "WY",
              "PXD",
              "EOG",
              "COP",
              "ZBH",
              "WRB",
              "TXT",
              "TROW",
              "SYY",
              "SLB",
              "SHW",
              "RJF",
              "PGR",
              "PCAR",
              "NVDA",
              "MRK",
              "MCHP",
              "IP",
              "IBM"
            ],
            "counts": [
              16,
              7,
              7,
              7,
              7,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6
            ]
          },
          "histogram": {
            "labels": [
              "1",
              "2",
              "3",
              "4",
              "5"
            ],
            "counts": [
              58,
              202,
              1281,
              414,
              5
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
          "distribution": {
            "type": "topk",
            "labels": [
              "O",
              "WY",
              "PXD",
              "EOG",
              "COP",
              "ZBH",
              "WRB",
              "TXT",
              "TROW",
              "SYY",
              "SLB",
              "SHW",
              "RJF",
              "PGR",
              "PCAR",
              "NVDA",
              "MRK",
              "MCHP",
              "IP",
              "IBM"
            ],
            "counts": [
              16,
              7,
              7,
              7,
              7,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6,
              6
            ]
          },
          "profile_duration": "0.02",
          "elapsed_milli": 22
        },
        "DATE": {
          "name": "DATE",
          "type": "datetime",
          "schema_type": "DATE",
          "total": 1960,
          "samples": 1960,
          "non_nulls": 1960,
          "nulls": 0,
          "valids": 1960,
          "invalids": 0,
          "distinct": 289,
          "min": "2021-01-04",
          "max": "2022-03-31",
          "duplicates": 1921,
          "non_duplicates": 39,
          "histogram": {
            "labels": [
              "2021-01-01 - 2021-02-01",
              "2021-02-01 - 2021-03-01",
              "2021-03-01 - 2021-04-01",
              "2021-04-01 - 2021-05-01",
              "2021-05-01 - 2021-06-01",
              "2021-06-01 - 2021-07-01",
              "2021-07-01 - 2021-08-01",
              "2021-08-01 - 2021-09-01",
              "2021-09-01 - 2021-10-01",
              "2021-10-01 - 2021-11-01",
              "2021-11-01 - 2021-12-01",
              "2021-12-01 - 2022-01-01",
              "2022-01-01 - 2022-02-01",
              "2022-02-01 - 2022-03-01",
              "2022-03-01 - 2022-04-01"
            ],
            "counts": [
              67,
              148,
              164,
              73,
              162,
              149,
              80,
              168,
              149,
              74,
              174,
              154,
              70,
              156,
              172
            ],
            "bin_edges": [
              "2021-01-01",
              "2021-02-01",
              "2021-03-01",
              "2021-04-01",
              "2021-05-01",
              "2021-06-01",
              "2021-07-01",
              "2021-08-01",
              "2021-09-01",
              "2021-10-01",
              "2021-11-01",
              "2021-12-01",
              "2022-01-01",
              "2022-02-01",
              "2022-03-01",
              "2022-04-01"
            ]
          },
          "distribution": {
            "type": "monthly",
            "labels": [
              "2021-01-01 - 2021-02-01",
              "2021-02-01 - 2021-03-01",
              "2021-03-01 - 2021-04-01",
              "2021-04-01 - 2021-05-01",
              "2021-05-01 - 2021-06-01",
              "2021-06-01 - 2021-07-01",
              "2021-07-01 - 2021-08-01",
              "2021-08-01 - 2021-09-01",
              "2021-09-01 - 2021-10-01",
              "2021-10-01 - 2021-11-01",
              "2021-11-01 - 2021-12-01",
              "2021-12-01 - 2022-01-01",
              "2022-01-01 - 2022-02-01",
              "2022-02-01 - 2022-03-01",
              "2022-03-01 - 2022-04-01"
            ],
            "counts": [
              67,
              148,
              164,
              73,
              162,
              149,
              80,
              168,
              149,
              74,
              174,
              154,
              70,
              156,
              172
            ],
            "bin_edges": [
              "2021-01-01",
              "2021-02-01",
              "2021-03-01",
              "2021-04-01",
              "2021-05-01",
              "2021-06-01",
              "2021-07-01",
              "2021-08-01",
              "2021-09-01",
              "2021-10-01",
              "2021-11-01",
              "2021-12-01",
              "2022-01-01",
              "2022-02-01",
              "2022-03-01",
              "2022-04-01"
            ]
          },
          "profile_duration": "0.02",
          "elapsed_milli": 22
        },
        "DIVIDENDS": {
          "name": "DIVIDENDS",
          "type": "numeric",
          "schema_type": "NUMERIC(8, 4)",
          "total": 1960,
          "samples": 1960,
          "non_nulls": 1960,
          "nulls": 0,
          "valids": 1960,
          "invalids": 0,
          "zeros": 17,
          "negatives": 0,
          "positives": 1943,
          "distinct": 284,
          "min": 0,
          "max": 15.3298,
          "sum": 1195.5275000000004,
          "avg": 0.6099630102040818,
          "stddev": 0.6130400574938016,
          "duplicates": 1902,
          "non_duplicates": 58,
          "histogram": {
            "labels": [
              "0 _ 0.307",
              "0.307 _ 0.613",
              "0.613 _ 0.920",
              "0.920 _ 1.23",
              "1.23 _ 1.53",
              "1.53 _ 1.84",
              "1.84 _ 2.15",
              "2.15 _ 2.45",
              "2.45 _ 2.76",
              "2.76 _ 3.07",
              "3.07 _ 3.37",
              "3.37 _ 3.68",
              "3.68 _ 3.99",
              "3.99 _ 4.29",
              "4.29 _ 4.60",
              "4.60 _ 4.91",
              "4.91 _ 5.21",
              "5.21 _ 5.52",
              "5.52 _ 5.83",
              "5.83 _ 6.13",
              "6.13 _ 6.44",
              "6.44 _ 6.75",
              "6.75 _ 7.05",
              "7.05 _ 7.36",
              "7.36 _ 7.66",
              "7.66 _ 7.97",
              "7.97 _ 8.28",
              "8.28 _ 8.58",
              "8.58 _ 8.89",
              "8.89 _ 9.20",
              "9.20 _ 9.50",
              "9.50 _ 9.81",
              "9.81 _ 10.12",
              "10.12 _ 10.42",
              "10.42 _ 10.73",
              "10.73 _ 11.04",
              "11.04 _ 11.34",
              "11.34 _ 11.65",
              "11.65 _ 11.96",
              "11.96 _ 12.26",
              "12.26 _ 12.57",
              "12.57 _ 12.88",
              "12.88 _ 13.18",
              "13.18 _ 13.49",
              "13.49 _ 13.80",
              "13.80 _ 14.10",
              "14.10 _ 14.41",
              "14.41 _ 14.72",
              "14.72 _ 15.02",
              "15.02 _ 15.33"
            ],
            "counts": [
              604,
              625,
              346,
              225,
              82,
              36,
              15,
              1,
              3,
              8,
              2,
              3,
              1,
              6,
              0,
              2,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              1
            ],
            "bin_edges": [
              0,
              0.30659600000000004,
              0.6131920000000001,
              0.9197880000000002,
              1.2263840000000001,
              1.5329800000000002,
              1.8395760000000003,
              2.1461720000000004,
              2.4527680000000003,
              2.759364,
              3.0659600000000005,
              3.3725560000000003,
              3.6791520000000006,
              3.9857480000000005,
              4.292344000000001,
              4.598940000000001,
              4.905536000000001,
              5.212132,
              5.518728,
              5.825324000000001,
              6.131920000000001,
              6.438516000000001,
              6.745112000000001,
              7.0517080000000005,
              7.358304000000001,
              7.664900000000001,
              7.971496000000001,
              8.278092000000001,
              8.584688000000002,
              8.891284,
              9.197880000000001,
              9.504476,
              9.811072000000001,
              10.117668000000002,
              10.424264,
              10.730860000000002,
              11.037456,
              11.344052000000001,
              11.650648000000002,
              11.957244000000001,
              12.263840000000002,
              12.570436,
              12.877032000000002,
              13.183628000000002,
              13.490224000000001,
              13.796820000000002,
              14.103416000000001,
              14.410012000000002,
              14.716608000000003,
              15.023204000000002,
              15.329800000000002
            ]
          },
          "p5": 0.0925,
          "p25": 0.26,
          "p50": 0.5,
          "p75": 0.8,
          "p95": 1.48,
          "distribution": {
            "type": "histogram",
            "labels": [
              "0 _ 0.307",
              "0.307 _ 0.613",
              "0.613 _ 0.920",
              "0.920 _ 1.23",
              "1.23 _ 1.53",
              "1.53 _ 1.84",
              "1.84 _ 2.15",
              "2.15 _ 2.45",
              "2.45 _ 2.76",
              "2.76 _ 3.07",
              "3.07 _ 3.37",
              "3.37 _ 3.68",
              "3.68 _ 3.99",
              "3.99 _ 4.29",
              "4.29 _ 4.60",
              "4.60 _ 4.91",
              "4.91 _ 5.21",
              "5.21 _ 5.52",
              "5.52 _ 5.83",
              "5.83 _ 6.13",
              "6.13 _ 6.44",
              "6.44 _ 6.75",
              "6.75 _ 7.05",
              "7.05 _ 7.36",
              "7.36 _ 7.66",
              "7.66 _ 7.97",
              "7.97 _ 8.28",
              "8.28 _ 8.58",
              "8.58 _ 8.89",
              "8.89 _ 9.20",
              "9.20 _ 9.50",
              "9.50 _ 9.81",
              "9.81 _ 10.12",
              "10.12 _ 10.42",
              "10.42 _ 10.73",
              "10.73 _ 11.04",
              "11.04 _ 11.34",
              "11.34 _ 11.65",
              "11.65 _ 11.96",
              "11.96 _ 12.26",
              "12.26 _ 12.57",
              "12.57 _ 12.88",
              "12.88 _ 13.18",
              "13.18 _ 13.49",
              "13.49 _ 13.80",
              "13.80 _ 14.10",
              "14.10 _ 14.41",
              "14.41 _ 14.72",
              "14.72 _ 15.02",
              "15.02 _ 15.33"
            ],
            "counts": [
              604,
              625,
              346,
              225,
              82,
              36,
              15,
              1,
              3,
              8,
              2,
              3,
              1,
              6,
              0,
              2,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              1
            ],
            "bin_edges": [
              0,
              0.30659600000000004,
              0.6131920000000001,
              0.9197880000000002,
              1.2263840000000001,
              1.5329800000000002,
              1.8395760000000003,
              2.1461720000000004,
              2.4527680000000003,
              2.759364,
              3.0659600000000005,
              3.3725560000000003,
              3.6791520000000006,
              3.9857480000000005,
              4.292344000000001,
              4.598940000000001,
              4.905536000000001,
              5.212132,
              5.518728,
              5.825324000000001,
              6.131920000000001,
              6.438516000000001,
              6.745112000000001,
              7.0517080000000005,
              7.358304000000001,
              7.664900000000001,
              7.971496000000001,
              8.278092000000001,
              8.584688000000002,
              8.891284,
              9.197880000000001,
              9.504476,
              9.811072000000001,
              10.117668000000002,
              10.424264,
              10.730860000000002,
              11.037456,
              11.344052000000001,
              11.650648000000002,
              11.957244000000001,
              12.263840000000002,
              12.570436,
              12.877032000000002,
              13.183628000000002,
              13.490224000000001,
              13.796820000000002,
              14.103416000000001,
              14.410012000000002,
              14.716608000000003,
              15.023204000000002,
              15.329800000000002
            ]
          },
          "profile_duration": "0.02",
          "elapsed_milli": 22
        },
        "SPLITS": {
          "name": "SPLITS",
          "type": "numeric",
          "schema_type": "NUMERIC(10, 2)",
          "total": 1960,
          "samples": 1960,
          "non_nulls": 1960,
          "nulls": 0,
          "valids": 1960,
          "invalids": 0,
          "zeros": 0,
          "negatives": 0,
          "positives": 1960,
          "distinct": 11,
          "min": 0.13,
          "max": 4,
          "sum": 1974.93,
          "avg": 1.0076173469387755,
          "stddev": 0.1304681424606919,
          "duplicates": 1956,
          "non_duplicates": 4,
          "histogram": {
            "labels": [
              "0.130 _ 0.207",
              "0.207 _ 0.285",
              "0.285 _ 0.362",
              "0.362 _ 0.440",
              "0.440 _ 0.517",
              "0.517 _ 0.594",
              "0.594 _ 0.672",
              "0.672 _ 0.749",
              "0.749 _ 0.827",
              "0.827 _ 0.904",
              "0.904 _ 0.981",
              "0.981 _ 1.06",
              "1.06 _ 1.14",
              "1.14 _ 1.21",
              "1.21 _ 1.29",
              "1.29 _ 1.37",
              "1.37 _ 1.45",
              "1.45 _ 1.52",
              "1.52 _ 1.60",
              "1.60 _ 1.68",
              "1.68 _ 1.76",
              "1.76 _ 1.83",
              "1.83 _ 1.91",
              "1.91 _ 1.99",
              "1.99 _ 2.06",
              "2.06 _ 2.14",
              "2.14 _ 2.22",
              "2.22 _ 2.30",
              "2.30 _ 2.37",
              "2.37 _ 2.45",
              "2.45 _ 2.53",
              "2.53 _ 2.61",
              "2.61 _ 2.68",
              "2.68 _ 2.76",
              "2.76 _ 2.84",
              "2.84 _ 2.92",
              "2.92 _ 2.99",
              "2.99 _ 3.07",
              "3.07 _ 3.15",
              "3.15 _ 3.23",
              "3.23 _ 3.30",
              "3.30 _ 3.38",
              "3.38 _ 3.46",
              "3.46 _ 3.54",
              "3.54 _ 3.61",
              "3.61 _ 3.69",
              "3.69 _ 3.77",
              "3.77 _ 3.85",
              "3.85 _ 3.92",
              "3.92 _ 4.00"
            ],
            "counts": [
              1,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              1947,
              1,
              1,
              0,
              0,
              1,
              2,
              0,
              0,
              0,
              0,
              0,
              0,
              2,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              3,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              2
            ],
            "bin_edges": [
              0.13,
              0.2074,
              0.2848,
              0.36219999999999997,
              0.4396,
              0.517,
              0.5944,
              0.6718,
              0.7492,
              0.8266,
              0.904,
              0.9813999999999999,
              1.0588,
              1.1362,
              1.2136,
              1.291,
              1.3683999999999998,
              1.4457999999999998,
              1.5232,
              1.6006,
              1.678,
              1.7553999999999998,
              1.8327999999999998,
              1.9102000000000001,
              1.9876,
              2.065,
              2.1424,
              2.2197999999999998,
              2.2971999999999997,
              2.3745999999999996,
              2.452,
              2.5294,
              2.6068,
              2.6841999999999997,
              2.7615999999999996,
              2.839,
              2.9164,
              2.9938,
              3.0711999999999997,
              3.1485999999999996,
              3.226,
              3.3034,
              3.3808,
              3.4581999999999997,
              3.5355999999999996,
              3.613,
              3.6904,
              3.7678,
              3.8451999999999997,
              3.9225999999999996,
              3.9999999999999996
            ]
          },
          "p5": 1,
          "p25": 1,
          "p50": 1,
          "p75": 1,
          "p95": 1,
          "distribution": {
            "type": "histogram",
            "labels": [
              "0.130 _ 0.207",
              "0.207 _ 0.285",
              "0.285 _ 0.362",
              "0.362 _ 0.440",
              "0.440 _ 0.517",
              "0.517 _ 0.594",
              "0.594 _ 0.672",
              "0.672 _ 0.749",
              "0.749 _ 0.827",
              "0.827 _ 0.904",
              "0.904 _ 0.981",
              "0.981 _ 1.06",
              "1.06 _ 1.14",
              "1.14 _ 1.21",
              "1.21 _ 1.29",
              "1.29 _ 1.37",
              "1.37 _ 1.45",
              "1.45 _ 1.52",
              "1.52 _ 1.60",
              "1.60 _ 1.68",
              "1.68 _ 1.76",
              "1.76 _ 1.83",
              "1.83 _ 1.91",
              "1.91 _ 1.99",
              "1.99 _ 2.06",
              "2.06 _ 2.14",
              "2.14 _ 2.22",
              "2.22 _ 2.30",
              "2.30 _ 2.37",
              "2.37 _ 2.45",
              "2.45 _ 2.53",
              "2.53 _ 2.61",
              "2.61 _ 2.68",
              "2.68 _ 2.76",
              "2.76 _ 2.84",
              "2.84 _ 2.92",
              "2.92 _ 2.99",
              "2.99 _ 3.07",
              "3.07 _ 3.15",
              "3.15 _ 3.23",
              "3.23 _ 3.30",
              "3.30 _ 3.38",
              "3.38 _ 3.46",
              "3.46 _ 3.54",
              "3.54 _ 3.61",
              "3.61 _ 3.69",
              "3.69 _ 3.77",
              "3.77 _ 3.85",
              "3.85 _ 3.92",
              "3.92 _ 4.00"
            ],
            "counts": [
              1,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              1947,
              1,
              1,
              0,
              0,
              1,
              2,
              0,
              0,
              0,
              0,
              0,
              0,
              2,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              3,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              0,
              2
            ],
            "bin_edges": [
              0.13,
              0.2074,
              0.2848,
              0.36219999999999997,
              0.4396,
              0.517,
              0.5944,
              0.6718,
              0.7492,
              0.8266,
              0.904,
              0.9813999999999999,
              1.0588,
              1.1362,
              1.2136,
              1.291,
              1.3683999999999998,
              1.4457999999999998,
              1.5232,
              1.6006,
              1.678,
              1.7553999999999998,
              1.8327999999999998,
              1.9102000000000001,
              1.9876,
              2.065,
              2.1424,
              2.2197999999999998,
              2.2971999999999997,
              2.3745999999999996,
              2.452,
              2.5294,
              2.6068,
              2.6841999999999997,
              2.7615999999999996,
              2.839,
              2.9164,
              2.9938,
              3.0711999999999997,
              3.1485999999999996,
              3.226,
              3.3034,
              3.3808,
              3.4581999999999997,
              3.5355999999999996,
              3.613,
              3.6904,
              3.7678,
              3.8451999999999997,
              3.9225999999999996,
              3.9999999999999996
            ]
          },
          "profile_duration": "0.02",
          "elapsed_milli": 20
        }
      },
      "piperider_assertion_result": {
        "tests": [
          {
            "name": "assert_row_count",
            "status": "passed",
            "parameters": {},
            "expected": {
              "min": 1764
            },
            "actual": 1960,
            "tags": [
              "RECOMMENDED"
            ]
          }
        ],
        "columns": {
          "SYMBOL": [
            {
              "name": "assert_column_schema_type",
              "status": "passed",
              "parameters": {},
              "expected": {
                "schema_type": "VARCHAR(16777216)"
              },
              "actual": "VARCHAR(16777216)",
              "tags": [
                "RECOMMENDED"
              ]
            },
            {
              "name": "assert_column_not_null",
              "status": "passed",
              "parameters": {},
              "expected": {
                "success": true
              },
              "actual": {
                "success": true
              },
              "tags": [
                "RECOMMENDED"
              ]
            }
          ],
          "DATE": [
            {
              "name": "assert_column_schema_type",
              "status": "passed",
              "parameters": {},
              "expected": {
                "schema_type": "DATE"
              },
              "actual": "DATE",
              "tags": [
                "RECOMMENDED"
              ]
            },
            {
              "name": "assert_column_not_null",
              "status": "passed",
              "parameters": {},
              "expected": {
                "success": true
              },
              "actual": {
                "success": true
              },
              "tags": [
                "RECOMMENDED"
              ]
            }
          ],
          "DIVIDENDS": [
            {
              "name": "assert_column_schema_type",
              "status": "passed",
              "parameters": {},
              "expected": {
                "schema_type": "NUMERIC(8, 4)"
              },
              "actual": "NUMERIC(8, 4)",
              "tags": [
                "RECOMMENDED"
              ]
            },
            {
              "name": "assert_column_max_in_range",
              "status": "passed",
              "parameters": {},
              "expected": {
                "max": [
                  13.7968,
                  16.8628
                ]
              },
              "actual": {
                "max": 15.3298
              },
              "tags": [
                "RECOMMENDED"
              ]
            },
            {
              "name": "assert_column_not_null",
              "status": "passed",
              "parameters": {},
              "expected": {
                "success": true
              },
              "actual": {
                "success": true
              },
              "tags": [
                "RECOMMENDED"
              ]
            }
          ],
          "SPLITS": [
            {
              "name": "assert_column_schema_type",
              "status": "passed",
              "parameters": {},
              "expected": {
                "schema_type": "NUMERIC(10, 2)"
              },
              "actual": "NUMERIC(10, 2)",
              "tags": [
                "RECOMMENDED"
              ]
            },
            {
              "name": "assert_column_max_in_range",
              "status": "passed",
              "parameters": {},
              "expected": {
                "max": [
                  3.6,
                  4.4
                ]
              },
              "actual": {
                "max": 4
              },
              "tags": [
                "RECOMMENDED"
              ]
            },
            {
              "name": "assert_column_not_null",
              "status": "passed",
              "parameters": {},
              "expected": {
                "success": true
              },
              "actual": {
                "success": true
              },
              "tags": [
                "RECOMMENDED"
              ]
            }
          ]
        }
      }
    },
    ...
```

</details>

### Assertion

When you first time execute `piperider run` , You will be prompted for the generation of recommended assertions, if _yes_, recommended assertions will be generated, otherwise, assertion scaffoldings will be generated at `.piperider/assertions`.

```
No assertion found
Do you want to auto generate recommended assertions for this datasource [Yes/no]
```

The scaffolding of assertion yaml looks like below

```yaml
your_table_name:
  tests: []
  columns:
    your_column_name_a:
      tests: []
    your_column_name_b:
      tests: []
      ...
```

You can add built-in assertions and custom assertions against tables/column here.

e.g.

```yaml
PRICE:  # Table Name
  # Test Cases for Table
  tests: # assertion functions
    - name: assert_row_count_in_range # built-in assertion function takes a parameter
      assert:
        count: [0, 157881]
  columns:
    SYMBOL:  # Column Name
      # Test Cases for Column
      tests: # assertion functions
      - name: alphanumeric_only # custom assertion function without parameters
      - name: distinct_count # custom assertion function takes a parameter
        assert:
          count: 505
```

### Scaffolding of Assertion Function

This is the context of `customized_assertions.py`. A custom assertion class has to implement `BaseAssertionType` \_\_ and its functions, `name()`, _`execute()` and `validate()`._

<details>

<summary><em>Assertion function scaffolding</em></summary>

```python
from piperider_cli.assertion_engine.assertion import AssertionContext, AssertionResult, ValidationResult
from piperider_cli.assertion_engine.types import BaseAssertionType, register_assertion_function


class AssertNothingTableExample(BaseAssertionType):
    def name(self):
        return 'assert_nothing_table_example'

    def execute(self, context: AssertionContext, table: str, column: str, metrics: dict) -> AssertionResult:
        table_metrics = metrics.get('tables', {}).get(table)
        if table_metrics is None:
            # cannot find the table in the metrics
            return context.result.fail()

        # 1. Get the metric for the current table
        # We support two metrics for table level metrics: ['row_count', 'col_count']
        row_count = table_metrics.get('row_count')
        # col_count = table_metrics.get('col_count')

        # 2. Get expectation from assert input
        expected = context.asserts.get('something', [])

        # 3. Implement your logic to check requirement between expectation and actual value in the metrics

        # 4. send result

        # 4.1 mark it as failed result
        # return context.result.fail('what I saw in the metric')

        # 4.2 mark it as success result
        # return context.result.success('what I saw in the metric')

        return context.result.success('what I saw in the metric')

    def validate(self, context: AssertionContext) -> ValidationResult:
        result = ValidationResult(context)
        # result.errors.append('explain to users why this broken')
        return result


class AssertNothingColumnExample(BaseAssertionType):
    def name(self):
        return "assert_nothing_column_example"

    def execute(self, context: AssertionContext, table: str, column: str, metrics: dict) -> AssertionResult:
        column_metrics = metrics.get('tables', {}).get(table, {}).get('columns', {}).get(column)
        if column_metrics is None:
            # cannot find the column in the metrics
            return context.result.fail()

        # 1. Get the metric for the column metrics
        total = column_metrics.get('total')
        non_nulls = column_metrics.get('non_nulls')

        # 2. Get expectation from assert input
        expected = context.asserts.get('something', [])

        # 3. Implement your logic to check requirement between expectation and actual value in the metrics

        # 4. send result

        # 4.1 mark it as failed result
        # return context.result.fail('what I saw in the metric')

        # 4.2 mark it as success result
        # return context.result.success('what I saw in the metric')

        return context.result.success('what I saw in the metric')

    def validate(self, context: AssertionContext) -> ValidationResult:
        result = ValidationResult(context)
        # result.errors.append('explain to users why this broken')
        return result

# register new assertions
register_assertion_function(AssertNothingTableExample)
register_assertion_function(AssertNothingColumnExample)
```

**Methods**

`name()`: return the name of the testing function that will be used in assertion yaml.

`execute()` : the implementation of the testing logic. PipeRider will bring arguments below to `execute` method for you:

* _context_: helper object for assembling result entry to the report.
* _table_: the table name you are asserting.
* _column_: the column name you are checking, but it could be null when the assertion is run against a table.
* _metrics_: the profiling results could be referred for the assertion.

`validate()` : the validation of user inputs to the assertion function, i.e., it validates if the parameter values taken by the function are valid.

`register_assertion_function()`: register the custom assertion function so that it can be recognized in the assertion yaml.

**Code snippets**

Read a _table metrics_ / _column metrics_

\# get a dict object of a table metricstable\_metrics = metrics.get('tables', {}).get(table)if table\_metrics is None: # cannot find the table in the metrics return context.result.fail() # get a value of a metric by a key from the dict object of the table metricstable\_metrics.get('key')# get a dict objec of a column metrics of a table column\_metrics = metrics.get('tables', {}).get(table, {}).get('columns', {}).get(column)if column\_metrics is None: # cannot find the column in the metrics return context.result.fail() # get a value of a metric by a key from the dict objec of the column metricscolumn\_metrics.get('key')

Read user input value of a parameter from an assertion yaml.

```python
context.asserts.get('parameter_a', [])
```

Return `fail()` or `success()` with a message

```
context.result.fail("message")
context.result.success("message")
```

One thing is not mentioned in the context.

The value of the _actual_ will be printed out in the format `Actual: value` when assertion is executed.

```
context.result.actual
```

Assign `actual` any value as the actual finding that why the assertion successes or fails.

**Hands-On**

So far you already have the fundamental knowledge of the assertion, start creating your first custom assertion.

Check your `./piperider/outputs/<run>/.profiler.json` and select a metric as the measurement. In my case, I choose `distinct`.

In my case, there is a table called _SYMBOL_, one of its columns is _Name_. Because I know Name must be contained in a list of names which has 600 names in the total, the metric value of _distinct_ should not exceed 600 otherwise something wrong in my data.

```yaml
 "SYMBOL": {
            "name": "SYMBOL",
            "row_count": 505,
            "col_count": 11,
            "columns": {
                "NAME": {
                    "total": 505,
                    "non_nulls": 505,
                    "distinct": 505,
```

Therefore, I want an assertion function called _assert\_distinct\_in\_range which takes two parameters, min and max, furthermore, it is declared in range\_check.py,_ in future I'll create more range-related assertions.

At `.piperider/assertions/` I create an assertion file, _my\_assertion.yml_ and edit the file to add my custom assertion logic against the column _Name_.

```yaml
SYMBOL:  # Table Name
  # Test Cases for Table
  tests:
  columns:
    NAME: # Column Name
      # Test Cases for Column
      tests:
        - name: assert_distinct_in_range
          assert:
            range: [0, 600]
            
```

Next I will define the corresponding assertion function. I go to `.piperider/plugins` and create a python file, _range\_check.py_. Edit the python file and add the custom assertion codes.

```python
from piperider_cli.assertion_engine.assertion import AssertionContext, AssertionResult, ValidationResult
from piperider_cli.assertion_engine.types import BaseAssertionType, register_assertion_function

class AssertDistinctInRange(BaseAssertionType):
    def name(self):
        return "assert_distinct_in_range"

    def execute(self, context: AssertionContext, table: str, column: str, metrics: dict) -> AssertionResult:
        column_metrics = metrics.get('tables', {}).get(table, {}).get('columns', {}).get(column)
        if column_metrics is None:
            # cannot find the column in the metrics
            return context.result.fail()

        context.result.actual = column_metrics.get('distinct')

        # Get user input range from the assertion
        expected = context.asserts.get('range', [])
        min = expected[0]
        max = expected[1]
    
        # Check if the distinct value sits in the range and return the result
        if context.result.actual <= max and context.result.actual >= min:
            return context.result.success("The value is {} sitting in the range".format(context.result.actual))

        return context.result.fail("The value is {} exceeding the range, something wrong in my data.".format(context.result.actual)) 

    def validate(self, context: AssertionContext) -> ValidationResult:
        expected = context.asserts.get('range')
        result = ValidationResult(context)

        # validate if the value of the parameter "range" is valid
        if expected is None:
            result.errors.append('Range is not defined')
        return result

register_assertion_function(AssertDistinctInRange)
```

I have added the custom assertion and the corresponding assertion function. Time to test it.

```shell
piperider run --table SYMBOL
```

After the running, I see the assertion result. The first custom assertions works!

```shell-session
────────────────────────────────────────────────────────────────── Assertion Results ───────────────────────────────────────────────────────────────────
[  OK  ] SYMBOL.NAME  assert_distinct_in_range  Expected: {'range': [0, 600]} Actual: The value is 505 sitting in the range
```

The custom assertion works, but not perfect. It needs other exception handling to make it more robust and more assertive.

**Debug Mode**

If you need to debug custom assertion functions, just enable the debug mode with `--debug`.

It will print out the trackback of calls.

```
piperider run --debug
```

</details>
