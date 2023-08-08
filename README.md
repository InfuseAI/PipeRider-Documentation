---
description: Data impact assessment for code-change in dbt projects
---

# Introduction

[PipeRider](https://github.com/infuseai/piperider) is a **data impact assessment tool for dbt data projects**, we call it “code review for data”. Using PipeRider, you are able to see the downstream impact of code changes so you can confidently merge changes, without breaking prod.

PipeRider provides data profile comparison and column lineage diffs that visualize the impact radius of your code-change with side-by-side data profiling statistics and impact summaries to help you review dbt project updates.

Features include:

* **PipeRider Data Profile Reports** - Perform **data reconnaissance** and get a holistic view of your data. Know what you’re dealing with before making changes.
* **PipeRider Data Impact Report** - See the **before-and-after** **impact** of your code changes in rich HTML reports. Includes column Lineage Diff when used with [PipeRider Cloud](broken-reference).
* **Pull Request Impact Summary** - Get an overview of the **points-of-impact** of your code changes right in pull-request comments. Includes deep-linking into your Impact Report so **code-reviewers can find out more information** easily.
  * Variations of Impact Summaries are available on the command line, in Impact Reports, and in pull request comments.

## How it works

1. **Zero-config installation** for dbt projects - Just install and compare to get an Impact Report from your code changes.
2. **Compare code-changes** **during development** to make sure your code is having the desired impact.
3. **Automate it in your CI process,** with [PipeRider’s official GitHub Action](https://github.com/marketplace/actions/piperider-compare-action), to get an Impact Summary in your pull request comment.&#x20;

## Getting Started

Find out how to [install PipeRider](get-started/install-piperider.md) and get your first Impact Report, or follow our [Quick Start](get-started/quick-start.md) for sample project that dbt’s Jaffle Shop.
