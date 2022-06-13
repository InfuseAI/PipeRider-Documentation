---
description: How to use PipeRider with dbt
---

# dbt integration

### dbt Integration

[dbt](https://www.getdbt.com/) is a great tool for the data transformation, and yes, Piperider supports dbt projects. Extra manual configuration is not required.

Piperider will search and load the `dbt_project.yml` file into `./piperider/config.yml` by default when `piperider-cli init` .&#x20;
