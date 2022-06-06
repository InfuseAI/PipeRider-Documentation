---
description: PipeRider CLI installation
---

# Installation

### Requirements

{% hint style="info" %}
Python 3.8+
{% endhint %}

### Install or update the CLI

Piperider-cli installation comes with the sqlite connector by default.

```shell
pip install -U piperider-cli
```

Also, it supports optional connectors, _Postgres_ and _Snowflake_.

Install Piperider-cli with optional connectors.

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
