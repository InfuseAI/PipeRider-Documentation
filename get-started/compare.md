# Compare

The compare feature is to compare two runs and generate the comparison report.  You can use it to:

* **Compare models transformed by main branch and PR branch.** This is just the main use cse of PipeRider
* **Compare profiling result for two snapshots.** In the dbt daily or period job, you can run PipeRider after running dbt. It allows you to visualize the change for two time points later on.
* **Compare two data sources**. You can compare staging and production envronments, or migration source and destingation.

## Compare two runs

The most easy way to compare is to compare the last two runs

```
piperider compare-reports --last
```

You can also compare two runs by specifying two run JSON results

```
piperider compare-reports --base /tmp/base/run.json --target /tmp/target/run.json
```

You can show only partial tables of the comparison.

```
piperider compare-ports --last --tables-from target-only
```

## Compare artifacts

Just like [executing a run](run/), compare generate two artifacts are generated under the comparison output directory

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

In a dbt project, it is a challenge to visualize the data impact for a pull request. By using PipeRider, it streamline the process to understand the impact of the PR. Here is the general step

1. Switch to base branch, run dbt, and run PipeRider
2. Switch to PR branch, run dbt, and run PipeRider
3. Compare these two runs

For the detailed comments, it would looks like this one

```sh
# Main branch
git switch main
dbt deps
dbt build
piperider run

# PR branch
git switch features/my-awesome-feature
dbt deps
dbt build
piperider run

# Compare
piperider compare-reports --last
```

It's a batch of commands to run. In additions, there is a problem here. By default, the PR compare is use the three-way diff rather than two-way diff. From [github document](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-comparing-branches-in-pull-requests#three-dot-and-two-dot-git-diff-comparisons)

> There are two comparison methods for the `git diff` command; two-dot (`git diff A..B`) and three-dot (`git diff A...B`). By default, pull requests on GitHub show a three-dot diff.

So the better way is

```sh

# Merge base of base branch and PR barnch
git switch --detach $(git merge-base features/my-awesome-feature main)
dbt deps
dbt build
piperider run

# PR branch
git switch features/my-awesome-feature
dbt deps
dbt build
piperider run

# Compare
piperider compare-reports --last
```

To streamline the process, PipeRider has a higher-level command `piperider command` and it can run a comparison recipe.

## Comparison Recipe

Comparison recipe is a yaml description to describe how to run a compare. Here is an example of a recipe

```yaml
# .piperider/compare/default.yml
base:
  branch: main
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

The recipe is generated when initiating the PipeRider project, it will compare current branch with the main branch.

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

![](<../.gitbook/assets/image (3).png>)

For more information about the `InfuseAI/piperider-compare-action`, please visit our [GitHub repository](https://github.com/InfuseAI/piperider-compare-action).



