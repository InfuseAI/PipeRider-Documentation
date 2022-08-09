---
description: How to set PipeRider GitHub Action in the continuous integration
---

# GitHub Action

In terms of the continuous integration, PipeRider provides the _GitHub Action_ that PipeRider can be triggered to profile data, test data and generate reports on events such push, pull\_request etc. Through reports you can have the insight to see if any concern of data changes or decayed data quality before taking next actions.

{% hint style="info" %}
You can find the [PipeRider CLI Action](https://github.com/marketplace/actions/piperider-cli-action) from GitHub Action Marketplace
{% endhint %}

{% hint style="info" %}
GitHub has the great documents about Actions, if you're new to it, suggest you to visit [GitHub Actions Documents](https://docs.github.com/en/actions/quickstart).
{% endhint %}

## Requirement

Basically, you will need to create a workflow yaml file e.g. `ci.yml` at `.github/workflows` and the `requirements.txt` at the root in your Git repository. If your project is with dbt, you will need to provide dbt-related `profiles.yml` as well.

* `.github/workflows/ci.yml` - the definition of the workflow
* `requirements.txt` - the necessary PiP packages for PipeRider GitHub Action, PipeRider Action will load it and install these packages accordingly.
* (if dbt project) `profiles.yml` - the dbt-related profiles configuration

`Requirements.txt`, PipeRider Action will load the file to install necessary packages. You may want to replace the version of `snowflake-sqlalchemy`. If with dbt, you need to specify the required [dbt adapter](https://docs.getdbt.com/docs/available-adapters) to the project.

#### A sample of the requirements.text

[reference](https://github.com/InfuseAI/dbt-infuse-finance/blob/main/requirements.txt)

```
snowflake-sqlalchemy==1.1.0
piperider
#e.g. dbt-snowflake
#e.g. dbt-postgres
#e.g. dbt-sqlite
```

### PipeRider Action Only

Basically, if there are no required credentials or environment variables to your project, you just need to add the step of PipeRider action.

#### A sample of a ci.yml

You may want to replace the [version](https://github.com/marketplace/actions/piperider-cli-action) when newer `piperider-action` is released in the future.

* `InfuseAI/piperider-action@<version>` - Specify the version

```yaml
name: piperider-ci
env:
  ACTIONS_STEP_DEBUG: true

on: # define the trigger on events, pull requests to main branch
  pull_request:
    branches: [ main ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

jobs:
  piperider-ci:
    runs-on: ubuntu-latest

    steps:
      - name: checkout
        uses: actions/checkout@v3

      - name: PipeRider CLI Action
        uses: InfuseAI/piperider-action@v0.4.1
```

### With Snowflake/dbt

If Snowflake is the data source of your project and being with dbt, you will have the yml file with Snowflake-related credentials like the sample below.

#### Prepare Secrets

Before that, you will need to add several environment variables and secrets for your workflow in the setting of the Git repository. About how to add secrets, please see [Environment Secrets](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment#environment-secrets) on GitHub Docs.

```yaml
SNOWFLAKE_ACCOUNT: ${{ secrets.SNOWFLAKE_ACCOUNT }}
SNOWFLAKE_USER: ${{ secrets.SNOWFLAKE_USER }}
SNOWFLAKE_PASSWORD: ${{ secrets.SNOWFLAKE_PASSWORD }}
SNOWFLAKE_ROLE: ${{ secrets.SNOWFLAKE_ROLE }}
SNOWFLAKE_DATABASE: ${{ secrets.SNOWFLAKE_DATABASE }}
SNOWFLAKE_SCHEMA: ${{ secrets.SNOWFLAKE_SCHEMA }}
SNOWFLAKE_WAREHOUSE: ${{ secrets.SNOWFLAKE_WAREHOUSE }}
```

#### A sample of ci.yml

* `dbt-snowflake==<version>` - Specify the version
* `PROFILES_DIR: ${{ github.workspace }}` - Instruct PipeRider Action where the dbt-related `profiles.yml` is.

[reference](https://github.com/InfuseAI/dbt-infuse-finance/blob/main/.github/workflows/ci.yml)

```yaml
name: piperider-ci

on: # define the trigger on events, pull requests to main branch
  pull_request:
    branches: [ main ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

jobs:
  piperider-ci:
    runs-on: ubuntu-latest

    steps:
      - name: checkout
        uses: actions/checkout@v3

      - name: PipeRider CLI Action
        uses: InfuseAI/piperider-action@v0.4.1
        id: piperider
        env:
          SNOWFLAKE_ACCOUNT: ${{ secrets.SNOWFLAKE_ACCOUNT }}
          SNOWFLAKE_USER: ${{ secrets.SNOWFLAKE_USER }}
          SNOWFLAKE_PASSWORD: ${{ secrets.SNOWFLAKE_PASSWORD }}
          SNOWFLAKE_ROLE: ${{ secrets.SNOWFLAKE_ROLE }}
          SNOWFLAKE_DATABASE: ${{ secrets.SNOWFLAKE_DATABASE }}
          SNOWFLAKE_SCHEMA: ${{ secrets.SNOWFLAKE_SCHEMA }}
          SNOWFLAKE_WAREHOUSE: ${{ secrets.SNOWFLAKE_WAREHOUSE }}
          DBT_PROFILES_DIR: ${{ github.workspace }}
```

#### A sample of dbt-related profiles.yml

The format is exactly same as your local `~/.dbt/profiles`, you just need to replace those values with preset [environmental secrets.](github-action.md#prepare-secrets) Save it at the root of the repository where `DBT_PROFILES_DIR: ${{ github.workspace }}` points.

[reference](https://github.com/InfuseAI/dbt-infuse-finance/blob/main/profiles.yml)

```yaml
your_dbt_profile_name:
  outputs:
    dev:
      type: snowflake
      account: "{{ env_var('SNOWFLAKE_ACCOUNT') }}"
      user: "{{ env_var('SNOWFLAKE_USER') | as_text }}"
      password: "{{ env_var('SNOWFLAKE_PASSWORD') | as_text }}"
      role: "{{ env_var('SNOWFLAKE_ROLE') | as_text }}"
      database: "{{ env_var('SNOWFLAKE_DATABASE') | as_text }}"
      warehouse: "{{ env_var('SNOWFLAKE_WAREHOUSE') | as_text }}"
      schema: "{{ env_var('SNOWFLAKE_SCHEMA') | as_text }}"
      threads: 4
  target: dev
```

### Send notifications to your Slack

One more thing, you may want to receive notifications on Slack when the workflow finishes. If so, you can add the step by using the `slack-github-action` at the bottom of the CI yaml.

Before that you will need to add the environment secret, `SLACK_WEBHOOK_URL`.

{% hint style="info" %}
How to gain your Slack Webhook URI, please see [Slack Send GitHub Action](https://github.com/slackapi/slack-github-action) repo.
{% endhint %}

* `slackapi/slack-github-action@<version>` - Specify the version

#### A sample of the step of Slack

[Reference](https://github.com/InfuseAI/dbt-infuse-finance/blob/693ed44c6e352604135e901fefa50eab591aa7c7/.github/workflows/ci.yml#L31)

```
      - name: send custom message to Slack
        uses: slackapi/slack-github-action@v1.19.0
        with:
          payload: |
            {
              "status": "${{ steps.piperider.outputs.status }}",
              "message": "${{ steps.piperider.outputs.analysis }}"
            }
        env:
          SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

### Workflow succeeded

When the workflow succeeded, you will see the _PipRider-Reports_ in the _Artifacts_. Download the zip file and check the reports.

![](../.gitbook/assets/ci\_succeeded.png)
