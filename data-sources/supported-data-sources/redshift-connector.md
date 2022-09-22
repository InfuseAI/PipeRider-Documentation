---
description: Install PipeRider with the Redshift connector and connect to a data source.
---

# Redshift Connector

## Installation

```
pip install 'piperider[redshift]'
```

## Configure connection settings

Initialize a new PipeRider project using `piperider init` and when prompted select **Redshift** as the data source.

The following information is required.

* Redshift Endpoint URL
* Authentication method (_password_ or _iam_)



### Example initialization steps

#### IAM steps

```
Please enter the following fields for redshift
[?] Redshift Endpoint: default.xxxxxxx.ap-northeast-1.redshift-serverless.amazonaws.com:5439/dev
[?] Authentication Methods: iam
   password
 > iam

[?] Redshift IAM Role (optional):
[?] Redshift Database Name: sp500
[?] Redshift Schema: public
```

#### Password steps

```
Please enter the following fields for redshift
[?] Redshift Endpoint: default.xxxxxxx.ap-northeast-1.redshift-serverless.amazonaws.com:5439/dev
[?] Authentication Methods: password
 > password
   iam

[?] Redshift User: ppr@infuseai.io
[?] Redshift Password: ************
[?] Redshift Database Name: sp500
[?] Redshift Schema: public
```

## Test connection settings

After configuring your connection settings, ensure that PipeRider can connect to your Redshift data source.

```
piperider diagnose
```

{% hint style="info" %}
If connection timeout expired occurs, please try it later, in case, Redshift service was asleep.
{% endhint %}
