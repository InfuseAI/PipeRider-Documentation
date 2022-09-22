---
description: >-
  This is a HOW-TO of generating PipeRider reports, save it in AWS S3 and
  comparing  the latest report with previously saved report in S3 by GitHub CI.
---

# AWS S3 + GitHub CI

The PipeRider profiling report could give you an overview of your data from time to time. Integrating PipeRider into your CI workflow could benefit you with the auto-generated data profiling report, furthermore, you could save the report of each run in AWS S3 and have a comparison report of latest two run. Moreover, adding the Slack incoming webhook in the workflow to receive these reports from Slack in real time.

In this HowTo, we will show the scenario below

1. Generate a PipeRider latest run report and upload to S3
2. Download a previously saved report from S3
3. Compare these two report and upload the comparison report to S3
4. Push a notification to Slack with the links of latest run report and the comparison report

## Prerequisites

### S3 Bucket

Prepare a S3 bucket. Enable _ACL_ under _Permissions_ tab and _Static website hosting_ under _Properties_ tab.

![Enable ACL](../.gitbook/assets/s3\_permission\_acl.png)

![Enable Static website hosting](../.gitbook/assets/s3\_properties\_static\_website\_hosting.png)

#### Access key ID and secret access key

Prepare a user account for the aws-cli and save the generated key pair.

[Create a key pair](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-quickstart.html#cli-configure-quickstart-creds-create) (_Access key ID_ & _Secret access key_) for aws-cli and save them for the later use.

#### IAM Policy

Prepare a [IAM Policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access\_policies\_create-console.html) dedicated to the S3 bucket and grant an account used by aws-cli the permission by assign the policy.

You can create your own or just replace `DOC-EXAMPLE-BUCKET1` with your bucket in the following context and import it.

<details>

<summary>IAM Policy</summary>

```json
{
   "Version":"2012-10-17",
   "Statement":[
      {
         "Effect":"Allow",
         "Action": "s3:ListAllMyBuckets",
         "Resource":"*"
      },
      {
         "Effect":"Allow",
         "Action":["s3:ListBucket","s3:GetBucketLocation"],
         "Resource":"arn:aws:s3:::DOC-EXAMPLE-BUCKET1"
      },
      {
         "Effect":"Allow",
         "Action":[
            "s3:PutObject",
            "s3:PutObjectAcl",
            "s3:GetObject",
            "s3:GetObjectAcl",
         ],
         "Resource":"arn:aws:s3:::DOC-EXAMPLE-BUCKET1/*"
      }
   ]
}
```

</details>

### GitHub Repository

Prepare your repository with following configurations and files.

#### Environment secrets

Create the following [environment secrets](https://docs.github.com/en/actions/deployment/targeting-different-environments/using-environments-for-deployment#environment-secrets) in your repository:

* **AWS\_ACCESS\_KEY\_ID**\
  The Access key ID for the aws-cli
* **AWS\_SECRET\_ACCESS\_KEY**\
  The Secret access key for the aws-cli
* **AWS\_DEFAULT\_REGION**\
  The default AWS region
* **PIPERIDER\_BUCKET\_NAME**\
  The S3 bucket name
* **SLACK\_INCOMING\_WEBHOOK**\
  The _WebHook URL._ You will need to install the [Incoming WebHooks](https://infuseai.slack.com/apps/A0F7XDUAZ-incoming-webhooks) integration into Slack and create a configuration that specifies a channel where notifications go to. Then you will have the url.

#### Workflow YAML

A workflow yaml is required to GitHub CI. In your repo, create the path, necessary directories and the file of `.github/workflows/piperider.yml`. In this file, we define a event of pushing to main branch will trigger the workflow.

<details>

<summary>piperider.yml</summary>

```yaml
name: PipeRider Daily

on:
  push:
    branches:
      - main

jobs:
  piperider:
    runs-on: ubuntu-latest
    strategy:
      fail-fast: false

    steps:
      - uses: actions/checkout@v2

      - name: Set up Python 3.8
        uses: actions/setup-python@v2
        with:
          python-version: 3.8

      - name: Install PipeRider and check tools
        run: |
          echo "Upgrading pip"
          pip install --upgrade pip

          echo "Installing PipeRider"
          pip install piperider

          echo "Check the version of PipeRider"
          piperider version

          echo "Check the version of aws-cli"
          aws --version

          echo "Check the version of curl"
          curl --version

      - name: get-started project
        env:
          AWS_ACCESS_KEY_ID: ${{ secrets.AWS_ACCESS_KEY_ID }}
          AWS_SECRET_ACCESS_KEY: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          AWS_DEFAULT_REGION: ${{ secrets.AWS_DEFAULT_REGION }}
          PIPERIDER_BUCKET_NAME: ${{ secrets.PIPERIDER_BUCKET_NAME }}
          SLACK_INCOMING_WEBHOOK: ${{ secrets.SLACK_INCOMING_WEBHOOK }}
        run: ./daily.sh
```

</details>

There are two major steps in the workflow:

**Step: Install PipeRider and check tools**

We need _PipeRider_, _aws-cli_, and _curl_ tools. The Ubuntu provided by GitHub has installed _aws-cli_ and _curl_ tools. So in this step, PipeRider installation is the only required

**Step: get-started project**

We adopt the another repo, [PipeRider Getting Started](https://github.com/InfuseAI/piperider-getting-started), as our data project. daily.sh is a script to accomplish the whole of scenario.

#### daily.sh

Create the file with the following script and put it at the root of the repo. It will run through the scenario.

<details>

<summary>daily.sh</summary>

```bash
#!/usr/bin/env bash

# Get the PipeRider project and fetch dataset
git clone <https://github.com/InfuseAI/piperider-getting-started.git>
cd piperider-getting-started/
curl -o data/sp500.db <https://piperider-data.s3.ap-northeast-1.amazonaws.com/getting-started/sp500_20220401.db>

# Prepare for output location
report_dir=$(date +"%Y%m%d%H%M")
mkdir -p "${report_dir}/run"
piperider run -o "${report_dir}/run"

# Get the name of the previous single run
previous_report=$(aws s3 ls s3://${PIPERIDER_BUCKET_NAME}/storage/piperider/outputs/ | tail -1 | awk '{print $2}')

# Upload the latest single run
aws s3 sync .piperider/outputs s3://${PIPERIDER_BUCKET_NAME}/storage/piperider/outputs

# Download the previous single run
aws s3 sync s3://${PIPERIDER_BUCKET_NAME}/storage/piperider/outputs/${previous_report} .piperider/outputs/${previous_report}

# TODO check if there is the only one report
mkdir -p "${report_dir}/comparison"
piperider compare-reports --last -o "${report_dir}/comparison"

# upload data to s3
aws s3 sync .piperider/comparisons s3://${PIPERIDER_BUCKET_NAME}/storage/piperider/comparisons

# upload reports to web storage with public-read
# url {...}/reports/run/index.html
# url {...}/reports/comparison/index.html
single_report="<https://$>{PIPERIDER_BUCKET_NAME}.s3.${AWS_DEFAULT_REGION}.amazonaws.com/reports/${report_dir}/run/index.html"
comparison_report="<https://$>{PIPERIDER_BUCKET_NAME}.s3.${AWS_DEFAULT_REGION}.amazonaws.com/reports/${report_dir}/comparison/index.html"
aws s3 sync --acl public-read "${report_dir}" "s3://${PIPERIDER_BUCKET_NAME}/reports/${report_dir}"

# send to slack
payload='{
  "blocks": [
    {
      "type": "section",
      "text": {
        "type": "mrkdwn",
        "text": "<__SINGLE__|View> Single Report"
      }
    },
    {
      "type": "section",
      "text": {
        "type": "mrkdwn",
        "text": "<__COMPARISON__|View> Comparison Report"
      }
    }
  ]
}'

payload=${payload/__SINGLE__/$single_report}
payload=${payload/__COMPARISON__/$comparison_report}

curl $SLACK_INCOMING_WEBHOOK -d "$payload"
```

</details>

**Senario**

* preparation
  * PipeRider project
    * get the [piperider-getting-started](https://github.com/InfuseAI/piperider-getting-started.git) as the PipeRider data project and download the sample database sp500.db
  * decide the output path and create the necessary directory/sub-directory
    * decide the name of the output directory by the current datetime `$(date +"%Y%m%d%H%M")`
    * create the directory/sub-directory
    * fetch the name of the `previous` report from S3
* execution
  * generate a single latest report by `piperider run` with `-o` to specify where to save the copy of generated report
  * upload the latest report to S3
  * download the `previous` report from S3 to the default `.piperider/outputs` path
  * make a comparison by `piperider compare-reports` with `-o` and `--last`
    * `—last` will compare the latest two reports. One we generated, the other is one we downloaded from S3.
    * `-o` will save a copy of the comparison report where you specify
  * upload the comparison report to S3
* Publish and notify
  * form two URLs for the latest run report and the comparison report
  * upload reports to S3 by `aws s3 sync` with `--acl public-read` for public accessibility
  * push a notification containing links of two reports hosted in S3 to Slack

Try to push a commit to the main branch of your repository to trigger the workflow, then check the S3 bucket.

![S3 bucket](../.gitbook/assets/s3\_bucket\_reports.png)
