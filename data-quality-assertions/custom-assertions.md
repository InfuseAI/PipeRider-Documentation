---
description: How to create custom assertions to check the quality of your data.
---

# Custom Assertions

Piperider provides a few [built-in assertions](assertion-configuration.md) and also supports custom assertions as _plugins_ which can satisfy the data quality check on your demand. Here you will learn the magic and create your first custom assertion.

## How to Create an Assertion Function

### Plugins

Piperider, by default, will load python files under `.piperider/plugins` as custom assertion functions automatically. `.piperider/plugins` is created by `piperider init` with a scaffolding of a custom assertion function, `customized_assertions.py`. You can rename the file or create assertion functions in other python file there.

{% hint style="info" %}
The search path to `plugins/` can be overwritten by the environment variable **`PIPERIDER_PLUGINS`**. Define your path by setting the variable.
{% endhint %}

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
if column_metrics is None:
  # cannot find the column in the metrics
  return context.result.fail()
```

### Assertion

When you first time execute `piperider run` , You will be prompted for the generation of recommended assertions, if _yes_, recommended assertions will be generated, otherwise, assertion scaffoldings will be generated at `.piperider/assertions`.

```
No assertion found
Do you want to auto generate recommended assertions for this datasource [Yes/no]
```

The scaffolding of assertion yaml looks like below

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
      - name: alphanumeric_only # custom assertion function without parameters
      - name: distinct_count # custom assertion function takes a parameter
        assert:
          count: 505
```

### Scaffolding of Assertion Function

This is the context of `customized_assertions.py`. A custom assertion class has to implement `BaseAssertionType` \_\_ and its functions, `name()`, _`execute()` and `validate()`._

```python
from piperider_cli.assertion_engine.assertion import AssertionContext, AssertionResult, ValidationResult
from piperider_cli.assertion_engine.types import BaseAssertionType, register_assertion_function


class AssertNothingTableExample(BaseAssertionType):
    def name(self):
        return 'assert_nothing_table_example'

    def execute(self, context: AssertionContext, table: str, column: str, metrics: dict) -> AssertionResult:
        table_metrics = metrics.get('tables', {}).get(table)
        if table_metrics is None:
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

    def validate(self, context: AssertionContext) -> ValidationResult:
        result = ValidationResult(context)
        # result.errors.append('explain to users why this broken')
        return result


class AssertNothingColumnExample(BaseAssertionType):
    def name(self):
        return "assert_nothing_column_example"

    def execute(self, context: AssertionContext, table: str, column: str, metrics: dict) -> AssertionResult:
        column_metrics = metrics.get('tables', {}).get(table, {}).get('columns', {}).get(column)
        if column_metrics is None:
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

    def validate(self, context: AssertionContext) -> ValidationResult:
        result = ValidationResult(context)
        # result.errors.append('explain to users why this broken')
        return result

# register new assertions
register_assertion_function(AssertNothingTableExample)
register_assertion_function(AssertNothingColumnExample)
```

#### Methods

`name()`: return the name of the testing function that will be used in assertion yaml.

`execute()` : the implementation of the testing logic. PipeRider will bring arguments below to `execute` method for you:

* _context_: helper object for assembling result entry to the report.
* _table_: the table name you are asserting.
* _column_: the column name you are checking, but it could be null when the assertion is run against a table.
* _metrics_: the profiling results could be referred for the assertion.

`validate()` : the validation of user inputs to the assertion function, i.e., it validates if the parameters taken by testing function are valid.

`register_assertion_function()`: register the custom assertion function so that it can be recognized in the assertion yaml.

#### Tips

Read a table metrics and a value of a specified key from it:

```python
# get a dict object of a table metrics
table_metrics = metrics.get('tables', {}).get(table)

if table_metrics is None:
  # cannot find the table in the metrics
  return context.result.fail()
  
# get a value of a metric by a key from the dict object of the table metrics
table_metrics.get('key')
```

Read a column metrics and a value of a specified key from it.

```python
# get a dict objec of a column metrics of a table 
column_metrics = metrics.get('tables', {}).get(table, {}).get('columns', {}).get(column)

if column_metrics is None:
  # cannot find the column in the metrics
  return context.result.fail()
  
# get a value of a metric by a key from the dict objec of the column metrics
column_metrics.get('key')
```

Read user input value of a parameter from an assertion yaml.

```python
context.asserts.get('parameter_a', [])
```

Return fail() or success() with a message

```
context.result.fail("message")
context.result.success("message")
```

One thing is not mentioned in the context.

{% hint style="info" %}
The value of the _actual_ will be printed out in the format `Actual: value` when assertion is executed.
{% endhint %}

```
context.result.actual
```

Assign `actual` any value as the actual finding that why the assertion successes or fails.

### Hands-On

So far you already have the fundamental knowledge of the assertion, start creating your first custom assertion.

Check your `./piperider/outputs/<run>/.profiler.json` and select a metric as the measurement. In my case, I choose `distinct`.

In my case, there is a table called _SYMBOL_, one of its columns is _Name_. Because I know Name must be contained in a list of names which has 600 names in the total, the metric value of _distinct_ should not exceed 600 otherwise something wrong in my data.

```yaml
 "SYMBOL": {
            "name": "SYMBOL",
            "row_count": 505,
            "col_count": 11,
            "columns": {
                "NAME": {
                    "total": 505,
                    "non_nulls": 505,
                    "distinct": 505,
```

Therefore, I want an assertion function called _assert\_distinct\_in\_range which takes two parameters, min and max, furthermore, it is declared in range\_check.py,_ in future I'll create more range-related assertions.

At `.piperider/assertions/` I create an assertion file, _my\_assertion.yml_ and edit the file to add my custom assertion logic against the column _Name_.

```yaml
SYMBOL:  # Table Name
  # Test Cases for Table
  tests:
  columns:
    NAME: # Column Name
      # Test Cases for Column
      tests:
        - name: assert_distinct_in_range
          assert:
            range: [0, 600]
            
```

Next I will define the corresponding assertion function. I go to `.piperider/plugins` and create a python file, _range\_check.py_. Edit the python file and add the custom assertion codes.

```python
from piperider_cli.assertion_engine.assertion import AssertionContext, AssertionResult, ValidationResult
from piperider_cli.assertion_engine.types import BaseAssertionType, register_assertion_function

class AssertDistinctInRange(BaseAssertionType):
    def name(self):
        return "assert_distinct_in_range"

    def execute(self, context: AssertionContext, table: str, column: str, metrics: dict) -> AssertionResult:
        column_metrics = metrics.get('tables', {}).get(table, {}).get('columns', {}).get(column)
        if column_metrics is None:
            # cannot find the column in the metrics
            return context.result.fail()

        context.result.actual = column_metrics.get('distinct')

        # Get user input range from the assertion
        expected = context.asserts.get('range', [])
        min = expected[0]
        max = expected[1]
    
        # Check if the distinct value sits in the range and return the result
        if context.result.actual <= max and context.result.actual >= min:
            return context.result.success("The value is {} sitting in the range".format(context.result.actual))

        return context.result.fail("The value is {} exceeding the range, something wrong in my data.".format(context.result.actual)) 

    def validate(self, context: AssertionContext) -> ValidationResult:
        expected = context.asserts.get('range')
        result = ValidationResult(context)

        # validate if the value of the parameter "range" is valid
        if expected is None:
            result.errors.append('Range is not defined')
        return result

register_assertion_function(AssertDistinctInRange)
```

I have added the custom assertion and the corresponding assertion function. Time to test it.

```shell
piperider run --table SYMBOL
```

After the running, I see the assertion result. The first custom assertions works!

```shell-session
?????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????? Assertion Results ?????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????????
[  OK  ] SYMBOL.NAME  assert_distinct_in_range  Expected: {'range': [0, 600]} Actual: The value is 505 sitting in the range
```

The custom assertion works, but not perfect. It needs other exception handling to make it more robust and more assertive.

### Debug Mode

If you need to debug custom assertion functions, just enable the debug mode with `--debug`.

It will print out the trackback of calls.

```
piperider run --debug
```
