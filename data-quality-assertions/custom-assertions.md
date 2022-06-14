---
description: How to create custom assertions to check the quality of your data.
---

# Custom Assertions

Piperider provides a few [built-in assertions](../assertion-configuration.md) and also supports custom assertions as _plugins_ which can satisfy the data quality check on your demand. Here you will learn the magic and create your first custom assertion.

## How to Create an Assertion Function

### Plugins

Piperider, by default, will load python files under `.piperider/plugins` as custom assertion functions automatically. `.piperider/plugins` is created by `piperider init` with a scaffolding of a custom assertion function, `customized_assertions.py`. You can rename the file or create other assertion functions in python there.&#x20;

{% hint style="info" %}
The search path to `plugins/` can be overwritten by the environment variable **`PIPERIDER_PLUGINS`**. Define your path by setting the variable.
{% endhint %}

### Namespace

In the real world, you usually have a plenty of assertions to assure the data quality. Those assertions are designed for various functions/purposes. In order to organize assertions, Piperider supports the _namespace_ which can organize assertions into groups.

The _namespace_ actually refers to the file name of assertions python file, i.e., you can create a python file, `string_validate.py`_,_ then _all of defined assertions in the file are under the namespace_, **string\_validate**_._ For the example_,_ You can use the defined assertion by `string_validate.alphanumeric_only`_._

### Metrics

`piperider run` will generate profiling results containing a plenty of metrics that your assertions could refer to those metrics for the data quality measurement. Metrics are saved in the `.piperider/outputs/<run>/.profiler.json` and the context are categorized into tables/columns. Please check the files to see what metrics you can refer to.

In the assertion python code, you can read metrics of tables into a dict object. The table will be what you use assertion against.

```python
table_metrics = metrics.get('tables', {}).get(table)
if not table_metrics:
  # cannot find the table in the metrics
  return context.result.fail()
```

read metrics of columns into a dict object. The column will be what you use assertion against.

```python
column_metrics = metrics.get('tables', {}).get(table, {}).get('columns', {}).get(column)
if not column_metrics:
  # cannot find the column in the metrics
  return context.result.fail()
```

### Assertion

You will be prompted by piperider run for the assertion templates generation if no assertions found. The generated assertions will be in `.yml`at `.piperider/assertions`.

```
No assertions found for datasource [ dataproject ]
Do you want to auto generate assertion templates for this datasource [yes/no]? y
```

The pattern of assertion yaml looks like below

```yaml
your_table_name:
  tests: []
  columns:
    your_column_name_a:
      tests: []
     your_column_name_b:
      tests: []
      ...
```

You can add built-in assertions and custom assertions against tables/column here.

e.g.

```yaml
PRICE:  # Table Name
  # Test Cases for Table
  tests: # assertion functions
    - name: assert_row_count_in_range # built-in assertion function takes a parameter
      assert:
        count: [0, 157881]
  columns:
    SYMBOL:  # Column Name
      # Test Cases for Column
      tests: # assertion functions
      - name: string_validate.alphanumeric_only # custom assertion function without parameters
      - name: distinct.count # custom assertion function takes a parameter
        assert:
          distinct_count: 505
```

### Scaffolding of Assertion Function

This is the context of `customized_assertions.py` that contains two sample functions which always return `success()`, _assert\_nothing\_table\_example_ and _assert\_nothing\_column\_example._

We will explain some noteworthy code in the [below](custom-assertions.md#undefined).

```python
def assert_nothing_table_example(context: AssertionContext, table: str, column: str, metrics: dict) -> AssertionResult:
    table_metrics = metrics.get('tables', {}).get(table)
    if not table_metrics:
        # cannot find the table in the metrics
        return context.result.fail()

    # 1. Get the metric for the current table
    # We support two metrics for table level metrics: ['row_count', 'col_count']
    row_count = table_metrics.get('row_count')
    # col_count = table_metrics.get('col_count')

    # 2. Get expectation from assert input
    expected = context.asserts.get('something', [])

    # 3. Implement your logic to check requirement between expectation and actual value in the metrics

    # 4. send result

    # 4.1 mark it as failed result
    # return context.result.fail('what I saw in the metric')

    # 4.2 mark it as success result
    # return context.result.success('what I saw in the metric')

    return context.result.success('what I saw in the metric')


def assert_nothing_column_example(context: AssertionContext, table: str, column: str, metrics: dict) -> AssertionResult:
    column_metrics = metrics.get('tables', {}).get(table, {}).get('columns', {}).get(column)
    if not column_metrics:
        # cannot find the column in the metrics
        return context.result.fail()

    # 1. Get the metric for the column metrics
    total = column_metrics.get('total')
    non_nulls = column_metrics.get('non_nulls')

    # 2. Get expectation from assert input
    expected = context.asserts.get('something', [])

    # 3. Implement your logic to check requirement between expectation and actual value in the metrics

    # 4. send result

    # 4.1 mark it as failed result
    # return context.result.fail('what I saw in the metric')

    # 4.2 mark it as success result
    # return context.result.success('what I saw in the metric')

    return context.result.success('what I saw in the metric')
```

#### Explanation

```python
# get a dict object of a table metrics
table_metrics = metrics.get('tables', {}).get(table)

# get a value of a metric by a key from the dict object of the table metrics
table_metrics.get('key')
```

```python
# get a dict objec of a column metrics of a table 
column_metrics = metrics.get('tables', {}).get(table, {}).get('columns', {}).get(column)

# get a value of a metric by a key from the dict objec of the column metrics
column_metrics.get('key')
```

```python
# Read the user input value of the parameter_a from the asserion yaml
context.asserts.get('parameter_a', [])
```

```
context.result.fail("message")
context.result.success("message")
```





### Customization
