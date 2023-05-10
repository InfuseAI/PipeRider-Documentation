---
description: Release notes for PipeRider OSS
---

# PipeRider

Please refer to the [PipeRider Releases page](https://github.com/InfuseAI/piperider/releases) on GitHub for more details, including notes on bug fixes and release candidates.&#x20;



## PipeRider OSS v0.25.0

Release date: next

#### Feature deprecation notice

The following features are now deprecated and will be removed from a future version of PipeRider:

* &#x20;Using PipeRider in **non-dbt projects**&#x20;
* **PipeRider assertions**

These features are still included in PipeRider for the time being, but are not recommended for continued use.

## PipeRider OSS v0.24.1

Release date: [2023-05-04](https://github.com/InfuseAI/piperider/releases/tag/v0.24.1)

Bug fix:

* Piperider bug with PipeRider using the wrong schema when using the `--table` option

## PipeRider OSS v0.24.0

Release date: [2023-05-04](https://github.com/InfuseAI/piperider/releases/tag/v0.24.0)

Bug fixes, including:

* Fix issue when loading dbt project yaml
* Fix PiperRider Cloud report URL in CLI output

## PipeRider OSS v0.23.0

Release date: [2023-04-20](https://github.com/InfuseAI/piperider/releases/tag/v0.23.0)

* **AWS Athena Support**: PipeRider now supports querying and profiling data stored in AWS Athena.
* **Skipped model reasons**: We have added the ability to display the reason why certain models were skipped during profiling and metric queries, making it easier for users to understand and improve their data pipeline.
* **Compare Recipe Enhancements**:&#x20;
  * Our Compare recipe has been enhanced to support both file and Jinja formats, allowing users to template values from environment variables for more dynamic comparisons.
  * The `compare` command now supports [comparing to a file](../get-started/compare.md#recipe-example-base-is-from-file). This means you can manually set the baseline to an existing report, and then use that in future comparisons without the need to run the baseline profile each time.
* **Other Enhancements and Bug Fixes**: We have also made several other enhancements and bug fixes, including the addition of buttons for GitHub, Discord, and documentation feedback, ensuring users have an easy and efficient way to provide feedback and suggestions for future improvements.

## PipeRider OSS v0.22.0

Release date: [2023-03-30](https://github.com/InfuseAI/piperider/releases/tag/v0.22.0)

* We are excited to announce the release of our new Compare GitHub Action! This powerful new feature allows you to easily compare code changes and send summary to your PR comment. For more information, please visit our [**GitHub Marketplace page**](https://github.com/marketplace/actions/piperider-compare-action).
* We have made some layout adjustments to our application to improve its responsiveness and overall user experience.
* Resolved an issue related to symbolic links on Windows.

## PipeRider OSS v0.21.0&#x20;

Release date [03-16-2023](https://github.com/InfuseAI/piperider/releases/tag/v0.21.0)

* Changed to using 'three-dot compare' (instead of two-dot) when running `piperider compare`. Refer to the GitHub documentation for more information on [two-dot and three-dot Git diff comparisons](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-comparing-branches-in-pull-requests#three-dot-and-two-dot-git-diff-comparisons).&#x20;

## PipeRider OSS v0.20.0

Release date: [2023-03-07](https://github.com/InfuseAI/piperider/releases/tag/v0.20.0)

* Improved the cloud signup process.
* `piperider compare` now supports PipeRider cloud.



