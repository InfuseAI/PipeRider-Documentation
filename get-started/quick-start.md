---
description: Integrate PipeRider with a dbt project in 5 mins
---

# Quick Start

This guide illustrates the usage of PipeRider with a dbt project by using dbt's [Jaffle Shop](https://github.com/dbt-labs/jaffle\_shop) project as an example.

## Prepare dbt project

Clone _jaffle\_shop_ repository, set up a profile, and make sure that you have installed dbt and its related packages.

```bash
git clone https://github.com/dbt-labs/jaffle_shop.git
cd jaffle_shop
```

To quickly set up a profile, we provide an example using [_DuckDB_](https://duckdb.org/).

Save the following content directly to `profiles.yml` and place it under the _jaffle\_shop_ directory.

```yaml
# ./profiles.yml
jaffle_shop:
  target: dev
  outputs:
    dev:
      type: duckdb
      path: jaffle_shop.duckdb
```

Install [`dbt-duckdb`](https://docs.getdbt.com/reference/warehouse-setups/duckdb-setup).

```bash
pip install dbt-duckdb
```

Run dbt and make sure everything works

```
dbt build
```

## Install PipeRider

Install PipeRider with _DuckDB_ related dependencies.

```bash
pip install 'piperider[duckdb]'
```

{% hint style="info" %}
&#x20;PipeRider requires Python 3.7+
{% endhint %}

## Make a change

Create a new branch.

```bash
git checkout -b feature/add-average-value-per-order
```

Edit `models/customers.sql` and add a new column to _customers_.&#x20;

```diff
...  
  final as (

      select
          customers.customer_id,
          customers.first_name,
          customers.last_name,
          customer_orders.first_order,
          customer_orders.most_recent_order,
+         customer_payments.total_amount / customer_orders.number_of_orders as average_value_per_order,
          customer_orders.number_of_orders,
          customer_payments.total_amount as customer_lifetime_value

      from customers
...
```

Run dbt command again to ensure that the changes have been applied correctly.

```
dbt build
```

## Review the changes by PipeRider

Commit this code change.

```bash
git add models/customers.sql
git commit -m 'Add average_value_per_order to customers'
```

Execute the following command to generate a comparison report for comparing the transformed models

```bash
piperider compare
```

Check out the report link at the end of output.

{% hint style="info" %}
PipeRider focuses on analyzing the data impact of code changes introduced in pull requests. By default, `piperider compare` employs a _three-dot comparison method_, which aligns with [GitHub’s approach](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-comparing-branches-in-pull-requests#about-three-dot-comparison-on-github).
{% endhint %}

To enhance the report with more information, you can add `piperider` tags to models and metrics. By doing so, you can gain deeper insights into the data. To explore these enriched features, please visit piperider [run](run/) command.

## What's next

* See the full tutorial of the [Jaffle Shop](tutorials/dbt.md)
* Understand the [run](run/)
* Understand the [compare](compare.md)
* Try [PipeRider Cloud](../piperider-cloud/get-started.md)
