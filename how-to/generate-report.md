---
description: How to generate reports
---

# Generate Report

A report, `index.html`, by default is generated with a profiling results, `run.json` by `piperider run`.&#x20;

`piperider generate-report` will just generate reports directly referring to the result of latest run.&#x20;

Besides, there are also a few ways to generate reports on your demand.

### Run and generate report of a single table

You can profile and generate a report of a single specified table.

E.g. Profile the table, `price`, and generate the report

```shell
piperider run --table price
```

### Generate a report of a specific run

Every profiling result will be saved at `.piperider/outputs/<each_run>/run.json`, you may just want to generate a report of a specified profile in the history.

E.g. Generate a report of `.piperider/outputs/dataproject-20220610154629/run.json`.

```
piperider generate-report --input .piperider/outputs/dataproject-20220610154629/run.json
```
