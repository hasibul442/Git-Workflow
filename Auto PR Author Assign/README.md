# PR Auto Assign Author GitHub Action

This GitHub Action automatically assigns the author of a pull request as an assignee when the pull request is opened. The configuration is defined in the `.github/workflows/auto-assign-author.yml` file.

## Purpose

The `PR Auto Assign Author` action simplifies the pull request workflow by automatically assigning the pull request's author as an assignee, ensuring they remain responsible for their pull request. This action skips Dependabot bot pull requests to avoid unnecessary assignments.

## Prerequisites

- A GitHub repository with write access to configure workflows.
- Basic familiarity with GitHub Actions and YAML configuration.

## Setup Instructions

Follow these steps to set up the PR auto-assign author action in your repository:

1. **Create the Workflow File**
   - Create a directory named `.github/workflows` in your repository if it doesn't already exist.
   - Add the following YAML configuration to a file named `auto-assign-author.yml` in the `.github/workflows` directory:

   ```yaml
   name: PR Auto Assign Author
   on:
     pull_request:
       types: [opened]
   jobs:
     assign:
       if: github.actor != 'dependabot[bot]'
       name: Assign author to PR
       runs-on: ubuntu-latest
       steps:
         - name: Assign author to PR
           uses: technote-space/assign-author@v1
   ```

2. **Commit and Push Changes**
   - Commit the `auto-assign-author.yml` workflow file to your repository's default branch (e.g., `main` or `master`).
   - Push the changes to GitHub.

3. **Verify the Action**
   - Open a new pull request in your repository (ensure it's not created by Dependabot).
   - Check the pull request to confirm that the author has been automatically assigned as an assignee.
   - If the author is not assigned, check the Actions tab in your repository to view the workflow logs for any errors.

## Configuration Details

- **Trigger**: The action runs when a pull request is opened (`on: pull_request: types: [opened]`).
- **Condition**: The action skips pull requests created by the Dependabot bot (`if: github.actor != 'dependabot[bot]'`).
- **Action Used**: The `technote-space/assign-author@v1` action assigns the pull request author as an assignee.

No additional configuration file is required for this action, as it uses the default settings of the `technote-space/assign-author` action.

## Troubleshooting

- **Author Not Assigned**: Ensure the pull request was not created by Dependabot, as the action skips Dependabot pull requests.
- **Workflow Not Triggered**: Verify that the `.github/workflows/auto-assign-author.yml` file is in the correct directory and has valid YAML syntax.
- **Permissions Issues**: Confirm that the GitHub Actions workflow has permission to assign assignees. You may need to adjust repository settings under Settings > Actions > General.

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [technote-space/assign-author](https://github.com/technote-space/assign-author)
- [Configuring a Workflow](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)

## License

This action is provided under the terms of the [MIT License](https://github.com/technote-space/assign-author/blob/master/LICENSE).