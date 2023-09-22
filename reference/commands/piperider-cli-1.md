---
description: Initialize a new project
---

# init (deprecated)

{% hint style="info" %}
From PipeRider 0.25.0 it is not longer necessary to initialize a new project.&#x20;
{% endhint %}

`init` will lead you through configuring a new data project according to the type of the data source selected. Refer to [Supported Data Sources](../supported-data-sources/) for a list of available connectors.

Initializing a project will generate a `.piperider` directory containing project configuration files such as `.piperider/config.yml` , `.piperider/credentials.yml` and others according to user inputs under your project directory.

<table><thead><tr><th width="243.33333333333334">Option</th><th width="130">Argument</th><th>Description</th></tr></thead><tbody><tr><td><code>--dbt-project-dir</code></td><td>Path</td><td>Specify the path to the directory of a <em>dbt project</em> configuration</td></tr><tr><td><code>--dbt-profiles-dir</code></td><td>Path</td><td>Specify the path to the directory of a <em>dbt profiles</em> configuration</td></tr><tr><td><code>--debug</code></td><td>N/A</td><td>Enable debug mode</td></tr><tr><td><code>--help</code></td><td>N/A</td><td>List available commands</td></tr></tbody></table>

##
