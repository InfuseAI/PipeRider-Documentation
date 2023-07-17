---
description: How to upload reports to PipeRider Cloud
---

# Upload Reports

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
