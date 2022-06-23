---
description: Get PipeRider up and running quickly with this sample data quality project.
---

# Quick Start

This quick start guide will show you how to:

* [Install PipeRider](quick-start.md#install-piperider-via-pip)
* [Prepare a SQLite data source](quick-start.md#prepare-sqlite-database)
* [Initialize a new PipeRider project](quick-start.md#initialize-a-new-piperider-project)&#x20;
* [Perform data profiling, quality check and report](quick-start.md#data-profiling)
* [Write data assertions](quick-start.md#data-assertions)
* Next Steps: [Compare reports](quick-start.md#compare-reports)

PipeRider currently supports three data sources - SQLite, Postgres, and Snowflake.  In this guide we will be using SQLite.

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

### Verify project configuration

Use the `diagnose` command to check the project settings and ensure that the data source can be connected to.

```shell
piperider diagnose
```

Sample output:

```
diagnose...
PipeRider Version: 0.2.1
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
  dataproject: [SKIP] provider is not dbt
✅ PASS

Check assertion files:
✅ PASS

🎉 You are all set!
```

### Data profiling & quality check & report

The `run` command will analyze the data source, create one data profile, `run.json` and also generate a HTML report, `index.html`. The data profiles can be used by PipeRider to generate a data quality report in a later step.

If an assertions file is present, then the data will be tested against the specified assertions. If no assertions file is present, you will be prompted to generate assertion templates based on the structure of your data source.

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
No assertions found for datasource [ dataproject ]
Do you want to auto generate assertion templates for this datasource [yes/no]? y
```

Recommended assertions YAML file for each table will be created under `.piperider/assertions/`. Then you will be prompted for the execution of these assertions.

```shell
Recommended Assertion: /path/to/dataproject/.piperider/assertions/recommended_ACTION.yml
Recommended Assertion: /path/to/dataproject/.piperider/assertions/recommended_PRICE.yml
Recommended Assertion: /path/to/dataproject/.piperider/assertions/recommended_SYMBOL.yml
Do you want to run above recommanded assertions for this datasource [yes/no]? yes
```

PipeRider will execute these assertions for the data quality check.

Sample output

```shell-session
───────────────────────────────────────────────────────────────────────────────────── Assertion Results ──────────────────────────────────────────────────────────────────────────────────────
[  OK  ] SYMBOL                     assert_row_count_in_range   Expected: {'count': [454, 555]} Actual: 505
[  OK  ] SYMBOL.SYMBOL              assert_column_type          Expected: {'type': 'string'} Actual: string
[  OK  ] SYMBOL.NAME                assert_column_type          Expected: {'type': 'string'} Actual: string
[  OK  ] SYMBOL.START_DATE          assert_column_type          Expected: {'type': 'datetime'} Actual: datetime
[  OK  ] SYMBOL.END_DATE            assert_column_type          Expected: {'type': 'datetime'} Actual: datetime
[  OK  ] SYMBOL.DESCRIPTION         assert_column_type          Expected: {'type': 'string'} Actual: string
[  OK  ] SYMBOL.EXCHANGE_CODE       assert_column_type          Expected: {'type': 'string'} Actual: string
[  OK  ] SYMBOL.MARKET              assert_column_type          Expected: {'type': 'string'} Actual: string
...
...
...
────────────────────────────────────────────────────────────────────────────────────────── Summary ───────────────────────────────────────────────────────────────────────────────────────────
Table 'ACTION'
  4 columns profiled
  7 test executed
  1 of 7 tests failed:
  [FAILED] ACTION.SPLITS  assert_column_min_in_range  Expected: {'min': [0, 0]} Actual: {'min': 0.13}

Table 'PRICE'
  11 columns profiled
  21 test executed
  1 of 21 tests failed:
  [FAILED] PRICE.MA60  assert_column_min_in_range  Expected: {'min': [4, 5]} Actual: {'min': 5.44}

Table 'SYMBOL'
  11 columns profiled
  12 test executed
  
Generating reports from: /path/to/dataproject/.piperider/outputs/latest/run.json
Report generated in /path/to/dataproject/.piperider/outputs/latest/index.html
```

From the output you can see the results in the following format:

| Result | Table\[.Column] | Used Assertion                 | Justification                                     |
| ------ | --------------- | ------------------------------ | ------------------------------------------------- |
| OK     | SYMBOL          | assert\_row\_count\_in\_range  | `Expected: {'count': [454, 555]} Actual: 505`     |
| FAILED | ACTION.SPLITS   | assert\_column\_min\_in\_range | `Expected: {'min': [0, 0]} Actual: {'min': 0.13}` |
| OK     | PRICE.SYMBOL    | assert\_column\_type           | `Expected: {'type': 'string'} Actual: string`     |

Furthermore, you will see the profiling and assertions results, and with a generated `run.json` and a generated HTML report, `index.html`.

Open the HTML report in the browser to see the visualized results or share it with your team.

{% hint style="info" %}
Refer to [How-To: Generate Report](how-to/generate-report.md) for other methods generate reports
{% endhint %}

### Data assertions

In your text editor, open `.piperider/assertions/recommended_PRICE.yml` . The auto-generated assertions file will look like this:

```yaml
# Auto-generated by Piperider based on table "PRICE"
PRICE:  # Table Name
  # Test Cases for Table
  tests:
  - name: assert_row_count_in_range
    assert:
      count:
      - 160271
      - 195886
    tags:
    - PIPERIDER_RECOMMENDED_ASSERTION
  columns:
    SYMBOL:  # Column Name
      # Test Cases for Column
      tests:
      - name: assert_column_type
        assert:
          type: string
        tags:
        - PIPERIDER_RECOMMENDED_ASSERTION
    DATE: # Column Name
      # Test Cases for Column
      tests:
      - name: assert_column_type
        assert:
          type: datetime
        tags:
        - PIPERIDER_RECOMMENDED_ASSERTION
    OPEN: # Column Name
      # Test Cases for Column
      tests:
      - name: assert_column_type
        assert:
          type: numeric
        tags:
        - PIPERIDER_RECOMMENDED_ASSERTION
      - name: assert_column_min_in_range
        assert:
          min:
          - 6
          - 7
        tags:
        - PIPERIDER_RECOMMENDED_ASSERTION
...
...
...
```

You can modify these auto-generated assertions logics or add your custom assertions. Then execute `piperider run` again.

{% hint style="info" %}
Refer to [Built-In Assertions](data-quality-assertions/assertion-configuration.md) and [Custom Assertions](data-quality-assertions/custom-assertions.md) for detailed assertion settings
{% endhint %}

## Next Steps

Once you have generated multiple reports it may be desirable to compare reports to view how your data has changed over time.

### Compare reports

`compare-report` allows you to compare two different reports by generating a comparison report.

For the sake of data changed simulation, you could download another sqlite database to overwrite the existing one.

```
curl -o sp500.db https://piperider-data.s3.ap-northeast-1.amazonaws.com/getting-started/sp500_20220527.db
```

Execute `piperider run`  to generate the profiling results with a report against the changed database.

Then run the command.

```
piperider compare-report
```

You will be prompted to select two reports for the comparison. Please select reports with same table name but with different timestamps.

```
#Output
[?] Please select the 2 reports to compare ( SPACE to select, and ENTER to confirm ):
   X dataproject  #table=3      #pass=0     #fail=1     2022-06-21T23:15:20.986503Z
 > o dataproject  #table=3      #pass=0     #fail=1     2022-06-21T23:01:50.499774Z
   X dataproject  #table=3      #pass=0     #fail=0     2022-06-21T22:58:23.500316Z
```

PipeRider will generate an HTML report of the comparison.

```
Selected reports:
  Base:  /path/to/dataproject/.piperider/outputs/dataproject-20220621231520/run.json
  Input: /path/to/dataproject/.piperider/outputs/dataproject-20220621225823/run.json

Comparison report: /path/to/dataproject/.piperider/comparisons/20220621231618/index.html
```

Open the HTML report in your browser to review any changes or share it with your team.
