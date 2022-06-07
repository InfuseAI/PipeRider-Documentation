---
description: PipeRider CLI installation
---

# Installation

### Requirements

{% hint style="info" %}
Python 3.8+ Installed
{% endhint %}

### Install or update the CLI

Piperider CLI installation comes with the _**sqlite**_ connector by default.

```shell
pip install -U piperider-cli
```

Also, there are supported optional connectors, _**Postgres**_ and _**Snowflake**_.

Install Piperider CLI with specified connectors.

{% tabs %}
{% tab title="Postgres" %}
```
pip install piperider-cli[postgres]
```
{% endtab %}

{% tab title="Snowflake" %}
```
pip install piperider-cli[snowflake]
```
{% endtab %}

{% tab title="Multiple" %}
```
pip install piperider-cli[postgres,snowflake]
```
{% endtab %}
{% endtabs %}

### Verify the version

```shell
piperider-cli version
```

### List all of commands

```shell
piperider-cli --help
```
