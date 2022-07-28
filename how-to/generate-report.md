---
description: How to generate reports
---

# Generate Report

PipeRider generates HTML reports that contain the following information:

* Data source profile
* PipeRider assertion test results
* dbt test results (if applicable)

Each `piperider run` creates a folder in `.piperider/outputs` that contains the profiling results and a report (unless skipped).

The following methods are also available to generate reports.

### Run and generate report of a single table

Use the following command to profile and generate a report for a specified table.

Profile the `price` table and generate a report:

```shell
piperider run --table price
```

### Generate a report for a specific run

It is also possible to generate a report for a specific run by specifying the location of the `run.json` file. Profiling results can be found in the output folder for that run, E.g. `.piperider/outputs/<run>/run.json`

Generate a report based on the profiling results stored in the `dataproject-20220610154629` run folder:

```
piperider generate-report --input .piperider/outputs/dataproject-20220610154629/run.json
```

### Specify output location for report

As mentioned above, the default location for reports is the `.piperider/outputs` folder. To specifiy the output location for the generated report use the `-o` or `--output` option.

Generate a report and store it in `~/piperider/reports`:

```
piperider generate-report -o ~/piperider/reports
```
