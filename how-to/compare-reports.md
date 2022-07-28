---
description: How to compare the results of previous reports.
---

# Compare Reports

Once you have generated multiple reports it may be desirable to compare reports to view how your data has changed over time.

The `compare-reports` feature allows you to compare two different reports by generating a comparison report.

```
piperider compare-reports
```

You will be prompted to select two reports for the comparison.

```
#Output
[?] Please select the 2 reports to compare ( SPACE to select, and ENTER to confirm ):
   X dataproject  #table=3      #pass=0     #fail=1     2022-06-21T23:15:20.986503Z
 > o dataproject  #table=3      #pass=0     #fail=1     2022-06-21T23:01:50.499774Z
   X dataproject  #table=3      #pass=0     #fail=0     2022-06-21T22:58:23.500316Z
```

The first report you select is defined as the `base` report, and the second report you select is the `target` that will be compared against the `base`.

### Compare the last two report

Use the `--last` option to compare the last two reports, without the need to manually select them.

```
piperider compare-reports --last
```

### Manually specify base and target reports

It is also possible to compare reports by manually specifying the `base` and `target` profiling json file.

#### --base

Use `--base` along with the location of the `run.json` file to specify the first report to use for the comparison:

```
piperider compare-reports --base /path/to/dataproject/.piperider/outputs/dataproject-20220609212919/run.json
```

You will then be prompted to select the second report to compare against:

```
#Output
[?] Please select a report to compare ( SPACE to select, and ENTER to confirm ):
 > X dataproject  #table=1      #pass=0     #fail=0     2022-06-21T23:34:27.519491Z
   o dataproject  #table=3      #pass=0     #fail=1     2022-06-21T23:23:40.859313Z
   o dataproject  #table=3      #pass=0     #fail=1     2022-06-21T23:01:50.499774Z
   o dataproject  #table=3      #pass=0     #fail=0     2022-06-21T22:58:23.500316Z
```

#### --base & --target

Specify both `base` and `target` reports to use for the comparison:

```shell
piperider compare-reports --base /path/to/<run>/run.json --input /path/to/<run>/run.json
```

PipeRider will generate an HTML report of the comparison.

```
Selected reports:
  Base:  /path/to/dataproject/.piperider/outputs/dataproject-20220609212919/run.json
  Input: /path/to/dataproject/.piperider/outputs/dataproject-20220609211729/run.json

Comparison report: /path/to/dataproject/.piperider/comparisons/dataproject-PRICE-20220609214450/index.html
```

### Specify output location for report

To specify the output location for the generated report use the `-o` or `--output` option.

Generate a comparison report and store it in `~/piperider/reports`:

```
piperider compare-reports -o ~/piperider/reports
```
