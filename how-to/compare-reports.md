---
description: How to compare the results of previous reports.
---

# Compare Reports

Once you have generated multiple reports it may be desirable to compare reports to view how your data has changed over time.

The `compare-report` feature allows you to compare two different reports by generating a comparison report.

```
piperider compare-report
```

You will be prompted to select two reports for the comparison.

```
#Output
[?] Please select the 2 reports to compare ( SPACE to select, and ENTER to confirm ):
   X dataproject  #table=3      #pass=0     #fail=1     2022-06-21T23:15:20.986503Z
 > o dataproject  #table=3      #pass=0     #fail=1     2022-06-21T23:01:50.499774Z
   X dataproject  #table=3      #pass=0     #fail=0     2022-06-21T22:58:23.500316Z
```

#### --base

another way is to specify a profiling `.json` as the base by `--base`.

```
piperider compare-report --base /path/to/dataproject/.piperider/outputs/dataproject-20220609212919/run.json
```

Then you will be prompted to select a result as the input for the comparison.

```
#Output
[?] Please select a report to compare ( SPACE to select, and ENTER to confirm ):
 > X dataproject  #table=1      #pass=0     #fail=0     2022-06-21T23:34:27.519491Z
   o dataproject  #table=3      #pass=0     #fail=1     2022-06-21T23:23:40.859313Z
   o dataproject  #table=3      #pass=0     #fail=1     2022-06-21T23:01:50.499774Z
   o dataproject  #table=3      #pass=0     #fail=0     2022-06-21T22:58:23.500316Z
```

#### --base & --input

Specify two `.json` in the command for the comparison, one as `--base`, the other as `--input`.

```shell
piperider compare-report --base /path/to/<.json> --input /path/to/<run>/run.json
```

PipeRider will generate an HTML report of the comparison.

```
Selected reports:
  Base:  /path/to/dataproject/.piperider/outputs/dataproject-20220609212919/run.json
  Input: /path/to/dataproject/.piperider/outputs/dataproject-20220609211729/run.json

Comparison report: /path/to/dataproject/.piperider/comparisons/dataproject-PRICE-20220609214450/index.html
```

Open the HTML report in your browser to review any changes.
