---
description: Integrate PipeRider with a dbt project in 5 mins
---

# Quick Start

This guide illustrates the usage of PipeRider with a dbt project by using dbt's [Jaffle Shop](https://github.com/dbt-labs/jaffle\_shop) project as an example.

## Prepare a dbt project

Clone the _jaffle\_shop_ repository, set up a profile, and make sure that you have installed dbt and related packages.

### Clone Jaffle Shop

```bash
git clone https://github.com/dbt-labs/jaffle_shop.git
cd jaffle_shop
```

### Create a dbt profile.yml

To quickly set up a profile, we provide the following example that uses [_DuckDB_](https://duckdb.org/).

Save the following content directly to a new file name `profiles.yml`, and place it under the _jaffle\_shop_ directory.

```yaml
# ./profiles.yml
jaffle_shop:
  target: dev
  outputs:
    dev:
      type: duckdb
      path: jaffle_shop.duckdb
```

### Install and run dbt

Install [`dbt-duckdb`](https://docs.getdbt.com/reference/warehouse-setups/duckdb-setup).

```bash
pip install dbt-duckdb
```

Ensure the project builds without error.

```
dbt build
```

## Install PipeRider

Install PipeRider with the DuckDB connector.

```bash
pip install 'piperider[duckdb]'
```

{% hint style="info" %}
&#x20;PipeRider requires Python 3.7+
{% endhint %}

## Tag models to profile

Use a config block to add the 'piperider tag to the following models:&#x20;

* &#x20;`models/customers.sql`&#x20;
* &#x20;`models/orders.sql`.

```
{{ config(tags=['piperider']) }}
```

Refer to [Specify resources to profile](specify-resources-to-profile.md) for other methods of selecting resources.

{% hint style="info" %}
If you don't specify models to profile, PipeRider will only be able to detect schema changes
{% endhint %}

## Make a change to the project

Now that the dbt project is configured and PipeRider is installed, you can follow the normal practice of create a new branch to make your project changes on.

### Create a new development branch

Create a new development branch to make changes to the project.

```bash
git checkout -b feature/add-average-value-per-order
```

### Update a model

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

### Build the project

Run dbt command again to ensure that the changes have been applied correctly and the build completes without error.

```
dbt build 
```

## Compare your branch with main

Using PipeRider's compare feature, you can now compare the state of you project before and and after making the recent model changes

Execute the following command to generate a comparison report for comparing the transformed models

```bash
piperider compare
```

PipeRider will output a comparison report link when the compssre process is finished. The report contains detailed data profiling statistics of your project before and after making the recent change.&#x20;

{% hint style="info" %}
PipeRider focuses on analyzing the data impact of code changes introduced in pull requests. By default, `piperider compare` employs a _three-dot comparison method_, which aligns with [GitHub’s approach](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-comparing-branches-in-pull-requests#about-three-dot-comparison-on-github).
{% endhint %}

Please check the [piperider run documentation](run/) for more information on running PipeRider and specifying resources.

## What's next

* See the full tutorial of the [Jaffle Shop](tutorials/dbt.md)
* Learn more about [piperider run](run/)
* Learn more about [piperider compare](compare.md)
* Try [PipeRider Cloud](../piperider-cloud/get-started.md)
