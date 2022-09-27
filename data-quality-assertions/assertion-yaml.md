---
description: The structure of the data assertions file
---

# Assertion YAML

PipeRider will parse all yaml files at `.piperider/assertions`. Grouping assertions by files is a way to organize your assertions. Using `piperider diagnose` to verify the format of assertion yaml files.

Whether recommended assertions or assertion templates, the format looks like the example below. You can use [built-in assertion](assertion-configuration.md) functions or [custom assertion](custom-assertions.md) functions against tables or columns depending on the functions.

```yaml
Table_name_1:
  # Test Cases for Table
  tests:
  - name: assertion_function_with_parameter
    assert:
      a_parameter: a_value
    tags:
    - name_your_tag_optional
  columns:
    column_name:
      # Test Cases for Column
      tests:
      - name: assertion_function_wtihout_parameter
        tags:
        - name_your_tag_optional
Table_name_2:
  # Test Cases for Table
  tests:
  - name: assertion_function_with_parameter
    assert:
      a_parameter: a_value
    tags:
    - name_your_tag_optional
  columns:
    column_name:
      # Test Cases for Column
      tests:
      - name: assertion_function_wtihout_parameter
        tags:
        - name_your_tag_optional
```
