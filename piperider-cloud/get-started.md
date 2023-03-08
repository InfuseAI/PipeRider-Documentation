---
description: Start uploading reports to PipeRider Cloud
---

# Get Started

## PipeRider Cloud features

With a [PipeRider Cloud](https://cloud.piperider.io) account you can upload reports from PipeRider CLI and access the following features:

* Create workspaces to organize reports and share with team members
* Share reports online
* Compare reports online
* View time-series data for certain profile statistics

Reports can be configured to automatically upload each time PipeRider is ran, or can be uploaded individually ([see below](get-started.md#how-to-upload-reports-to-piperider-cloud)).

## Sign up for PipeRider Cloud

There are two ways to sign up for PipeRider Cloud, either by using PipeRider CLI, or via the website.

It is preferable to sign up via the CLI, as this will simultaneously create an account and enable you to configure your API token for report uploading.

### PipeRider CLI

To sign up for PipeRider Cloud using PipeRider CLI, follow the steps below.

1. Ensure that you have installed [PipeRider CLI](../get-started/quick-start.md).
2. Run `piperider cloud signup`.
3. Enter your email address.
4. Open your email account and find the 'welcome' email.
5. Click the 'Get API Token' button in the email.
6. Set your account password by filling out `New Password` and `Confirm New Password` (leave `Current Password` blank), and then click `Update Password`.
7. Copy the API token.
8. Go back to the command line and paste the API token.
9. Your account is now created and you are logged in via PipeRider CLI

### Website

To sign up for PipeRider Cloud via the website navigate to the [sign up page](https://cloud.piperider.io/signup) and fill out the sign up form.

### Log in

If you have already created a PipeRider Cloud account and need to log in via the CLI, you should first obtain the API token from your account.

1. Navigate to your [PipeRider Cloud profile page](https://cloud.piperider.io/settings/profile).
2. Copy the Token.
3. On the command line run `piperider cloud login`.
4. Enter your email address.
5. Paste the API token.

If successful, your account details will be displayed in the output.

```
[?] API token: abc123
───────────────────────────── Login Successful ───────────────────────────────────
                                User Profile

  Email                  Username      Full Name    Storage Location   Timezone
──────────────────────────────────────────────────────────────────────────────────
  support@piperider.io   Support       PipeRider    North America      Asia/Taipei
  
  [Config] Default project is set to 'workspace/default'
```

### Verify PipeRider Cloud connection

If you need to verify that your current PipeRider Cloud connection settings, run the following command from inside your project:

```
piperider diagnose
```

If correctly configured, you should see `PASS` for the `Check cloud account` test.

```
Check cloud account:
  Run as user: support@infuseai.io
    User Name: Support
    Full Name: PipeRider Support
  Auto Upload: False
  Default project: workspace-name/project-name
✅ PASS
```

By default, Auto Upload is not configured, see below for instructions on how to enable [automatically uploading reports](get-started.md#automatically-upload-reports).

## How to upload reports to PipeRider Cloud

Reports can either be uploaded on a per-report basis, or PipeRider can be configured to automatically upload all reports.

### Manually upload reports to PipeRider Cloud

Reports can be uploaded on a per-run basis using the `--upload` option.

```
piperider run --upload
```

Existing reports can also be uploaded with the following command:

```
piperider cloud upload-report
```

You will be prompted to select the reports you wish to upload from a list.

### Automatically upload reports

Upload settings are configured in your PipeRider profile, located at `~/.piperider/profile.yml`.

To enable automatic uploading, add `auto_upload: true` to the `cloud_config` section of your PipeRider profile:

{% code title="~/.piperider/profile.yml" %}
```yaml
user_id: user123
...
api_token: abc123
cloud_config:
  default_project: workspace-name/project-name
  auto_upload: true
```
{% endcode %}

If enabled, `auto_upload` will automatically upload reports, without prompt, as they are generated. Such as with `piperider run` or `piperider compare-reports`.
