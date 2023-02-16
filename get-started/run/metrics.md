---
description: Query the metric
---

# Metrics

[dbt Metrics](https://docs.getdbt.com/docs/build/metrics) provide a way to define key metrics as time series data. Using PipeRider reports you can query, visualize, compare your time-series metrics

## Define the metrics

### Example metric

To define a metric, please see the [dbt metric document](https://docs.getdbt.com/docs/build/metrics#defining-a-metric) to see how to define a metric in your dbt project.

For PipeRider to see your metrics, you must add the `piperider` tag to the metric definition

```
tags: ['piperider']
```

The full metric definition

{% code title="models/marts/<metric>.yml" %}
```yaml
metrics:
  - name: revenue
    label: Revenue
    model: ref('orders')
    description: "The total revenue of our jaffle business"

    calculation_method: sum
    expression: amount 

    timestamp: order_date
    time_grains: [day, week, month, year]
    ...
    ...
    tags: ['piperider']
```
{% endcode %}

### Derived metric

PipeRider also supports querying derived metrics by adding the PipeRider tag, E.g.

{% code title="models/marts/<derived-metric>.yml" %}
```yaml
metrics:
  - name: average_revenue_per_customer
    label: Average Revenue Per Customer
    description: "The average revenue received per customer"

    calculation_method: derived
    expression: "{{metric('total_revenue')}} / {{metric('count_of_customers')}}"
    tags: ['piperider']
```
{% endcode %}

## Query the metrics

After defining the metric, when you run PipeRider metrics will be queried and included in your PipeRider report. But before it, remember to update the manifest artifact. The easiest way is to run `dbt compile`

```bash
dbt compile
```

then run piperider

```
piperider run
```

View metrics in your PipeRider Report. Metric query results can be accessed by click the `Metrics`  item in the left sidebar of your report.

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### How do metrics affect the number of queries performed?

dbt metric files serve only to _define_ the metrics, the way metrics are queried depends on the application.

PipeRider queries each `time_grain` _except_ `all_time` and, as `dimensions` are currently not considered, this results in a **maximum of 5 queries per metric**.

For instance, given the following metric, PipeRider would perform three queries: `month`, `quarter`, and `year`.

```yaml
metrics:
  - name: new_customers
    label: New Customers
    model: ref('dim_customers')
    calculation_method: count_distinct
    expression: user_id 
    timestamp: signup_date
    time_grains: [month, quarter, year, all_time]
    dimensions: [country, city]
```

PipeRider uses the following maximum ranges for each time\_grain.

| time\_grain | metric query                          |
| ----------- | ------------------------------------- |
| day         | daily result for last 30 days         |
| week        | weekly result for last 12 week        |
| month       | monthly result for last 12 months     |
| quarter     | quarterly result for last 10 quarters |
| year        | yearly result for last 10 years       |

### Debug the Metric

When developing a new metric, it is time-consuming to run through all the piperider run to test the metric result. A tip is that use the `dbt list` to test a specific metric. So development cycle would be

1. Update the dbt yaml file
2. Update the dbt manifest by `dbt compile`
3. Run piperider by dbt list output

```sh
dbt list -s metric:active_authors | piperider run --dbt-list     
```

## Compare metrics

With PipeRider's compare reports feature, you're also able to compare metrics between reports, which is particularly useful when analyzing the impact of data model changes.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>



## Dbt metrics compatibility

PipeRider supports the follow dbt metric properties.

| Field               | Support                                                 |
| ------------------- | ------------------------------------------------------- |
| name                | yes                                                     |
| model               | yes                                                     |
| label               | yes                                                     |
| description         | yes                                                     |
| calculation\_method | count, count\_distinct, sum, average, min, max, derived |
| expression          | yes                                                     |
| timestamp           | yes                                                     |
| time\_grains        | yes                                                     |
| dimensions          | Dimensions are currently not shown on metric charts     |
| window              | No. Support for 'window' is coming soon                 |

{% hint style="warning" %}
Metrics that use the 'window' property are currently **not** supported and will not be queried. Support for 'window' is will be added in an upcoming release.
{% endhint %}
