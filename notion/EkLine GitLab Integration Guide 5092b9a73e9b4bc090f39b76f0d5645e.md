# EkLine GitLab Integration Guide

Created: April 3, 2023 12:40 AM

This guide will walk you through setting up the EkLine integration with GitLab. We'll use a **`.gitlab-ci.yml`** configuration file to run EkLine on your content directory and post the results as merge request discussions.

## **Prerequisites**

1. You have an EkLine API token provided by the EkLine contact person.
2. You have a GitLab account and a GitLab API token.

## **Step 1: Create `.gitlab-ci.yml` file**

Create a new file named **`.gitlab-ci.yml`** in your project's root directory and add the following content:

```yaml
stages:
  - ekline-pr-review

ekline-pr-review-job:
  stage: ekline-pr-review
  image: ghcr.io/ekline-io/ekline-ci-cd:v5.6.0
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
  script:
    - echo "Running EkLine"
  variables:
    INPUT_EK_TOKEN: $EK_TOKEN
    INPUT_GITLAB_TOKEN: $GITLAB_API_TOKEN
    INPUT_CONTENT_DIR: '<path_to_content_directory>'
    INPUT_REPORTER: 'gitlab-mr-discussion'
    INPUT_FILTER_MODE: 'added'
		INPUT_IGNORE_RULE: "EK00001,EK00004" # Optional
```

Replace **`<path_to_content_directory>`** with the actual path to your content directory.

## **Step 2: Configure GitLab API Token**

1. In GitLab, go to Settings > Access tokens.
2. Create a token with API access.
3. Add the token to your project's CI/CD > Variables as **`GITLAB_API_TOKEN`**.

## **Step 3: Configure EkLine API Token**

1. Obtain your **`EK_TOKEN`** from the EkLine contact person.
2. Add the token to your project's CI/CD > Variables as **`EK_TOKEN`**.

## **Step 4: Configure Input Reporter (Optional)**

You can choose between two options for **`INPUT_REPORTER`**:

- **`gitlab-mr-discussion`** (default): Reports results as merge request discussions.
- **`gitlab-mr-commit`**: Reports results to each commit in GitLab MergeRequest.

## **Step 5: Configure Input Filter Mode (Optional)**

The following options are available for **`INPUT_FILTER_MODE`**:

- **`added`** (default): Filter results by added/modified lines.
- **`diff_context`**: Filter results by diff context. i.e. changed lines +-N lines (N=3 for example).
- **`file`**: Filter results by added/modified file. i.e. EkLine will report results as long as they are in added/modified files even if the results are not in the actual diff.
- **`nofilter`**: Do not filter any results. Useful for posting results as comments as much as possible and check other results in the console at the same time.

## Step 6: Ignoring Specific Rules (Optional)

To ignore specific rules during the review process, you can use the `INPUT_IGNORE_RULE` flag. This flag accepts a comma-separated list of rule identifiers that you wish to skip.

For example, if you want to ignore rules `EK00001` and `EK00004`, you can set the `INPUT_IGNORE_RULE` flag in your configuration like this:

```
  INPUT_IGNORE_RULE: "EK00001,EK00004"
```

Once you have configured the options, save your changes and push the **`.gitlab-ci.yml`** file to your repository. Now, EkLine will run on your content directory and provide feedback as merge request discussions.