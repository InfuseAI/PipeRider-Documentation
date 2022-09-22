---
description: The nightly build of PipeRider
---

# Nightly Version

Our CI will build and push the latest nightly version into Pypi. If you want to try and learn what new features could be in next release, you are welcome to install the PipeRider nightly version to examine it.

```
pip install piperider-nightly
```

You can also install connectors with the nightly build.

```
pip install 'piperider-nightly[postgres]'
```

If you require a specific version, add the `version_number` according to the [release history](https://pypi.org/project/piperider-nightly/#history).

```
pip install piperider-nightly==version_number
```

{% hint style="danger" %}
Nightly versions may include features that are not yet complete or changes to existing features. Therefore, we advise not using it your production environment.
{% endhint %}
