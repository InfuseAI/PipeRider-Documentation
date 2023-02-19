---
description: Query the metric
---

# Metrics

A metric is an aggregation over a table. In general, it consist

* The model to aggregate. (e.g. `Events`)
* The column to aggregate (e.g. `user_id`)
* The aggregation method (e.g. distinct count)
* The time column to group (e.g. `event_time`)
* The time granularity method to group (e.g. `group by date_trunc('DAY', event_time)`
* (Optional) Additional dimension to group (e.g. `group by country`)

So if we can define a **active users** by

> The **distinct count** of the `user_id` group by **day** of the `event_time` of the `Events` table.

The dbt metrics provide a way to define the metrics, and PipeRider implement the query of the metrics according to the dbt metrics definition.

## How to use

There are three steps to&#x20;

* define metrics in your dbt project
* update the dbt manfests
* run piperider

### Define a metric

To define a metric, please see the [dbt metric document](https://docs.getdbt.com/docs/build/metrics#defining-a-metric) to see how to define a metric in your dbt project

{% code title="models/marts/<metric>.yml" %}
```yaml
metrics:
  - name: active_users
    label: Active Users
    model: ref('stg_events')
    description: "The active user"

    calculation_method: count_distinct
    expression: user_id 

    timestamp: event_time
    time_grains: [day, week, month, year]
    tags: ['piperider']
```
{% endcode %}

{% hint style="info" %}
To tell PipeRider to query your metrics, you must add the `piperider` tag. The tag name can be changed in the `config.yml`
{% endhint %}

### Update the manifest

PipeRider use the [dbt manifest](https://docs.getdbt.com/reference/artifacts/manifest-json) to integrate with the dbt project. So any change in you dbt project, please run any commands which would change the manifest. In the development time, you can use  `compile`

```
dbt compile
```

### Run PipeRider

Now, you can try if the metric is successfully queried

```
piperider run
```

View metrics in your PipeRider Report. Metric query results can is shown in the `Metrics`  page

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

## More about metric queries

### How PipeRider query the dbt metric

PipeRider queries each `time_grain` _except_ `all_time` and, this results in a **maximum of 5 queries per metric**.

For instance, given the following metric, PipeRider would perform four queries: `daily`, `weekly`,  `monthly`, and `yearly`.

```yaml
metrics:
  - name: active_users
    label: Active Users
    model: ref('stg_events')
    description: "The active user"

    calculation_method: count_distinct
    expression: user_id 

    timestamp: event_time
    time_grains: [day, week, month, year]
    tags: ['piperider']
```

For each time grains, PipeRider only queries the last n period of the data

| time\_grain | metric query                          |
| ----------- | ------------------------------------- |
| day         | daily result for last 30 days         |
| week        | weekly result for last 12 week        |
| month       | monthly result for last 12 months     |
| quarter     | quarterly result for last 10 quarters |
| year        | yearly result for last 10 years       |

### How about dimensions?

PipeRider currently support only the default queries. Queries for specific dimensions and other query customization will be added in the future. Tell us if you consider it is super useful for us. It will help us to prioritize them earlier.

### Run on single metric

When developing a new metric, it is time-consuming to run through all the piperider run to test the metric result. A tip is that use the `dbt list` to shorten the development cycle

1. Update the dbt yaml file
2. Update the dbt manifest by `dbt compile`
3. Run piperider by dbt list output

```sh
dbt list -s metric:active_users | piperider run --dbt-list     
```

## Compare metrics

With PipeRider's **compare** feature, you're also able to compare metrics between reports, which is particularly useful when analyzing the impact of data model or metric definition changes.

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
| window              | No. Support for `window` is coming soon                 |

{% hint style="warning" %}
Metrics that use the 'window' property are currently **not** supported and will not be queried. Support for 'window' is will be added in an upcoming release.
{% endhint %}

### Derived metric

PipeRider supports querying [derived metrics](https://docs.getdbt.com/docs/build/metrics#derived-metrics). It's useful for

* ratios
* subtractions
* any arbitrary calculation

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
