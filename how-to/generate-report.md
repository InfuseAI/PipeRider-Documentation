---
description: How to generate reports
---

# Generate Report

A generated report refers to a profiling result in a `.json` which is an artifact by `piperider run`.

So `piperider generate-report` will generate reports based on the results of the latest run by default.

There are also a few ways to generate reports on demand.

### Run and generate reports

You can execute `piperider run` with `--generate-report`. It will generate reports after the profiling immediately. It is a short version of `piperider run && piperider generate-report` .

```
piperider run --generate-report
```

Furthermore, you can run and generate a report of a single specified table.

E.g. Profile the table, `price`, and generate the report

```shell
piperider run --table price --generate-report
```

### Generate a report of a specified profile

Every profiling result of a table will be saved at `.piperider/outputs/<each_run>`, you can generate a report of a specified profile.

E.g. Generate a report of `.piperider/outputs/dataproject-20220610154629/price.json`.

```
piperider generate-report --input .piperider/outputs/dataproject-20220610154629/price.json
```
