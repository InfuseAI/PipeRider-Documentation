---
description: Release notes for PipeRider OSS
---

# PipeRider

Please refer to the [PipeRider Releases page](https://github.com/InfuseAI/piperider/releases) on GitHub for more details, including notes on bug fixes and release candidates.

## PipeRider OSS v0.41.0

Release date: [2023-11-23](https://github.com/InfuseAI/piperider/releases/tag/v0.41.0)

* Support for dbt 1.7 added

## PipeRider OSS v0.40.0

Release date: [2023-11-09](https://github.com/InfuseAI/piperider/releases/tag/v0.40.0)

* Bug fixes

## PipeRider OSS v0.39.0

Release date: [2023-11-02](https://github.com/InfuseAI/piperider/releases/tag/v0.39.0)

* Support added for DuckDB accessing files stored in S3
* Removal of PipeRider Assertions feature
* Bug fixes

## PipeRider OSS v0.38.0

Release date: [2023-10-26](https://github.com/InfuseAI/piperider/releases/tag/v0.38.0)

Bug fixes and chores.

## PipeRider OSS v0.37.0

Release date: [2023-10-19](https://github.com/InfuseAI/piperider/releases/tag/v0.37.0)

* dbt profile and target can now be selected with options `--dbt-profile` and `--dbt-target` ([GitHub Issue 898](https://github.com/InfuseAI/piperider/issues/898))
* Service account impersonation now supported ([GitHub Issue 899](https://github.com/InfuseAI/piperider/issues/899))
* Bug fixes

## PipeRider OSS v0.36.0

Release date: [2023-10-12](https://github.com/InfuseAI/piperider/releases/tag/v0.36.0)

Bug fixes.

## PipeRider OSS v0.35.0

Release date: [2023-09-28](https://github.com/InfuseAI/piperider/releases/tag/v0.35.0)

* PipeRider CLI support share to temporary quick look
* \[Report Improvement] Disclaimer is now shown in report if the `--skip-datasource` option is used to let users know that only metadata was used and a data profile was **not** performed

## PipeRider OSS v0.34.0

Release date: [2023-09-22](https://github.com/InfuseAI/piperider/releases/tag/v0.34.0)

### Features

* `--skip-datasource` option added to `piperider run` and `piperider compare` that generates a lightweight comparison based on dbt manifest files and features schema changes and Lineage Diff.

Please check the [release notes](https://github.com/InfuseAI/piperider/releases/tag/v0.34.0) for all changes.

## PipeRider OSS v0.33.0

Release date: [2023-09-07](https://github.com/InfuseAI/piperider/releases/tag/v0.33.0)

### Features

* PipeRider now supports the dbt Semantic Layer, introduced in dbt 1.6

### Breaking Changes

* PipeRider no longer supports metric configuration prior to dbt 1.6

## PipeRider OSS v0.32.0

Release date: [2023-08-24](https://github.com/InfuseAI/piperider/releases/tag/v0.32.0)

### Features

* The default resource selection behavior of `piperider run` and `piperider compare` has been updated:
  * `piperider run` now selects ALL resources by default.
  * `piperider compare` now selects `state:modified+` by default.
* Support for dbt-core 1.6

## PipeRider OSS v0.31.0

Release date: [2023-08-10](https://github.com/InfuseAI/piperider/releases/tag/v0.31.0)

### Features

* Improved CLI and Report hints when PipeRider detects that no resources have been profiled

## PipeRider OSS v0.30.0

Release date: [2023-08.02](https://github.com/InfuseAI/piperider/releases/tag/v0.30.0)

### Features

* Databricks now supported as a data source
* Updated Impact Summary on CLI, and in Markdown summary and HTML report
* When the `--modified` option is used, PipeRider will run `dbt build` with `state:modified+`
* [PipeRider Compare Action](https://github.com/marketplace/actions/piperider-compare-action) now supports `--modified` and `--select`

## PipeRider OSS v0.29.0

Release date: [2023-07-13](https://github.com/InfuseAI/piperider/releases/tag/v0.29.0)

* `piperider compare` now works in an uncommited Git repository. Previously, you were required to commit your changes, before running `piperider compare`. This requirement is no longer neccessary, and `compare` now works in a 'dirty repository'.
* `--modified` option added to `piperider compare` to enable profiling only modified resources.

## PipeRider OSS v0.28.0

Release date: [2023-06-29](https://github.com/InfuseAI/piperider/releases/tag/v0.28.0)

### Features

* **Run and compare command support dbt selector**: just like `dbt run -s <selector>`, you can use selector in the `run` and `compare` ([#739](https://github.com/InfuseAI/piperider/pull/739))
* **Add cloud config to local project config**: the default cloud project config is not set to project level instead of user level. It allow you to upload to different cloud project for different dbt repositories. ([#748](https://github.com/InfuseAI/piperider/pull/748))
* **Add metric hierarchy in the sidebar menu**: The project sidebar tree adds the metric hierarchy. ([#750](https://github.com/InfuseAI/piperider/pull/750) [#758](https://github.com/InfuseAI/piperider/pull/758))

### Fixes

* Make sure the dbt-helper working with the dbt-core 1.3 ([#740](https://github.com/InfuseAI/piperider/pull/740))
* Fix metric date range display ([#746](https://github.com/InfuseAI/piperider/pull/746))
* Fix required project’s target path ([#753](https://github.com/InfuseAI/piperider/pull/753))

## PipeRider OSS v0.27.0

Release date: [2023-06-15](https://github.com/InfuseAI/piperider/releases/tag/v0.27.0)

### Features

* Improvements to formatting and information contained in the **comparison summary markdown** for pull request comments
* PipeRider compare now profiles models based on piperider tags contained in the development/target branch
* Removed unnecessary files in the `.piperider/` when executing the PipeRider commands. Please refer to the following pull requests [#723](https://github.com/InfuseAI/piperider/pull/723) [#725](https://github.com/InfuseAI/piperider/pull/725) [#729](https://github.com/InfuseAI/piperider/pull/729)

### Fixes

* Fixed wrong source table name when users specify identifier in dbt schema [#724](https://github.com/InfuseAI/piperider/pull/724)
* If the run result and manifest do not match, it would cause an error [#726](https://github.com/InfuseAI/piperider/pull/726)
* Add warning to auto-upload when not logged in instead of raising an exception [#734](https://github.com/InfuseAI/piperider/pull/734)

## PipeRider OSS v0.26.2

Release date: [2023-06-07](https://github.com/InfuseAI/piperider/releases/tag/v0.26.2)

### Fixes

* Fix wrong source table name when users specify identifier in dbt schema

## PipeRider OSS v0.26.1

Release date: [2023-06-06](https://github.com/InfuseAI/piperider/releases/tag/v0.26.1)

### Fixes

* Fix PipeRider compare action helper for init-less flow

## PipeRider OSS v0.26.0

Release date: [2023-06-01](https://github.com/InfuseAI/piperider/releases/tag/v0.26.0)

### Features

* **Support for dry run and interactive mode**: The `piperider compare` command now includes the new options `--dry-run` and `--interactive`.
* **Improvements to the Report UI**: Sidebar enhancements including column orders, a modified state indicator, overview item, and hide column nodes for no-profiling models.

### Fixes

* Fixed an issue where the table list displayed incorrect entries.
* Addressed a key error in `compare-reports` when using the `--tables-from` option.
* Resolved an error that occurred when using the `--table` option in reports.

### Changes

* **dbt test results are now included by default:** It's no longer neccessary to use `--dbt-run-results`, but it can still be specified without error.

## PipeRider OSS v0.25.1

Release date: [2023-05-19](https://github.com/InfuseAI/piperider/releases/tag/v0.25.1)

* Bug fixes

## PipeRider OSS v0.25.0

Release date: [2023-05-18](https://github.com/InfuseAI/piperider/releases/tag/v0.25.0)

Increased focus on dbt project integration.

* It's no longer required to run `piperider init` inside dbt projects, just install PipeRider and run
* The report sidebar has been redesigned to follow closely with the layout of the dbt docs sidebar, making it easier to navigate for dbt users
* Tracking schema changes: Previously, the profiling was a one-time scan of models with tags, including metadata fetching and execution profiling. Now, in the new version, we have separated the metadata collection and profiling into two distinct phases:
  1. PipeRider will gather all metadata to track schema changes for models, seeds, and sources.
  2. PipeRider will continue to profile only those models that have been tagged for profiling.

#### Feature deprecation notice

The following features are now deprecated and will be removed from a future version of PipeRider:

* Using PipeRider in **non-dbt projects**
* **PipeRider assertions**

These features are still included in PipeRider for the time being, but are not recommended for continued use.

## PipeRider OSS v0.24.1

Release date: [2023-05-04](https://github.com/InfuseAI/piperider/releases/tag/v0.24.1)

Bug fix:

* PipeRider bug with PipeRider using the wrong schema when using the `--table` option

## PipeRider OSS v0.24.0

Release date: [2023-05-04](https://github.com/InfuseAI/piperider/releases/tag/v0.24.0)

Bug fixes, including:

* Fix issue when loading dbt project yaml
* Fix PiperRider Cloud report URL in CLI output

## PipeRider OSS v0.23.0

Release date: [2023-04-20](https://github.com/InfuseAI/piperider/releases/tag/v0.23.0)

* **AWS Athena Support**: PipeRider now supports querying and profiling data stored in AWS Athena.
* **Skipped model reasons**: We have added the ability to display the reason why certain models were skipped during profiling and metric queries, making it easier for users to understand and improve their data pipeline.
* **Compare Recipe Enhancements**:
  * Our Compare recipe has been enhanced to support both file and Jinja formats, allowing users to template values from environment variables for more dynamic comparisons.
  * The `compare` command now supports [comparing to a file](../get-started/compare.md#recipe-example-base-is-from-file). This means you can manually set the baseline to an existing report, and then use that in future comparisons without the need to run the baseline profile each time.
* **Other Enhancements and Bug Fixes**: We have also made several other enhancements and bug fixes, including the addition of buttons for GitHub, Discord, and documentation feedback, ensuring users have an easy and efficient way to provide feedback and suggestions for future improvements.

## PipeRider OSS v0.22.0

Release date: [2023-03-30](https://github.com/InfuseAI/piperider/releases/tag/v0.22.0)

* We are excited to announce the release of our new Compare GitHub Action! This powerful new feature allows you to easily compare code changes and send summary to your PR comment. For more information, please visit our [**GitHub Marketplace page**](https://github.com/marketplace/actions/piperider-compare-action).
* We have made some layout adjustments to our application to improve its responsiveness and overall user experience.
* Resolved an issue related to symbolic links on Windows.

## PipeRider OSS v0.21.0

Release date [03-16-2023](https://github.com/InfuseAI/piperider/releases/tag/v0.21.0)

* Changed to using 'three-dot compare' (instead of two-dot) when running `piperider compare`. Refer to the GitHub documentation for more information on [two-dot and three-dot Git diff comparisons](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-comparing-branches-in-pull-requests#three-dot-and-two-dot-git-diff-comparisons).

## PipeRider OSS v0.20.0

Release date: [2023-03-07](https://github.com/InfuseAI/piperider/releases/tag/v0.20.0)

* Improved the cloud signup process.
* `piperider compare` now supports PipeRider cloud.
