---
description: Using dbt Metrics with PipeRider
---

# Metrics

[dbt Metrics](https://docs.getdbt.com/docs/build/metrics) provide a way to track key business metrics as time series data. Using PipeRider reports you can now visualize your time-series business metrics and compare metrics to different reports.&#x20;

<figure><img src="../../.gitbook/assets/piperider_business-metrics.png" alt=""><figcaption><p>Example of dbt metrics in PipeRider report</p></figcaption></figure>

### Expose dbt metrics to PipeRider

For PipeRider to see your metrics, you must add the `piperider` tag to the metric definition yaml file in your dbt project.&#x20;

Add the following line to the metrics you wish to track in PipeRider:

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

#### Derived metrics

PipeRider also supports querying derived metrics by adding the PipeRider tag, E.g.

{% code title="models/marts/<derived-metric>.yml" %}
```
metrics:
  - name: average_revenue_per_customer
    label: Average Revenue Per Customer
    description: "The average revenue received per customer"

    calculation_method: derived
    expression: "{{metric('total_revenue')}} / {{metric('count_of_customers')}}"
    tags: ['piperider']
```
{% endcode %}

Now, when you run PipeRider against your dbt state, metrics will be queried and included in your PipeRider report.

```
piperider run --dbt-state target/
```

### View metrics in your PipeRider Report

Metrics can be accessed by click the `Business Metrics` in the left sidebar of your report.

<figure><img src="../../.gitbook/assets/click_business-metrics.png" alt=""><figcaption><p>Click the Business Metrics icon to view metric charts</p></figcaption></figure>

### Comparison reports

With PipeRider's compare reports feature, you're also able to compare metrics between reports, which is particularly useful when analyzing the impact of data model changes.

<figure><img src="../../.gitbook/assets/piperider_compare-business-metrics.png" alt=""><figcaption><p>Compare metrics</p></figcaption></figure>

### Compatibility
