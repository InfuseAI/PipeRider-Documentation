# Compare

The compare feature is to compare two runs and generate the comparison report.  You can use it to:

* **Compare the changes from base branch to pull request branch.** This is just the main use case of PipeRider
* **Compare the changes between two time points.** In the dbt daily or period job, you can run PipeRider after running dbt. It allows you to visualize the change for two time points later on.
* **Compare two data sources**. You can compare staging and production environments, or migration source and destination.

## Compare two runs

The most easy way to compare is to compare the last two runs

```
piperider compare-reports --last
```

You can also compare two runs by specifying two run JSON results

```
piperider compare-reports --base /tmp/base/run.json --target /tmp/target/run.json
```

### Comparison artifacts

Compare generate two artifacts are generated under the comparison output directory

* Compare HTML report (`index.html`)
* Compare Markdown summary (`summary.md`). This markdown file is used to post on your PR comment for review.

The default comparison output directory is located at `.piperider/comparisons/<datetime>/` . The latest comparison would also be sym-linked at `.piperider/outputs/latest`

You can use the `--output` to change the output directory

```
piperider compare-reports --last --output /tmp/mycompare
```

## Compare for pull requests

From [github document](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests)

> Pull requests let you tell others about changes you've pushed to a branch in a repository on GitHub. Once a pull request is opened, you can discuss and review the potential changes with collaborators and add follow-up commits before your changes are merged into the base branch.

In a dbt project, it is a challenge to visualize the data impact for a pull request. By using PipeRider, it streamline the process to understand the impact of the PR.&#x20;

### How to use

To compare a pull requests, you have to be in the branch to be merged, and run the command

```
piperider compare
```

It will run corresponding git, dbt, pipoerider operations and generate the comparison report.

### How it works

The pull request is to compare the change of `git diff <base>...<branch>`.  The following diagram illustrates the two commits which a pull request compares. Assuming our branch `features/add-my-dashboard` originates from `v1`, when a PR is created, it actually compares `v1.3` with `v1`, rather than `v1.3` with `v5`.

<figure><img src="../.gitbook/assets/compare-merge-base.png" alt=""><figcaption></figcaption></figure>

The steps  `piperider compare` executes are

1. Switch to the merge base of the current branch and main (or master), run dbt, and run PipeRider
2. Run dbt, and run PipeRider in the current working directory (working tree)
3. Compare these two runs

For the detailed commands, it would looks like this one

```sh
# Run dbt and piperider against the merge-base commit.
git archive -o /path/to/temp $(git merge-base main features/add-my-dashboard)
dbt deps
dbt build
piperider run

# Working tree
dbt deps
dbt build
piperider run

# Compare
piperider compare-reports --last
```

Please note that the comparison is made between the **working tree** and the **default base** (main or master). It differs slightly from `main....HEAD`

### Compare with the specific branch or git reference

If you have a specific branch other than the default base branch (`main` or the `master`), you can specify a different branch by using the `--base-branch` option.&#x20;

```
piperider compare --base-branch <branch-name>
```

Or any supported git reference in the first argument, such as HEAD, commit hash, or a tag.

```
piperider compare <git-ref>
```

For example:

```
piperider compare --base-branch develop
piperider compare develop
piperider compare HEAD
piperider compare 9ed4e5f
piperider compare v0.32.0
```

### Compare between two git references

In addition to comparing the current working tree with a reference, \
PipeRider can also perform comparisons of _any_ two commit references. To achieve this, PipeRider employs a three-dot notation similar to `git diff <commit>...<commit>`&#x20;

```
piperider compare <base-ref>...<target-ref>
```

For example:

```
piperider compare develop...feature/my-featur
piperider compare 9ed4e5f...06ec1ef
piperider compare feature/abc...v0.32.0
piperider compare HEAD~1...HEAD
```

The following table illustrates how `compare` works by using the `git diff` equivalents as an example.

| piperider compare                       | git                          |
| --------------------------------------- | ---------------------------- |
| piperider compare                       | git diff --merge-base main   |
| piperider compare --base-branch \<base> | git diff --merge-base base   |
| piperider compare \<base>               | git diff --merge-base base   |
| piperider compare \<base>...\<target>   | git diff \<base>...\<target> |

## Comparison Recipe

Comparison recipe is a yaml description to describe how to run a compare. Here is an example of a recipe

```yaml
base:
  ref: <git-ref>
  dbt:
    commands:
    - dbt deps
    - dbt build
  piperider:
    command: piperider run
target:
  dbt:
    commands:
    - dbt deps
    - dbt build
  piperider:
    command: piperider run
```

The recipe is automatically generated when you execute the `piperider compare` command. PipeRider will compare the current working tree with the base branch `<git-ref>`, which by default is `main` (or `master`).

```
piperider compare
```

### Custom Recipe

The recipe is defined at `.piperider/compare/<recipe>.yml`. To define a recipe, just add a yaml file with the recipe name. Run the recipe by specifying the recipe.

```
piperider compare --recipe <recipe>
```

### Recipe Example: Base is from file

A common practice is to provide a base run and then use the "compare" command to compare subsequent runs against this base.

To begin, you can save the output of the base run to `/tmp/base` using the following command:

```
piperider run -o /tmp/base
```

Then you prepare the comparison recipe at `.piperider/compare/compare_from_file.yml`

```yaml
# location: .piperider/compare/compare_from_file.yml
base:
  file: /tmp/base/run.json
target:
  dbt:
    commands:
    - dbt deps
    - dbt build
  piperider:
    command: piperider run

```

Next, you can use the following "compare" command, which will only run the target's dbt and Piperider, while the base results will come from the file:

```
piperider compare --recipe compare_from_file
```

### Recipe Example: Use environment variable

Continuing with the previous example, we can also use Jinja templates in the recipe. A common use case is to use environment variables. Here's an example recipe:

```yaml
base:
  file: "{{ env_var('BASE') }}"
target:
  dbt:
    commands:
    - dbt deps
    - dbt build
  piperider:
    command: piperider run

```

Later, when running the "compare" command, you can pass in your environment variables like this:

```
BASE=/tmp/base/run.json piperider compare --recipe compare_from_file
```

## **Simulating and interacting for troubleshooting**

To explore the simulation and interact with Piperider Compare, you can use `--dry-run` and `--interactive`.

The `--dry-run` option makes the Recipe executions do nothing but display all commands that might change your working directory.

The `--interactive` option makes the Recipe execution proceed to the next step only after receiving your confirmation by typing "yes". If you respond with "no", it will stop immediately.

## Github Action for Comparison Recipe

Running the recipe using GitHub Actions is a straightforward way to facilitate the review of pull requests in your GitHub repository.

You can use the following workflow to enable PipeRider comparison on pull requests with a minimal setup:

```
name: PR with PipeRider

on: [pull_request]

jobs:
  piperider-compare:
    runs-on: ubuntu-latest
    permissions:
      pull-requests: write
    steps:
    - uses: actions/checkout@v3

    - name: PipeRider Compare
      uses: InfuseAI/piperider-compare-action@v1
```

This workflow is triggered on every pull request and runs the `InfuseAI/piperider-compare-action`, which compares the current pull request with the target branch. It automatically installs any required packages and related connectors and provides a comment on the pull request with the comparison output. To use this feature, please grant the action **write** permission so that it can add the comparison output as a comment on the pull request.

![](<../.gitbook/assets/截圖 2023-09-18 下午3.33.34.png>)

For more information about the `InfuseAI/piperider-compare-action`, please visit our [GitHub repository](https://github.com/InfuseAI/piperider-compare-action).



