---
description: Using the PipeRider config.yml file
---

# Config.yml

`config.yml` is the main PipeRider project configuration file, and contains data sources and related profiling settings.

An example `config.yml` file for a Postgres project:

<pre class="language-yaml" data-title="config.yml"><code class="lang-yaml">dataSources:
<strong>- name: salesData
</strong>  type: postgres

profiler:
#   table:
#     # the maximum row count to profile. (Default unlimited)
#     limit: 1000000
#     duplicateRows: false

# The tables to include/exclude
# includes: []
# excludes: []
# Include views or not
# include_views: true

# tables:
#   my-table-name:
#     # description of the table
#     description: "this is a table description"
#     columns:
#       my-col-name:
#         # description of the column
#         description: "this is a column description"

telemetry:
  id: ABC123
</code></pre>

The `config.yml` file is created when a new project is initialized, and stores the following information for your PipeRider project.

### Data source settings

* The name of your data source
* The type of data source e.g. sqlite, postgres etc.
* (sqlite) The path to the database file
* (dbt) the dbt project information (profile, target, project path)
* Telemetry ID for anonymized tracking

If the data source requires credentials they will be stored separately in `credentials.yml`.

For SQLIte projects, it is also possible to store the database path in `credentials.yml` if desired.

### Profiler settings

The following settings enable you to configure the behavior of the profiler. Uncomment and adjust the settings as required.

#### Profile a sample of rows

Set the maximum number of rows to profile per datasource table. In the following example, a maximum number of 10,000 rows will be profiled:

{% code title="config.yml" %}
```yaml
...
profiler:
  table:
    # the maximum row count to profile. (Default unlimited)
    limit: 10000
...
```
{% endcode %}

#### Profile duplicate rows

Enabling it to let the profiler find the duplicate rows from the table. _It is disabled by default_ due to it could be a time-consuming process according to datasets.

{% code title="config.yml" %}
```yaml
...
profiler:
  table:
    # the maximum row count to profile. (Default unlimited)
    duplicateRows: false
...
```
{% endcode %}

#### Include/exclude tables from profiling

By default, PipeRider will profile all existing tables. To specifically include or exclude tables from being profiled, add or remove tables from the `includes` and `excludes` arrays.

PipeRider will profile tables specified in `includes` and ignore tables specified in `excludes`.

```
...
# The tables to include/exclude
includes: [sales, customers]
excludes: [stg_raw_sales]
...
```

{% hint style="info" %}
An empty array means no tables are specified. To profile all tables, leave these options commented.
{% endhint %}

#### Profile views

By default, profiling views is not enabled. To allow PipeRider to profile views, uncomment the following line:

```
include_views: true
```

### Table settings

#### Add table descriptions

Add table and column descriptions which will be shown on your PipeRider report.

{% code title="config.yml" %}
```yaml
...
tables:
  sales:
    # description of the table
    description: "Yearly sales figures by platform"
    columns:
      NA_Sales:
        # description of the column
        description: "North American sales figures"
      EU_Sales:
        # description of the column
        description: "EU sales figures"
      Global_sales:
        # description of the column
        description: "Global sales figures"
...
```
{% endcode %}

### **Telemetry**

This is the anonymous project id that was created during project initialization.

{% code title="config.yml" %}
```yaml
...
telemetry:
  id: abc123
```
{% endcode %}
