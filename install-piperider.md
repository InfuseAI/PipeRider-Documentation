---
description: PipeRider CLI installation
---

# Installation

### Requirements

{% hint style="info" %}
Python 3.8+ Installed
{% endhint %}

### Install or update the CLI

PipeRider CLI installation comes with the _**sqlite**_ connector by default.

```shell
pip install -U piperider
```

Also, there are supported optional connectors, _**Postgres**_ and _**Snowflake**_.

Install Piperider CLI with specified connectors.

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

### Verify the version

```shell
piperider version
```

### List all of commands

```shell
piperider --help
```
