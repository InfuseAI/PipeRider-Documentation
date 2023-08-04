---
description: Install PipeRider with the Databricks connector
---

# Databricks

## Installation

Install PipeRider with the Databricks connector.

```
pip install 'piperider[databricks]'
```

## Configuration (dbt)

Verify PipeRider can connect to Databricks by running the  `diagnose` command from inside your dbt project.

```
piperider diagnose
```
