---
description: The supported assertions and their usages
---

# Assertion Configuration

## Column Assertion

### assert\_column\_exist

* Description: The column must exit.
* Assert: None
* Tags:

<details>

<summary>YAML example</summary>

```yaml
your_table_name:
  columns:
    your_coluumn_name:
      tests:
      - name: assert_column_exist
        tags:
          - OPTIONAL
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
your_table_name:
  columns:
    your_coluumn_name:
      tests:
      - name: assert_column_in_types
        assert:
          type: [string, datetime]
        tags:
          - OPTIONAL
```

</details>

### assert\_column\_min\_in\_range

* Description: The minimum value of the column must be between `min_value` and `max_value`.
* Assert:
  * `min: [min_value, max_value]`
* Tags:

<details>

<summary>YAML example</summary>

```yaml
your_table_name:
  columns:
    your_coluumn_name:
      tests:
      - name: assert_column_min_in_range
        assert:
          min: [10, 20]
        tags:
          - OPTIONAL
```

</details>

### assert\_column\_max\_in\_range

* Description: The maximum value of the column must be between `min_value` and `max_value`.
* Assert_:_
  * `max: [min_value, max_value]`
* Tags:

<details>

<summary>YAML example</summary>

```yaml
your_table_name:
  columns:
    your_coluumn_name:
      tests:
      - name: assert_column_max_in_range
        assert:
          max: [10, 20]
        tags:
          - OPTIONAL
```

</details>

### assert\_column\_not\_null

* Description: The values of the column must not be null.
* Assert: None
* Tags:

<details>

<summary>YAML example</summary>

```yaml
your_table_name:
  columns:
    your_coluumn_name:
      tests:
      - name: assert_column_not_null
        tags:
          - OPTIONAL
```

</details>

### assert\_column\_null

* Description: The values of the column must be null.
* Assert: None
* Tags:

<details>

<summary>YAML example</summary>

```yaml
your_table_name:
  columns:
    your_coluumn_name:
      tests:
      - name: assert_column_null
        tags:
          - OPTIONAL
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
your_table_name:
  columns:
    your_coluumn_name:
      tests:
      - name: assert_column_type
        assert:
          type: numeric
        tags:
          - OPTIONAL
```

</details>

### assert\_column\_unique

* Description: The values of column must be unique.
* Assert: None
* Tags:

<details>

<summary>YAML example</summary>

```yaml
your_table_name:
  columns:
    your_coluumn_name:
      tests:
      - name: assert_column_unique
        tags:
          - OPTIONAL
```

</details>

## Table Assertion

### assert\_row\_count

* Description: The row count must be between `min_count` and `max_count`.
* Assert:
  * `count: [min_count, max_count]`
* Tags:

<details>

<summary>YAML example</summary>

```yaml
your_table_name:
  tests:
  - name: assert_row_count
    assert:
      count: [10, 20]
    tags:
      - OPTIONAL
```

</details>
