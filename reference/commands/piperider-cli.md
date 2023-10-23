---
description: Generate an Exploratory Data Analysis (EDA) Report
---

# run

```shell
piperider run [options]
```

The `run` command performs the following functions.

* Analyze the data source and generate a data profile.
* (If assertion files exist) Test the data profile against any existing assertions located in `.piperider/assertions`.
* Generate a report in `.piperider/outputs`.

{% hint style="info" %}
Check [assertion configuration](broken-reference) for more information on built-in assertions
{% endhint %}



<table><thead><tr><th width="222">Option</th><th width="173">Argument</th><th>Description</th></tr></thead><tbody><tr><td><code>--datasource</code></td><td>Data source name or dbt target</td><td>Specify the data source to profile:<br>For non-dbt projects, the name of the data source,<br>for dbt projects: The <code>target</code> name</td></tr><tr><td><code>--dbt-profile</code></td><td>Text</td><td>The name of the dbt profile to load. This overrides the setting in <code>dbt_project.yml</code></td></tr><tr><td><code>--dbt-target</code></td><td>Text</td><td>The dbt target to load for the given profile</td></tr><tr><td><code>-o</code>, <code>--output</code></td><td>Path</td><td>Directory to save the results</td></tr><tr><td><code>--open</code></td><td>N/A</td><td>Automatically open the HTML report in your browser</td></tr><tr><td><code>--report-dir</code></td><td>Path</td><td>Specify the path to read and write reports <strong>relative</strong> to the <code>.piperider</code> folder</td></tr><tr><td><code>--skip-report</code></td><td>N/A</td><td>Don't generate any reports. (<code>run.json</code> is still  generated)</td></tr><tr><td><code>--upload</code></td><td>N/A</td><td>Uploads the generated report to PipeRider Cloud (If logged in, see <code>piperider cloud login</code>)</td></tr><tr><td><code>--table</code></td><td>Table name</td><td>Profile a specified table only. <strong>Deprecated</strong> - use <code>--select</code> instead</td></tr><tr><td><code>--select</code></td><td>Model, directory</td><td>Specify dbt resources to profile</td></tr><tr><td><code>--debug</code></td><td>N/A</td><td>Enable debugging output</td></tr><tr><td><code>--help</code></td><td>N/A</td><td>List command-line options</td></tr></tbody></table>

