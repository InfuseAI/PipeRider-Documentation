---
description: About PipeRider telemetry data
---

# Privacy

In order to provide a better user experience by understanding how users are interacting with PipeRider, we use the following anonymous tracking methods.

{% hint style="info" %}
**PipeRider does** **not store or transmit the contents of your data source**. Your data source is accessed for the sole purpose of providing your profiling report and assertion test functionality
{% endhint %}

## Telemetry

PipeRider telemetry data is gathered both per-user and per-project.

### User Telemetry

A `user_id` is stored in in your profile.yml file located in `~/.piperider/profile.yml`

```python
user_id: abc123
anonymous_tracking: true
```

#### Opt-out of anonymous tracking

To opt-out of all telemetry tracking change the value of `anonymous_tracking` to false.

```python
anonymous_tracking: false
```

### **Project telemetry**

Each time you create a PipeRider project a telemetry ID will be created. This telemetry ID is linked to your `user_id`.

#### Opt-out of anonymous tracking per-project

To opt-out of tracking on a per-project basis, you can remove the telemetry ID located in .piperider/config.yml

```python
telemetry:
  id: xyz123
```

## What information is stored?

For each `user_id` the following information is stored:

### User information

* First seen: The date that you first used PipeRider
* Last seen: The last time you used PipeRider
* Location: Country and region
* IP address
* Platform: OS platform
* Device ID: A unique identifier for your device
* Python version

### Project information

* Project ID (telemetry ID)
* Current PipeRider version
* Initial PipeRider version (the first version installed)
* Data source type E.g. Snowflake, Postgres, etc.
* The following information is also stored on a per event\* basis
  * Number of tables in the data source
  * Number of columns and rows in the data source
  * Number of built-in assertions in use
  * Number of custom assertions in use
  * Number of passed and failed assertions

\* An event refers to a PipeRider command such as `run`, `generate-assertions`, `generate-report` etc.
