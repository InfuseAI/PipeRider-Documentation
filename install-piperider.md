---
description: How to install PipeRider.
---

# Installation

### Requirements

{% hint style="info" %}
Python 3.7+ Installed
{% endhint %}

### Install or update PipeRider

Use pip to install or update PipeRider.

```shell
pip install -U piperider
```

PipeRider includes the **SQLite** connector by default. The following conectors are also available:

* **BigQuery**
* **Postgres**
* **Redshift**
* **Snowflake**

Use the following command to install PipeRider with your required connector:

```
pip install -U 'piperider[required_connector]'
```

Multiple connectors can also be installed, E.g.

```
pip install -U 'piperider[postgres,snowflake]'
```

{% hint style="info" %}
Don't forget the single quotes when installing PipeRider with a data source connector
{% endhint %}

We are also developing more connectors, please visit [supported data sources](data-sources/supported-data-sources/) and you are welcome to let us know if your desired connector is not listed.

### View installed PipeRider version

```shell
piperider version
```

### List all available commands

```shell
piperider --help
```

Check the PipeRider [Command Reference](piperider-cli.md) for a full list of commands and options.