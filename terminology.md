---
reference: https://www.notion.so/infuseai/PipeRider-Terminology-cfe67b62a1744749adcd4b340aec61ff
---

# Terminology

### Alert

An entry in the monitoring section, either in the normal or abnormal state.

### Assertion

Assertion can capture critical aspects of data understanding that analysts and practitioners know based on its semantic meaning.

### Configuration YAML

A config file describes what test will be run against what column.

### Custom assertion

Users can extend basic assertion with application-or domain-specific expectations to build the customized test assertion.

### Data-diff

Capability to compare the output of runs in a visual manner.

### Data quality

It is a perception or an assessment of data's fitness to serve its purpose in a given context.

### Data source

Provides a standard API for accessing and interacting with data from a wide variety of source systems.

### ID Token

An *ID token* is a JWT token issued by Firebase auth. It’s mainly used in a browser and should be invisible to users.

[Verify ID Tokens | Firebase Documentation](https://firebase.google.com/docs/auth/admin/verify-id-tokens)

### Incident

After analyzing the alert info, users are able to create a note to indicate something happens in when.

### Multi-run

Multiple executions of tests or sets of tests over a time period. The expected outcome is to generate a report for each run.

### Notification

The system sends a message to the subscribers when an abnormal state happens.

### Profiling

The act of generating metrics and candidate assertion from data.

### Snapshot test

In snapshot test, the initial run of the test saves the state of the current snapshot and compares the results of any consequent test against these saved snapshots to ensure the data persistence.

### Test harness

A web service that can be applied to trigger tests, list test history, view test reports, and visualize historical reports as piperider timeline.

### User API Token

An *user API token* is a 32 characters long string. This is usually the `PIPERIDER_API_KEY` users need to use with our [piperider-python-sdk](https://pypi.org/project/piperider-python-sdk/0.0.1/).

We may have other kind of tokens in the future, so to be clear, we call it `user API token` because it represents that user now.

Users can manage their `user API tokens` in the [User Setting page](https://app.piperider.io/settings).

