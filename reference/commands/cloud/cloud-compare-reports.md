# cloud compare-reports

```
piperider cloud compare-reports --base <base-report> --target <target-report>
```

`compare-reports` will compare the two specified reports (`base` and `target`) and generate a comparison report in your PipeRider Cloud account.&#x20;

| Option                | Argument                                       | Description                                        |
| --------------------- | ---------------------------------------------- | -------------------------------------------------- |
| `--base` (required)   | A report ID (numeric), or the data source name | The report to use as a base for the comparison     |
| `--target` (required) | A report ID (numeric), or the data source name | The report to use as the target for the comparison |
| `--summary-file`      | Desired filename for this file                 | Output the comparison summary as a Markdown file   |
| `--help`              | N/A                                            | List command-line options                          |
|                       |                                                |                                                    |

#### Examples

Compare reports with the ID `123` and `456` and save the Markdown comparison summary to a file named `summary.md`.

```
piperider cloud compare-reports --base 123 --target 456 --summary-file summary.md 
```

Compare the latest reports from the `jaffle-shop` and `jaffle-shop-dev` data sources  and save the Markdown comparison summary to a file named `summary.md`.

```
piperider cloud compare-reports --base datasource:jaffle-shop --target datasource:jaffle-shop-dev --summary-file summary.md   
```
