---
description: Code review for data in dbt
---

# Introduction

PipeRider is a **data impact assessment tool for dbt data projects**, we call it “code review for data”. Using PipeRider, you are able to see the downstream impact of code changes so you can confidently merge changes, without breaking prod.

PipeRider provides column-level data comparison and lineage diffs that visualize the impact radius of your code-change with side-by-side data profiling statistics and impact summaries to help you review dbt project updates.

Features include:

* **PipeRider Data Profile Reports**\
  Perform **data reconnaissance** and get a holistic view of your data. Know what you’re dealing with before making changes.
*   **PipeRider Data Impact Report**

    See the **before-and-after impact** of your code changes, with lineage diff and data profile side-by-side comparison.
*   **Pull Request Impact Summary**

    Get an overview of the **points-of-impact** from of your code changes right in your pull-request comments. Includes deep-linking into your Code-Change Impact Report, so **code-reviewers can find more information** easily. Variations of Impact Summaries are available in the following places:

    * Command line
    * Impact Reports
    * Pull request/merge request comments

## How it works

1. **Zero-config installation** in your dbt project - just install and compare to get an Impact Report off your code changes.
2. **Compare code-changes** **during development** to make sure your code is having the desired impact.
3. **Automate it in your CI process**, with PipeRider’s GitHub Action, to get an Impact Summary in your pull request comment.&#x20;

## Getting Started

Find out how to [install PipeRider](get-started/install-piperider.md) and get your first Impact Report, or follow our [Quick Start](get-started/quick-start.md) for sample project that dbt’s Jaffle Shop.
