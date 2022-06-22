---
description: How to set PipeRider GitHub Action in the continuous integration
---

# GitHub Action

For the sake of the continuous integration, PipeRider provides the GitHub Action that PipeRider can be triggered to profile data, test data and generate reports on events such push, pull\_request etc. Through reports you can have the insight to see if any concern of data changes or decayed data quality before taking next actions.

{% hint style="info" %}
You can find the [PipeRider CLI Action](https://github.com/marketplace/actions/piperider-cli-action) from GitHub Action Marketplace
{% endhint %}

## Requirement

You will need to create a yaml file e.g. `ci.yml` at `.github/workflows` and the `requirements.txt` at the root in your Git repository.

_Requirements.txt_, PipeRider Action will load the file to install necessary packages. You may want to replace the version of `snowflake-sqlalchemy`.

```
snowflake-sqlalchemy==1.1.0
piperider
```

### PipeRider Action Only

This is a sample of the CI yaml which contains PipeRider action only.&#x20;

You may want to replace the version when newer `piperider-action` is released in the future.

* `InfuseAI/piperider-action@<version>`

A sample of a CI yaml

```yaml
name: piperider-ci
env:
  ACTIONS_STEP_DEBUG: true

on: # define the trigger on events
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
        uses: InfuseAI/piperider-action@v0.3.0
```

### With Snowfalke/dbt

If Snowflake is the data source of your project and be with dbt, you can have the yml file like the sample below.

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

You may want to replace the version of `dbt-snowflake` in the yaml.

* `dbt-snowflake==<version>`

A sample of CI yaml

```yaml
name: piperider-ci
env:
  ACTIONS_STEP_DEBUG: true

# Action taken on events
on:
  push:
    branches: [ main ]
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

      - name: install dbt
        run: pip install -q 'dbt-snowflake==1.1.0'

      - name: dbt docs generate
        run: dbt docs generate --profiles-dir ./
        env:
          SNOWFLAKE_ACCOUNT: ${{ secrets.SNOWFLAKE_ACCOUNT }}
          SNOWFLAKE_USER: ${{ secrets.SNOWFLAKE_USER }}
          SNOWFLAKE_PASSWORD: ${{ secrets.SNOWFLAKE_PASSWORD }}
          SNOWFLAKE_ROLE: ${{ secrets.SNOWFLAKE_ROLE }}
          SNOWFLAKE_DATABASE: ${{ secrets.SNOWFLAKE_DATABASE }}
          SNOWFLAKE_SCHEMA: ${{ secrets.SNOWFLAKE_SCHEMA }}
          SNOWFLAKE_WAREHOUSE: ${{ secrets.SNOWFLAKE_WAREHOUSE }}

      - name: dbt run
        run: dbt run --profiles-dir ./
        env:
          SNOWFLAKE_ACCOUNT: ${{ secrets.SNOWFLAKE_ACCOUNT }}
          SNOWFLAKE_USER: ${{ secrets.SNOWFLAKE_USER }}
          SNOWFLAKE_PASSWORD: ${{ secrets.SNOWFLAKE_PASSWORD }}
          SNOWFLAKE_ROLE: ${{ secrets.SNOWFLAKE_ROLE }}
          SNOWFLAKE_DATABASE: ${{ secrets.SNOWFLAKE_DATABASE }}
          SNOWFLAKE_SCHEMA: ${{ secrets.SNOWFLAKE_SCHEMA }}
          SNOWFLAKE_WAREHOUSE: ${{ secrets.SNOWFLAKE_WAREHOUSE }}

      - name: PipeRider CLI Action
        uses: InfuseAI/piperider-action@v0.3.0
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

### Send notifications to your Slack

Finally, if you want to receive notifications when actions finish, you can use the `slack-github-action` at the bottom of the CI yaml.

Before that you will need to add the environment variable, SLACK\_WEBHOOK\_URL. How to gain your Slack Webhook URI, please see [Slack Send GitHub Action](https://github.com/slackapi/slack-github-action) repo.

You may want to replace the version of `slack-github-action`.

* `slackapi/slack-github-action@<version>`

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
