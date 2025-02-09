# Create Bit Lane for each Git Branch for CI/CD Pipelines
For each new Branch in Git, a Bit lane is created in **bit.cloud**.

# GitHub Actions

This task creates a Bit lane for each Git Branch. As the next step in your pipeline, use the `bit-tasks/commit-bitmap@v1` to update the `.Bitmap` file.

## Inputs

### `ws-dir`

**Optional** The workspace directory path from the root. Default `"Dir specified in Init Task or ./"`.

## Example usage

**Note:** Use `bit-task/init@v1` as a prior step in your pipeline before running `bit-tasks/branch-lane@v1`. As the next step, use the `bit-tasks/commit-bitmap@v1` to update the `.Bitmap` file.

```yaml
name: Test Bit Branch Lane
on:
  push:
    branches-ignore:
      - main # or your default branch
permissions:
  contents: write
jobs:
  build:
    runs-on: ubuntu-latest
    env:
      GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      GIT_USER_NAME: ${{ secrets.GIT_USER_NAME }}
      GIT_USER_EMAIL: ${{ secrets.GIT_USER_EMAIL }}
      BIT_CLOUD_ACCESS_TOKEN: ${{ secrets.BIT_CLOUD_ACCESS_TOKEN }}
    steps:
      - name: Checkout repository
        uses: actions/checkout@v3
      - name: Initialize Bit
        uses: bit-tasks/init@v1
        with:
          ws-dir: '<WORKSPACE_DIR_PATH>'
      - name: Bit Branch Lane
        uses: bit-tasks/branch-lane@v1
      - name: Bit Commit Bitmap
        uses: bit-tasks/commit-bitmap@v1
```

# Contributor Guide

Steps to create custom tasks in different CI/CD platforms.

## GitHub Actions

Go to the GithHub action task directory and build using NCC compiler. For example;

```
npm install
npm run build
git commit -m "Update task"
git tag -a -m "action release" v1 --force
git push --follow-tags
```

For more information, refer to [Create a javascript action](https://docs.github.com/en/actions/creating-actions/creating-a-javascript-action)
