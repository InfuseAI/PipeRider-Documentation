---
description: Get PipeRider up and running quickly with this sample data quality project.
---

# Quick Start

This quick start guide will show you how to:

* Install PipeRider
* Initialize a new PipeRider project with SQLite data source
* Write basic assertions
* Test your data
* Generate a data quality report

PipeRider currently supports three data sources - SQLite, _Postgres_, and _Snowflake_.  In this quick start guide we will be using SQLite.

### Before you start

Ensure you have the following installed:

* Python 3.7+

### Install PipeRider via pip

PipeRider can be installed via pip with the following command.

```shell
pip install piperider
```

### Prepare SQLite database

Create a directory for the new project then either place your own SQLite database inside or download the sample database below.

```shell
mkdir dataproject && cd dataproject
curl -o sp500.db https://piperider-data.s3.ap-northeast-1.amazonaws.com/getting-started/sp500_20220401.db
```

### Initialize a new PipeRider project

The `init` command creates a `.piperider` directory under the current directory where all of the project files will be stored. This includes data source configuration, data quality assertions, data profiling information, and generated report files.

```shell
piperider init
```

You will be prompted to enter a project name, enter `dataproject`.

```shell
What is your project name? (alphanumeric only)
```

Using the arrow keys, select `sqlite` as the desired data source.

```
What data source would you like to connect to?
  snowflake
  postgres
> sqlite
```

When prompted, enter the name of your SQLite database. If you downloaded the sample database from above enter `sp500.db` .

```
Please enter the following fields for sqlite
Path of database file:
```

### Verify Project Configuration

Use the `debug` command to check the project settings and ensure that the data source can be connected to.

```shell
piperider debug
```

Sample output:

```
Debugging...
PipeRider Version: 0.1.3.12
Check config files:
  /path/to/dataproject/.piperider/config.yml: [OK]
✅ PASS

Check format of data sources:
  dataproject: [OK]
✅ PASS

Check connections:
  Name: dataproject 
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

### Data Profiling

The `run` command will analyze the data source and create a data profile for each table. The data profiles will be used by PipeRider to generate a data quality report in a later step.

If an assertions file is present, then the data will be tested against the specified assertions. If no assertions file is present, you will be prompted to generated assertion templates based on the structure of your data source.

```
piperider run                                                                                                                 
```

Sample output:

```shell
DataSource: dataproject
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

Enter `y` or `yes` to generate an assertion templates for your datasource.

```bash
No assertions found for datasource [ mydataproj ]
Do you want to auto generate assertion templates for this datasource [yes/no]? y
```

An assertion template YAML file for each table will be created under `.piperider/assertions/`.&#x20;

```shell
Generating assertion template for table "ACTION" -> /path/to/dataproject/.piperider/assertions/ACTION.yml
Generating assertion template for table "PRICE" -> /path/to/dataproject/.piperider/assertions/PRICE.yml
Generating assertion template for table "SYMBOL" -> /path/to/dataprojectdataproject/.piperider/assertions/SYMBOL.yml
[Skip] Executing assertion for datasource [ mydataproj ]
──────────────────────────────────────────────────────────────────────────────────────── Summary ─────────────────────────────────────────────────────────────────────────────────────────
Table 'ACTION'
  4 columns profiled

Table 'PRICE'
  11 columns profiled

Table 'SYMBOL'
  11 columns profiled
```

### Write basic assertions

In your text editor, open `.piperider/assertions/PRICE.yml` . The auto-generated assertions file will look like this:

```yaml
# Auto-generated by Piperider CLI based on table "PRICE"
PRICE:  # Table Name
  # Test Cases for Table
  tests: []
  columns:
    SYMBOL:  # Column Name
      # Test Cases for Column
      tests: []
    DATE: # Column Name
      # Test Cases for Column
      tests: []
    OPEN: # Column Name
      # Test Cases for Column
      tests: []
    HIGH: # Column Name
      # Test Cases for Column
      tests: []
    LOW: # Column Name
      # Test Cases for Column
      tests: []
    CLOSE: # Column Name
      # Test Cases for Column
      tests: []
...
```

Edit the `tests` section for the SYMBOL column like so:

```yaml
    SYMBOL:  # Column Name
      # Test Cases for Column
      tests:
      - name: assert_column_in_types
        assert:
          type: [numeric]
```

Edit the `tests` section for the `OPEN` column like so:

```yaml
    OPEN: # Column Name
      # Test Cases for Column
      tests:
      - name: assert_column_min_in_range
        assert:
          min: [0, 50]
```

{% hint style="info" %}
Please check [assertion configuration](assertion-configuration.md) for detailed assertion settings
{% endhint %}

### Check data quality

Now that you have created some basic assertions, execute the `run` command again to check the quality of your data against the newly updated assertion file.

```
piperider run
```

PipeRider will automatically detect the assertions and check the data. The results of the assertion checks will be displayed.

```shell
───────────────────────────────────────────────── Assertion Results ──────────────────────────────────────────────────
[FAILED] PRICE.SYMBOL  assert_column_in_types      Expected: {'type': ['numeric']} Actual: string
[  OK  ] PRICE.OPEN    assert_column_min_in_range  Expected: {'min': [0, 50]} Actual: {'min': 6.78}
────────────────────────────────────────────────────── Summary ───────────────────────────────────────────────────────
Table 'ACTION'
  4 columns profiled

Table 'PRICE'
  11 columns profiled
  2 test executed
  1 of 2 tests failed:
[FAILED] PRICE.SYMBOL  assert_column_in_types  Expected: {'type': ['numeric']} Actual: string

Table 'SYMBOL'
  11 columns profiled
```

From the results you can see that the `assert_column_in_types` assertion against the `SYMBOL` column failed as the column type is STRING, and not NUMERIC as we specified.

The `assert_column_min_in_range` assertion passed our assertion of `[0, 50]` with an actual result of `6.78`.

### Generate a report

The `generate-report` command will crrate static HTML reports for each table based on the profiling results of the latest `run`.

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
