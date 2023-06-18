# EkLine GitHub Integration Guide

Created: April 3, 2023 1:06 AM

This guide will walk you through using the EkLine GitHub Action for automated documentation review in your GitHub repositories. The EkLine GitHub Action helps you maintain high-quality documentation by seamlessly integrating with your existing GitHub workflow.

## **Prerequisites**

Before you begin, make sure you have the following:

- You have an EkLine API token provided by the EkLine contact person.

## **Step 1: Create a new GitHub Actions workflow**

In your GitHub repository, navigate to the "Actions" tab. Click on "New workflow" and choose the "set up a workflow yourself" option.

In the new workflow file, you can name it **`ekline.yml`**. This file will be stored in the **`.github/workflows/`** directory in your repository.

## **Step 2: Configure the EkLine GitHub Action**

Copy and paste the following template into your workflow file:

```yaml
name: EkLine
on:
  push:
    branches:
      - master
      - main
  pull_request:
jobs:
  test-pr-review:
    if: github.event_name == 'pull_request'
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

Adjust the configuration options as needed for your specific use case. The available options are:

- **`content_dir`**: The content directory relative to the root directory (default: **`.`**).
- **`ek_token`**: The token for the EkLine application (required).
- **`filter_mode`**: The filtering mode for the EkLine reviewer command [added, diff_context, file, nofilter] (default: **`added`**).
- **`github_token`**: The GITHUB_TOKEN (default: **`${{ secrets.github_token }}`**).
- **`reporter`**: The reporter of the EkLine review command [github-pr-check, github-check, github-pr-review] (default: **`github-pr-check`**).

### **Filter Modes**

You can control how the EkLine reviewer filters results by specifying the **`filter_mode`** option. Available filter modes are as below:

- **`added`** (default): Filter results by added/modified lines.
- **`diff_context`**: Filter results by diff context. i.e., changed lines +-N lines (N=3, for example).
- **`file`**: Filter results by added/modified file. i.e., EkLine reviewer will report results as long as they are in an added/modified file, even if the results are not in the actual diff.
- **`nofilter`**: Do not filter any results. Useful for posting results as comments as much as possible and checking other results in the console simultaneously.

### **Reporter Types**

- **`github-pr-check`**: Reports results to GitHub Checks.
- **`github-check`**: Similar to **`github-pr-check`**, but works for both pull requests and commits.
- **`github-pr-review`**: Reports results to GitHub PullRequest review comments using a GitHub Personal API Access Token.

### Ignoring Specific Rules

To ignore specific rules during the review process, you can use the `ignore_rule` flag. This flag accepts a comma-separated list of rule identifiers that you wish to skip.

For example, if you want to ignore rules `EK00001` and `EK00004`, you can set the `ignore_rule` flag in your configuration like this:

```
			  ignore_rule: "EK00001,EK00004"
```

## **Step 3: Save and commit the workflow file**

After configuring the workflow file, click on "Start commit" to save and commit the file to your repository. The EkLine GitHub Action will now run automatically when you push changes to the **`master`** or **`main`** branches or create a pull request.

## **Step 4: Add the EkLine API token to your repository secrets**

Before the action can work, you need to provide the EkLine API token. Go to your repository settings, navigate to the "Secrets" tab, and add a new secret with the name **`ek_token`**. Paste your EkLine API token as the value.

## **Step 5: Monitor the EkLine GitHub Action results**

Once the EkLine GitHub Action runs, you can view the results in the "Actions" tab of your repository. 

Now, you have successfully integrated the EkLine GitHub Action into your repository for automated documentation review. Enjoy better quality and consistency in your documentation!

### Other options:

[**EkLine GitHub Action on Label**](EkLine%20GitHub%20Integration%20Guide%20d17acc0b3a404ac6b666ed571116857b/EkLine%20GitHub%20Action%20on%20Label%20b4a7656b3ddd402090c4ab9fe9801f1f.md)