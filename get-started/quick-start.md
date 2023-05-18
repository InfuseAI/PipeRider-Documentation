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

To quickly set up a profile, we provide an example using _duckdb_.

Save the following content directly to `profiles.yml` and place it under the _jaffle\_shop_ directory.

```yaml
jaffle_shop:
  target: dev
  outputs:
    dev:
      type: duckdb
      path: jaffle_shop.duckdb
```

Install `dbt-duckdb`.

```bash
pip install dbt-duckdb
```

Load the CSVs.

```
dbt seed
```

## Install PipeRider

Install PipeRider with _duckdb_ related dependencies.

```bash
pip install 'piperider[duckdb]'
```

{% hint style="info" %}
&#x20;PipeRider requires python 3.7+
{% endhint %}

## Make a change

Create a new branch.

```bash
git checkout -b feature/add-average-value-per-order
```

Edit `models/customers.sql` and add a column for _customers_.&#x20;

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

Commit this change.

```bash
git add models/customers.sql
git commit -m 'Add average_value_per_order to customers'
```

## Run PipeRider

Run this command to generate a comparison report for comparing models transformed.

```bash
piperider compare
```

Check out the report link at the end of output.





## What's next

* Integrate [dbt metrics](run/metrics.md)
* Specify which models to profile
* Try [PipeRider Cloud](../piperider-cloud/get-started.md)
