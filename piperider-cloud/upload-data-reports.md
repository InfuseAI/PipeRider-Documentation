---
description: How to upload data reports to PipeRider Cloud
---

# Upload Data Reports

How you will upload reports to PipeRider Cloud depends on how you are using PipeRider.

| Using PipeRider for | Upload method                   |
| ------------------- | ------------------------------- |
| Development         | PipeRider OSS                   |
| Deployment /CI      | PipeRider Compare GitHub Action |

{% hint style="info" %}
If you are new to PipeRider, please refer to the [PipeRider Introduction](../), which explains the main use-cases for how PipeRider fits into your data reliability strategy.
{% endhint %}

## Development (PipeRider OSS)

To upload reports as part of you dbt development process, you are first required to:&#x20;

* [Install PipeRider OSS](../get-started/install-piperider.md)
* [Sign up and Sign in](get-started.md#piperider-cli) to PipeRider Cloud

### Enable auto-upload

The easiest way to upload reports to PipeRider is to enable auto upload.

```bash
$ piperider config enable-auto-upload
```

Once enabled, every time a report is generated it will be automatically uploaded to your PipeRider Cloud account.

The URL to access the report in your PipeRider Cloud account will be displayed in the command output.

### Per-report upload

To upload specific reports to PipeRider Cloud, use the `--upload` option. Both `piperider run` and `piperider compare` support the `--upload` option.

```bash
# Generate a PipeRider EDA Report and upload to PipeRider Cloud
$ piperider run --upload

# Generate a PipeRider Impact Report and upload to PipeRider Cloud
$ piperider compare --upload 
```

The URL to access the report in your PipeRider Cloud account will be displayed in the command output.

### Upload existing reports

To upload reports that have already been generated, use the following command:

```yaml
$ piperider cloud upload-report
```

You will be presented with a list of existing reports to select from.



## Deployment / CI (PipeRider Compare GitHub Action)

The following steps require a PipeRider API Token. This can be obtained from your [PipeRider Cloud Profile](https://cloud.piperider.io/settings/profile).

To use PipeRider as part of your deployment process for verifying data impact before merging to production, you can use the PipeRider Compare GitHub Action.

To configure the PipeRider Compare GitHub Action [follow the instructions on the GitHub Marketplace](https://github.com/marketplace/actions/piperider-compare-action#dbt-integration). A basic example, **without data warehouse settings**, is provided below. \\

Note the `upload: true` setting, which ensures the generated Impact Report will be uploaded to your PipeRider Cloud account.

```yaml
name: PipeRider Impact Report
on:
  pull_request:
    types: [opened, synchronize, reopened]
    branches: ['main']
    paths:
      - models/**
      - seeds/**
      - tests/**

jobs:
  piperider-compare:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
    steps:
    - uses: actions/checkout@v3

    - name: PipeRider Compare
      uses: InfuseAI/piperider-compare-action@v1
      with:
        cloud_api_token: ${{ secrets.PIPERIDER_CLOUD_API_TOKEN }}
        upload: true
```



## Next Steps

After uploading reports to PipeRider Cloud, you can now do the following:

* Keep track of reports with report history
* Share reports with others
  * Share workspace access to teammates
  * Share reports publicly
* View Lineage Diff in Impact Reports
* Generate Impact Reports online (by comparing two reports)
* Bi-directional links from pull request Impact Summaries to full Impact Reports in PipeRider Cloud (only for reports uploaded via the PipeRider Compare GitHub Action)





