# Quick Start

Piperider CLI supports three types of data sources, _Postgres_, _Snowflake_ and _Sqlite_, here we use Sqlite for the quick-start.

### Before you start

* Python 3.8+ installed

### Install PipeRider CLI

```shell
pip install piperider
```

### Prepare Sqlite example database

You can use own Sqlite database or download the example database, `sp500.db`.

```shell
mkdir mydataproj && cd mydataproj
curl -o sp500.db https://piperider-data.s3.ap-northeast-1.amazonaws.com/getting-started/sp500_20220401.db
```

### Initiate Data Project

`init` will create a `./piperider` directory and generate a few configuration files inside such as `.piperider/config.yml` , `.piperider/credentials.yml` and others according to user inputs.

```shell
piperider init
```

Prompt for a project name, please type `mydataproj`.

```shell
What is your project name? (alphanumeric only)
```

Prompt for a data source, please select `sqlite` by ⬆⬇.

```
What data source would you like to connect to?
1. snowflake
2. postgres
3. sqlite
```

Prompt for the path to the Sqlite database, please type `sp500.db` or yours.

```
Please enter the following fields for sqlite
Path of database file:
```

### Diagnose Project Configuration

```shell
piperider debug
```

```
code#Output
Debugging...
PipeRider Version: 0.1.3.12
Check config files:
  Path/to/mydataproj/.piperider/config.yml: [OK]
✅ PASS

Check format of data sources:
  mydataproj: [OK]
✅ PASS

Check connections:
  Name: mydataproj
  Type: sqlite
  Available Tables: ['ACTION', 'PRICE', 'SYMBOL']
  Connection: [OK]
✅ PASS

Check dbt catalog files:
  mydataproj: [SKIP] provider is not dbt
✅ PASS

Check assertion files:
✅ PASS

🎉 You are all set!
```

### Profile and check data quality&#x20;

`run` will profile the data project, check data quality by assertions if any and generate outputs in _`.json`_ for each table in each run under `.piperider/outputs/`. In the example database, there are three tables, _ACTION_, _PRICE_ and _SYMBOL_.

{% hint style="info" %}
There are no assertions by default.
{% endhint %}

```
piperider run
```

```shell
#Ouput                                                                                                                    
DataSource: mydataproj
─────────────────────────────────────────────────────────────────────────────────────── Profiling ────────────────────────────────────────────────────────────────────────────────────────
fetching metadata
profiling [ACTION.SYMBOL] type=VARCHAR(16777216)
profiling [ACTION.DATE] type=DATE
profiling [ACTION.DIVIDENDS] type=NUMERIC(8, 4)
profiling [ACTION.SPLITS] type=NUMERIC(10, 2)
profiling [PRICE.SYMBOL] type=VARCHAR(16777216)
profiling [PRICE.DATE] type=DATE
profiling [PRICE.OPEN] type=NUMERIC(10, 2)
profiling [PRICE.HIGH] type=NUMERIC(10, 2)
profiling [PRICE.LOW] type=NUMERIC(10, 2)
profiling [PRICE.CLOSE] type=NUMERIC(10, 2)
profiling [PRICE.VOLUME] type=NUMERIC(38, 0)
profiling [PRICE.ADJCLOSE] type=NUMERIC(10, 2)
profiling [PRICE.MA5] type=NUMERIC(10, 2)
profiling [PRICE.MA20] type=NUMERIC(10, 2)
profiling [PRICE.MA60] type=NUMERIC(10, 2)
profiling [SYMBOL.SYMBOL] type=VARCHAR(16777216)
profiling [SYMBOL.NAME] type=VARCHAR(16777216)
profiling [SYMBOL.START_DATE] type=DATE
profiling [SYMBOL.END_DATE] type=DATE
profiling [SYMBOL.DESCRIPTION] type=VARCHAR(16777216)
profiling [SYMBOL.EXCHANGE_CODE] type=VARCHAR(16777216)
profiling [SYMBOL.MARKET] type=VARCHAR(16777216)
profiling [SYMBOL.COUNTRY] type=VARCHAR(16777216)
profiling [SYMBOL.SECTOR] type=VARCHAR(16777216)
profiling [SYMBOL.INDUSTRY] type=VARCHAR(16777216)
profiling [SYMBOL.RECOMMENDATION_KEY] type=VARCHAR(16777216)

```

Prompt for assertion templates generation, please type `y` or `yes`.

```bash
No assertions found for datasource [ mydataproj ]
Do you want to auto generate assertion templates for this datasource [yes/no]? y
```

It will generate assertion scaffoldings in `.yml` for each table under `.piperider/assertions/`.&#x20;

{% hint style="info" %}
Regarding how to configure assertions, please check the [assertion configuration](assertion-configuration.md) for the detail.
{% endhint %}

```shell
Generating assertion template for table "ACTION" -> /path/to/mydataproj/.piperider/assertions/ACTION.yml
Generating assertion template for table "PRICE" -> /path/to/mydataproj/.piperider/assertions/PRICE.yml
Generating assertion template for table "SYMBOL" -> /path/to/mydataproj/.piperider/assertions/SYMBOL.yml
[Skip] Executing assertion for datasource [ mydataproj ]
──────────────────────────────────────────────────────────────────────────────────────── Summary ─────────────────────────────────────────────────────────────────────────────────────────
Table 'ACTION'
  4 columns profiled

Table 'PRICE'
  11 columns profiled

Table 'SYMBOL'
  11 columns profiled
```

### Generate a report

`generate-report` will generate static HTML reports for each table by the profiling result of the latest `run`.

```
piperider generate-report
```

```shell
#Output
Generating reports from: /path/to/mydataproj/.piperider/outputs/latest
──────────────────────────────────────────────────────────────────────────────────────── Reports
Table 'ACTION' /path/to/mydataproj/.piperider/reports/mydataproj-20220607111429/ACTION.html
Table 'SYMBOL' /path/to/mydataproj/.piperider/reports/mydataproj-20220607111429/SYMBOL.html
Table 'PRICE'  /path/to/mydataproj/.piperider/reports/mydataproj-20220607111429/PRICE.html
```

Open the HTML report in the browser to see the profiling and testing results.
