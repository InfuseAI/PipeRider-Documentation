# Assertion Yaml

PipeRider will parse all yaml files at `.piperider/assertions`. Grouping assertions by files is a way to organize your assertions. Using `piperider diagnose` to verify the format of assertion yaml files.

Whether recommended assertions or assertion templates, the format looks like the example below. You can use [built-in assertion](assertion-configuration.md) functions or [custom assertion](custom-assertions.md) functions against tables or columns depending on the functions.&#x20;

```yaml
Table_name_1:
  description: the description will be displayed as a tool tips in the report
  # Test Cases for Table
  tests:
  - name: assertion_function_wtih_parameter
    assert:
      a_parameter: a_value
    tags:
    - name_your_tag_optional
  columns:
    column_name:
      description: the description will be displayed as a tool tips in the report
      # Test Cases for Column
      tests:
      - name: assertion_function_wtihout_parameter
        tags:
        - name_your_tag_optional
  Table_name_2:
  description: the description will be displayed as a tool tips in the report
  # Test Cases for Table
  tests:
  - name: assertion_function_wtih_parameter
    assert:
      a_parameter: a_value
    tags:
    - name_your_tag_optional
  columns:
    column_name:
      description: the description will be displayed as a tool tips in the report
      # Test Cases for Column
      tests:
      - name: assertion_function_wtihout_parameter
        tags:
        - name_your_tag_optional
```

