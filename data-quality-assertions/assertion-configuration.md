---
description: Out-of-the-box assertions and usage examples.
---

# Built-In Assertions

You can go to `.piperider/assertions/` to add assertions in _\<table>.yml_.

Here are built-in assertions, there are two types of assertions, one takes no parameter, the other takes parameters.

## Column Assertion

### assert\_column\_exist

* Description: The column must exit.
* Assert: None
* Tags:

<details>

<summary>YAML example</summary>

```yaml
world_city:  #Table Name
  columns:
    country_code:
      tests:
      - name: assert_column_exist
        tags:
          - dialing code
```

</details>

### assert\_column\_in\_types

* Description: The type of the column must be contained in the list.
* Assert:
  * `types: [numeric, string, datetime]`
* Tags:

<details>

<summary>YAML example</summary>

```yaml
world_city:  #Table Name
  columns:
    country_code:
      tests:
      - name: assert_column_in_types
        assert:
          types: [string]
        tags:
          - dialing code
```

</details>

### assert\_column\_min\_in\_range

* Description: The minimum value of the column must be between _min\_value_ and _max\_value_.
* Assert:
  * `min: [min_value, max_value]`
* Tags:

<details>

<summary>YAML example</summary>

```yaml
world_city:
  columns:
    population:
      tests:
      - name: assert_column_min_in_range
        assert:
          min: [1, 1000]
        tags:
          - small country
```

</details>

### assert\_column\_max\_in\_range

* Description: The maximum value of the column must be between _min\_value_ and _max\_value_.
* Assert\_:\_
  * `max: [min_value, max_value]`
* Tags:

<details>

<summary>YAML example</summary>

```yaml
world_city:
  columns:
    population:
      tests:
      - name: assert_column_max_in_range
        assert:
          max: [100000000, 2000000000]
        tags:
          - large country
```

</details>

### assert\_column\_not\_null

* Description: The values of the column must not be null.
* Assert: None
* Tags:

<details>

<summary>YAML example</summary>

```yaml
world_city:
  columns:
    name:
      tests:
      - name: assert_column_not_null
        tags:
          - city name
```

</details>

### assert\_column\_null

* Description: The values of the column must be null.
* Assert: None
* Tags:

<details>

<summary>YAML example</summary>

```yaml
world_city:
  columns:
    crime_rate:
      tests:
      - name: assert_column_null
        tags:
          - ToDo
```

</details>

### assert\_column\_type

* Description: The type of the column must match the specified type.
* Assert:
  * `type: numeric, string, datetime`
* Tags:

<details>

<summary>YAML example</summary>

```yaml
world_city:
  columns:
    name:
      tests:
      - name: assert_column_type
        assert:
          type: string
        tags:
          - city name
```

</details>

### assert\_column\_schema\_type

* Description: The column schema type must match a specified schema type.
* Assert:
  * `schema_type`: `TEXT`, `DATE`, `VARCHAR(128)`, or etc...
* Tags:

<details>

<summary>YAML example</summary>

```yaml
world_city:
  columns:
    country_code:
      tests:
      - name: assert_column_schema_type
        assert:
          schema_type: CHAR(3)
        tags:
          - dialing code
```

</details>

### assert\_column\_unique

* Description: The values of column must be unique.
* Assert: None
* Tags:

<details>

<summary>YAML example</summary>

```yaml
world_city:
  columns:
    country_code:
      tests:
      - name: assert_column_unique
        tags:
          - dialing code
```

</details>

## Table Assertion

### assert\_row\_count

* Description: The row count must be _>= min_ or _<= max_. _If min is not specified, it is assumed to be 0. If max is not specified, it is assumed to be infinity._ If min and max are both specified, min must be less than or equal to max.
* Assert:
  * `min: min_count`
  * `max: max_count`
* Tags:

<details>

<summary>YAML example</summary>

```yaml
# With the minimum and maximum row count in the following format
world_city:
  tests:
  - name: assert_row_count
    assert:
      min: 10000
    tags:
      - United Nations
```

```yaml
# With the minimum row count only in the following format
world_city:
  tests:
  - name: assert_row_count
    assert:
      min: 10000
      max: 100000
    tags:
      - United Nations
```

</details>

### assert\_row\_count\_in\_range

* Description: The row count must be between _min\_count_ and _max\_count_.
* Assert:
  * `count: [min_count, max_count]`
* Tags:

<details>

<summary>YAML example</summary>

```yaml
world_city:
  tests:
  - name: assert_row_count_in_range
    assert:
      count: [10000, 20000]
    tags:
      - United Nations
```

</details>
