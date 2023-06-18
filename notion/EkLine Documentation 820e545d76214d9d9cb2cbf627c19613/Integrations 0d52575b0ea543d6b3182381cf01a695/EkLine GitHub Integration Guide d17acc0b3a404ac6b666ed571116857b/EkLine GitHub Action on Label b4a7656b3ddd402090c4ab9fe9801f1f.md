# EkLine GitHub Action on Label

In this guide, we will configure the EkLine GitHub Action to run only when a specific label, called "**ekline**", is added to a pull request.

## **Prerequisites**

- A GitHub repository with EkLine set up.
- The EkLine GitHub Action already configured in your repository.

## **Steps**

1. Open the **`.github/workflows/ekline.yml`** file in your GitHub repository.
2. Modify the **`on`** section of the GitHub action to trigger on **`pull_request`** with **`types: [labeled]`**:

```yaml
on:
  pull_request:
    types: [labeled]
```

1. Add a new **`if`** condition to the **`jobs`** section to check if the label "ekline" is present:

```yaml
jobs:
  test-pr-review:
    if: contains(github.event.label.name, 'ekline')
```

1. Save the changes to the **`ekline.yml`** file.

The updated **`ekline.yml`** should look like this:

```yaml
name: EkLine
on:
  pull_request:
    types: [labeled]

jobs:
  test-pr-review:
    if: contains(github.event.label.name, 'ekline')
    name: runner / EkLine Reviewer (github-pr-review)
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: ekline-io/ekline-github-action@v5.6.0
        with:
          content_dir: ./src/docs
          ek_token: ${{ secrets.ek_token }}
          github_token: ${{ secrets.github_token }}
          reporter: github-pr-review
          filter_mode: added
          ignore_rule: "EK00001,EK00004" # Optional
```

Now, the EkLine GitHub Action will only be triggered when a label named "ekline" is added to a pull request.

## **Usage**

To run the EkLine GitHub Action on a specific pull request, simply add the "ekline" label to the pull request. The action will then be triggered and run the checks as configured.