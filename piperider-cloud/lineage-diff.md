---
description: View the Lineage Diff in comparison reports
---

# Lineage Diff

**Lineage Diff** is a PipeRider Cloud feature that enables you to compare the lineage of your dbt project before and after making code changes. Using a DAG (Directed Acyclic Graph), Lineage Diff highlights the models and metrics in your project that have been impacted by changes to your code or data.

## How to Use Lineage Diff

{% hint style="info" %}
This documentation assumes that you have already created a PipeRider Cloud account and **uploaded at least two reports** for your project
{% endhint %}

To open the lineage DAG, click the. ‘Show Graph’ button at the bottom right of the comparison report. The ‘Change Only’ view is shown by default, which shows impacted nodes.

<figure><img src="../.gitbook/assets/lineage-diff-docs.gif" alt=""><figcaption><p>View the Lineage Diff (lineage change) for your dbt project</p></figcaption></figure>

### Select nodes

Use the node selector at the bottom right of the lineage graph to switch between:

* All nodes - view the whole project (or that which was included in this PipeRider run).
* Change only - view only impacted nodes.
* Focus active - view the active node and its lineage (click a node to focus it).

<figure><img src="../.gitbook/assets/node-selector-fs8.png" alt=""><figcaption><p>Select which nodes to view using the node selector at the bottom right</p></figcaption></figure>

### Show stats

The following stats can be displayed for nodes:

* Row count
* Execution time (dbt build time)

<figure><img src="../.gitbook/assets/stat-selector-fs8.png" alt=""><figcaption><p>Display stats for nodes using the Stat selector at the bottom left</p></figcaption></figure>
