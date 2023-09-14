---
description: Start uploading reports to PipeRider Cloud
---

# Get Started

## PipeRider Cloud features

With a [PipeRider Cloud](https://cloud.piperider.io) account you can upload reports from PipeRider CLI and access the following features:

* Upload reports automatically
* Create workspaces to organize reports&#x20;
* Share reports with team members or publicly
* Compare reports online
* View the Lineage Diff between reports&#x20;

## Sign up for PipeRider Cloud

There are two ways to sign up for PipeRider Cloud:

* PipeRider CLI
* PipeRider website

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

### PipeRider Website

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

Now you're ready to start [uploading reports](upload-data-reports.md) to PipeRider Cloud.
