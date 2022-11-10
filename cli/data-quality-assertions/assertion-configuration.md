---
description: PipeRider's built-in set of data assertions with usage examples
---

# Built-In Assertions

Here are built-in assertions, there are two types of assertions, one takes no parameter, the other takes parameters.

## Basic Assertions

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

### assert\_column\_value

* Description: Assert the column value should be in the range.
* Assert:
  * `gte`: the value should be greater than or equal to
  * `gt`: the value should be greater than
  * `lte`: the value should be less than or equal to
  * `lt`: the value should be less than
  * `in`: the value should belong to the set

<details>

<summary>YAML example</summary>

The value should be between \[0,10000)

```yaml
world_city:
  columns:
    population:
      tests:
      - name: assert_column_value
        assert:
            gte: 0
            lt: 10000
```

The value of a datetime type column should be `>= '2022-01-01'`

```yaml
world_city:
  columns:
    create_at:
      tests:
      - name: assert_column_value
        assert:
          gte: '2022-01-01;
```

The value of the column should belong to \["male", "female"] set

```
TITANIC:
  columns:
    Sex:
      tests:
      - name: assert_column_value
        assert:
          in: ["male", "female"]
```

</details>

## Schema Assertions

### assert\_column\_exist

* Description: The column must exist.
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

* Description: The column schema type should match the specific schema type.
* Assert:
  * schema\_type: the schema type in data source. (e.g. `TEXT`, `DATE`, `VARCHAR(128)`, ...)

<details>

<summary>YAML Example</summary>

```
world_city:
  columns:
    name:
      tests:
      - name: assert_column_schema_type
        assert:
          schema_type: TEXT
```

</details>

### assert\_column\_in\_types

* Description: The type of the column must be contained in the list.
* Assert:
  * `types: [string, integer, numeric, datetime, boolean, other]`
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

## Metric-based Assertions

You can have assertions against [metrics](broken-reference) generated by PipeRider directly with several assertion expressions.

* Description: Metric-based assertions are assert the value of a metric.
* Assert:
  * `gte`: the value should be greater than or equal to
  * `gt`: the value should be greater than
  * `lte`: the value should be less than or equal to
  * `lt`: the value should be less than
  * `eq`: the value should equal to
  * `ne`: the value should not equal to

<details>

<summary>YAML Example</summary>

The row count should be <= 1000000

```yaml
world_city:
  tests:
  - metric: row_count
    assert:
      lte: 1000000
```

The missing percentage should be <= 0.01

```yaml
world_city:
  columns:
    country_code:
      tests:
      - metrics: nulls_p
        assert:
          lte: 0.01
```

The median should be between \[10, 20]

```yaml
world_city:
  columns:
    country_code:
      tests:
      - metrics: p50
        assert:
          gte: 10
          lte: 20
```

</details>