# Quick Start (dbt case)

PipeRider supports dbt projects as well, it is super easy and straightforward to init Piperider in a existing dbt project.

Let's create a dbt project bases on a _SQLite_ database as the data source first, then show you how easy to get PipeRider involved in.



### Before you start

Ensure you have the follow installed:

* Python 3.7+
* [dbt-core](https://docs.getdbt.com/dbt-cli/install/pip#install-dbt-core-only)

### dbt Project

#### Prepare SQLite database

```shell-session
curl -o sp500.db https://piperider-data.s3.ap-northeast-1.amazonaws.com/getting-started/sp500_20220401.db
```

#### Install dbt SQLite adapter

```shell-session
pip install dbt-sqlite
```

Due to the [dbt-sqlite](https://docs.getdbt.com/reference/warehouse-profiles/sqlite-profile) requires the _crypto_ library file, please [download](https://github.com/nalgeon/sqlean/blob/main/docs/crypto.md) the appropriate library according to your CPU architecture/OS. In the case, using `crypto.arm64.dylib`.

{% hint style="info" %}
* `*.dll` - for Windows
* `*.so` - for Linux
* `*.dylib` - for macOS Intel
* `*.arm64.dylib - for macOS AppleSilicon`
{% endhint %}

#### Init dbt project

```shell-session
dbt init
```

Prompted for a project name: enter `sp500_index`

```shell-session
Running with dbt=1.1.1
Enter a name for your project (letters, digits, underscore): sp500_index
```

Prompted for the database type: enter `1`

```shell-session
Which database would you like to use?
[1] sqlite

(Don't see the one you want? https://docs.getdbt.com/docs/available-adapters)

Enter a number: 1
```

dbt will generate a template of `~/.dbt/profiles.yml`, let's edit the context like the example below:

{% hint style="warning" %}
Please replace the **path** to where the _sp500.db_ is.
{% endhint %}

{% hint style="info" %}
Please replace the **path** to where _crypto.\*_ library is;  remove the file extension part, ~~_.so, .dylib, .dll_~~ from the path like the example below.
{% endhint %}

```yaml
sp500_index:
  outputs:

    dev:
      type: sqlite
      threads: 1
      database: sp500
      schema: 'main'
      schemas_and_paths:
        main: '/Users/infuseai/data/sp500.db'
      schema_directory: '/Users/infuseai/data/'
      extensions:
        - '/Users/infuseai/file/crypto.arm64'

  target: dev
```

#### test dbt project configuration

Let's testify the dbt configuration

```
cd sp500_index
dbt debug --profile sp500_index
```

If the configuration is correct, the output looks like the context below:

```shell-session
Running with dbt=1.1.1
dbt version: 1.1.1
python version: 3.9.12
python path: /example/path/to/pyenv/bin/python3.9
os info: macOS-12.3-arm64-arm-64bit
Using profiles.yml file at /Users/infuseai/path/to/.dbt/profiles.yml
Using dbt_project.yml file at /Users/infuseai/path/to/sp500_index/dbt_project.yml

Configuration:
  profiles.yml file [OK found and valid]
  dbt_project.yml file [OK found and valid]

Required dependencies:
 - git [OK found]

Connection:
  database: sp500
  schema: main
  schemas_and_paths: {'main': '/Users/infuseai/data/sp500.db'}
  schema_directory: /Users/infuseai/data/
  Connection test: [OK connection ok]

All checks passed!
```

#### Do some data transformation

The sp500.db is a set of records of sp500 stocks price, let's build a model to meet our interests.

Edit the context of `models/example/my_first_dbtmodel.sql` __ at the `sp500_index`/:

```sql
with source_data as (

    SELECT * FROM PRICE
    LEFT JOIN SYMBOL ON SYMBOL.SYMBOL = PRICE.SYMBOL
    WHERE PRICE.DATE = '2022-01-04' 
)

SELECT *
FROM source_data
```

and the second model, `models/example/my_second_dbt_model.sql`, referring to the first model:

```sql
SELECT SYMBOL, DATE, OPEN, HIGH, LOW, CLOSE, VOLUME, ADJCLOSE, MA5, MA20,MA60, NAME, COUNTRY, EXCHANGE_CODE, SECTOR, INDUSTRY
FROM {{ ref('my_first_dbt_model') }}
WHERE COUNTRY = 'United States'
```

#### Do little schema test

Let's tweak the `models/example/schema.yml` to meet our updated models.

```yaml
version: 2

models:
  - name: my_first_dbt_model
    description: "Record Date on 2022-01-04"
    columns:
      - name: SYMBOL 
        description: "Stock symbol"
        tests:
          - unique
          - not_null

  - name: my_second_dbt_model
    description: "Stocks in the U.S."
    columns:
      - name: OPEN
        description: "Opening price"
        tests:
          - not_null
```

#### Build dbt models with tests

_dbt build_ will generate models and run dbt tests.

```shell-session
dbt build
```

Sample Output:

```
14:45:58  Running with dbt=1.1.1
14:45:58  Found 2 models, 3 tests, 0 snapshots, 0 analyses, 173 macros, 0 operations, 0 seed files, 0 sources, 0 exposures, 0 metrics
14:45:58  
14:45:58  Concurrency: 1 threads (target='dev')
14:45:58  
14:45:58  1 of 5 START view model main.my_first_dbt_model ................................ [RUN]
14:45:58  1 of 5 OK created view model main.my_first_dbt_model ........................... [OK in 0.07s]
14:45:58  2 of 5 START test not_null_my_first_dbt_model_SYMBOL ........................... [RUN]
14:45:58  2 of 5 PASS not_null_my_first_dbt_model_SYMBOL ................................. [PASS in 0.07s]
14:45:58  3 of 5 START test unique_my_first_dbt_model_SYMBOL ............................. [RUN]
14:45:58  3 of 5 PASS unique_my_first_dbt_model_SYMBOL ................................... [PASS in 0.04s]
14:45:58  4 of 5 START view model main.my_second_dbt_model ............................... [RUN]
14:45:58  4 of 5 OK created view model main.my_second_dbt_model .......................... [OK in 0.02s]
14:45:58  5 of 5 START test not_null_my_second_dbt_model_OPEN ............................ [RUN]
14:45:58  5 of 5 PASS not_null_my_second_dbt_model_OPEN .................................. [PASS in 0.04s]
14:45:58  
14:45:58  Finished running 2 view models, 3 tests in 0.40s.
14:45:58  
14:45:58  Completed successfully
14:45:58  
14:45:58  Done. PASS=5 WARN=0 ERROR=0 SKIP=0 TOTAL=5
```

It looks all good. :smile:

### PipeRider In

Now we have an existing dbt project, let's see how easily to have PipeRider involved.

#### Install PipeRider

```shell-session
pip install piperider
```

Inside the _sp500\_index_ directory, run the command, PipeRider will load _dbt\_project.yml_ and find the right _dbt profile_ accordingly.

```
piperider init
```

Sample output:

```shell-session
Initialize piperider to path /Users/infuseai/path/to/sp500_index/.piperider
Start to search dbt project ...
Use dbt project file: /Users/infuseai/path/to/sp500_index/dbt_project.yml
─────────────────────────────────────────────────────────────────────────────────────────── .piperider/config.yml ────────────────────────────────────────────────────────────────────────────────────────────
   1 dataSources:
   2 - name: sp500_index
   3   type: sqlite
   4   dbt:
   5     profile: sp500_index
   6     target: dev
   7     projectDir: .
   8 telemetry:
   9   id: 181f0172e54546c99ea971d0a7b913e2

Next step:
  Please execute command 'piperider diagnose' to verify configurationshe
```

#### Verify PipeRider + dbt project

```shell-session
piperider diagnose
```

```
Diagnosing...
PipeRider Version: 0.4.1
Check config files:
  /Users/infuseai/path/to/sp500_index/.piperider/config.yml: [OK]
✅ PASS

Check format of data sources:
  sp500_index: [OK]
✅ PASS

Check connections:
  DBT: sqlite > sp500_index > dev
  Name: sp500_index
  Type: sqlite
  Available Tables: ['ACTION', 'PRICE', 'SYMBOL']
  Connection: [OK]
✅ PASS

Check assertion files:
  /Users/infuseai/path/to/sp500_index/.piperider/assertions/my_second_dbt_model.yml: [OK]
  /Users/infuseai/path/to/sp500_index/.piperider/assertions/my_first_dbt_model.yml: [OK]
✅ PASS

🎉 You are all set!


Next step:
  Please execute command 'piperider run' to generate your second report
```

#### Profiling dbt models, dbt tests and generate reports

PipeRider will profile dbt models, run dbt tests and generate the report according to the result.

```
piperider run --dbt-test
```

Sample output:

```
DataSource: sp500_index
──────────────────────────────────────────────────────────────────────────────────── Validating ─────────────────────────────────────────────────────────────────────────────────────
everything is OK.
──────────────────────────────────────────────────────────────────────────────────── Running dbt ────────────────────────────────────────────────────────────────────────────────────
dbt working dir: .
Execute command: dbt test
00:40:22  Running with dbt=1.1.1
00:40:22  Found 2 models, 3 tests, 0 snapshots, 0 analyses, 173 macros, 0 operations, 0 seed files, 0 sources, 0 exposures, 0 metrics
00:40:22
00:40:22  Concurrency: 1 threads (target='dev')
00:40:22
00:40:22  1 of 3 START test not_null_my_first_dbt_model_SYMBOL ........................... [RUN]
00:40:22  1 of 3 PASS not_null_my_first_dbt_model_SYMBOL ................................. [PASS in 0.08s]
00:40:22  2 of 3 START test not_null_my_second_dbt_model_OPEN ............................ [RUN]
00:40:22  2 of 3 PASS not_null_my_second_dbt_model_OPEN .................................. [PASS in 0.04s]
00:40:22  3 of 3 START test unique_my_first_dbt_model_SYMBOL ............................. [RUN]
00:40:23  3 of 3 PASS unique_my_first_dbt_model_SYMBOL ................................... [PASS in 0.06s]
00:40:23
00:40:23  Finished running 3 tests in 0.30s.
00:40:23
00:40:23  Completed successfully
00:40:23
00:40:23  Done. PASS=3 WARN=0 ERROR=0 SKIP=0 TOTAL=3
───────────────────────────────────────────────────────────────────────────────────── Profiling ─────────────────────────────────────────────────────────────────────────────────────
fetching metadata for table 'my_second_dbt_model'
fetching metadata for table 'my_first_dbt_model'
[1/2] my_second_dbt_model ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 16/16 0:00:00
[2/2] my_first_dbt_model  ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 22/22 0:00:00
```

after the profiling, there is a prompt. Let's type _no_ this time.

```
No assertion found
Do you want to auto generate recommended assertions for this datasource [Yes/no]?
```

After all of it, you should see output saying dbt test results, dbt summary, PipeRider summary and a report generated.

Open the report by the browser to see what profiling result PipeRider found and the testing results.

```shell-session
────────────────────────────────────────────────────────────────────────── Generating Assertion Templates ───────────────────────────────────────────────────────────────────────────
Template Assertion: /Users/gabriel/Workspace/qiita/sp500_index/.piperider/assertions/my_second_dbt_model.yml
Template Assertion: /Users/gabriel/Workspace/qiita/sp500_index/.piperider/assertions/my_first_dbt_model.yml
───────────────────────────────────────────────────────────────────────────────── Assertion Results ─────────────────────────────────────────────────────────────────────────────────
──────────────────────────────────────────────────────────────────────────────────────── dbt ────────────────────────────────────────────────────────────────────────────────────────

  Status     Target                      Test Name                                                        Message
 ─────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  [  OK  ]   my_first_dbt_model.SYMBOL   test.sp500_index.not_null_my_first_dbt_model_SYMBOL.06ca293b81
  [  OK  ]   my_first_dbt_model.SYMBOL   test.sp500_index.unique_my_first_dbt_model_SYMBOL.0463684cd6
  [  OK  ]   my_second_dbt_model.OPEN    test.sp500_index.not_null_my_second_dbt_model_OPEN.fa199efe22

────────────────────────────────────────────────────────────────────────────────────── Summary ──────────────────────────────────────────────────────────────────────────────────────
──────────────────────────────────────────────────────────────────────────────────────── dbt ────────────────────────────────────────────────────────────────────────────────────────

  Table Name            #DBT Tests Executed   #DBT Tests Failed
 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  my_first_dbt_model                      2                   0
  my_second_dbt_model                     3                   0

───────────────────────────────────────────────────────────────────────────────────── PipeRider ─────────────────────────────────────────────────────────────────────────────────────

  Table Name            #Columns Profiled   #Tests Executed   #Tests Failed
 ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
  my_second_dbt_model                  16                 0               0
  my_first_dbt_model                   22                 0               0

Generating reports from: /Users/gabriel/Workspace/qiita/sp500_index/.piperider/outputs/latest/run.json
Report generated in /Users/gabriel/Workspace/qiita/sp500_index/.piperider/outputs/latest/index.html

Next step:
  Please execute command 'piperider run' to generate your second report
```

![Index](.gitbook/assets/sp500\_index.png)

![table](.gitbook/assets/sp500\_index\_profiling.png)

![dbt tests](.gitbook/assets/sp500\_index\_dbt\_test.png)
