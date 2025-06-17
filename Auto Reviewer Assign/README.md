# Auto Assign Reviewer GitHub Action

This GitHub Action automatically assigns reviewers to pull requests in your repository. The configuration is defined in the `.github/workflows/auto-assign.yml` file.

## Purpose

The `Auto Assign Reviewer` action streamlines the code review process by automatically assigning reviewers when a pull request is opened, reducing manual effort and ensuring timely reviews.

## Prerequisites

- A GitHub repository with write access to configure workflows.
- A team or individual contributors set up as collaborators in the repository.
- Basic familiarity with GitHub Actions and YAML configuration.

## Setup Instructions

Follow these steps to set up the auto-assign reviewer action in your repository:

1. **Create the Workflow File**
   - Create a directory named `.github/workflows` in your repository if it doesn't already exist.
   - Add the following YAML configuration to a file named `auto-assign.yml` in the `.github/workflows` directory:

   ```yaml
   name: Auto Assign Reviewer
   on: pull_request
   jobs:
     add-reviewer:
       runs-on: ubuntu-latest
       steps:
         - name: Auto Assign
           uses: kentaro-m/auto-assign-action@v2.0.0
   ```

2. **Configure Reviewer Assignment**
   - By default, the `kentaro-m/auto-assign-action` uses a configuration file to determine which reviewers to assign. Create a file named `.github/auto-assign.yml` in your repository with the following example content:

   ```yaml
   addReviewers: true
   reviewers:
     - reviewer1
     - reviewer2
     - reviewer3
   numberOfReviewers: 1
   ```

   - Replace `reviewer1`, `reviewer2`, and `reviewer3` with the GitHub usernames of the reviewers you want to assign.
   - Adjust `numberOfReviewers` to specify how many reviewers should be assigned to each pull request (e.g., `1` for one reviewer, `2` for two, etc.).
   - For more advanced configuration options, refer to the [kentaro-m/auto-assign-action documentation](https://github.com/kentaro-m/auto-assign-action).

3. **Commit and Push Changes**
   - Commit the `auto-assign.yml` workflow file and the `.github/auto-assign.yml` configuration file to your repository's default branch (e.g., `main` or `master`).
   - Push the changes to GitHub.

4. **Verify the Action**
   - Open a new pull request in your repository.
   - Check the pull request to confirm that the specified reviewers have been automatically assigned.
   - If reviewers are not assigned, check the Actions tab in your repository to view the workflow logs for any errors.

## Configuration Options

The `.github/auto-assign.yml` file supports additional settings to customize reviewer assignment. Below are some common options:

- `skipKeywords`: Skip assigning reviewers if the pull request title contains specific keywords.
  ```yaml
  skipKeywords:
    - "[WIP]"
    - "[DRAFT]"
  ```

- `useReviewGroups`: Assign reviewers from predefined groups.
  ```yaml
  useReviewGroups: true
  reviewGroups:
    group1:
      - reviewer1
      - reviewer2
    group2:
      - reviewer3
      - reviewer4
  ```

- `addAssignees`: Automatically add reviewers as assignees.
  ```yaml
  addAssignees: true
  ```

For a complete list of configuration options, see the [official documentation](https://github.com/kentaro-m/auto-assign-action).

## Troubleshooting

- **Reviewers Not Assigned**: Ensure the GitHub usernames in `.github/auto-assign.yml` are correct and that the users have read access to the repository.
- **Workflow Not Triggered**: Verify that the `.github/workflows/auto-assign.yml` file is in the correct directory and has valid YAML syntax.
- **Permissions Issues**: Confirm that the GitHub Actions workflow has permission to assign reviewers. You may need to adjust repository settings under Settings > Actions > General.

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [kentaro-m/auto-assign-action](https://github.com/kentaro-m/auto-assign-action)
- [Configuring a Workflow](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)

