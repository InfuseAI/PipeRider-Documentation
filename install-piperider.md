---
description: How to install PipeRider
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

PipeRider includes the _**SQLite**_ connector by default. Optional _**Postgres**_ and _**Snowflake** connectors_ are also available.

Install Piperider with required connectors:

{% tabs %}
{% tab title="Postgres" %}
```
pip install piperider[postgres]
```
{% endtab %}

{% tab title="Snowflake" %}
```
pip install piperider[snowflake]
```
{% endtab %}

{% tab title="Multiple" %}
```
pip install piperider[postgres,snowflake]
```
{% endtab %}
{% endtabs %}

### View installed PipeRider version

```shell
piperider version
```

### List all available commands

```shell
piperider --help
```

Check the PipeRider [Command Reference](piperider-cli.md) for a full list of commands and options.
