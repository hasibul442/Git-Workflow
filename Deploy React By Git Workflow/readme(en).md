# Deploy to cPanel GitHub Action

This GitHub Action automates the deployment of a project to a cPanel server whenever code is pushed to the `main` branch. The configuration is defined in the `.github/workflows/deploy-to-cpanel.yml` file.

## Purpose

The `Deploy to cPanel` action streamlines the process of building and deploying a web project to a cPanel server, ensuring that updates to the `main` branch are automatically reflected on the server.

## Prerequisites

- A GitHub repository with write access to configure workflows.
- A cPanel hosting account with FTP access enabled.
- A Node.js-based project with a build process (e.g., using `npm`).
- Basic familiarity with GitHub Actions, YAML configuration, and FTP credentials setup.

## Setup Instructions

Follow these steps to set up the Deploy to cPanel action in your repository:

1. **Create the Workflow File**
   - Create a directory named `.github/workflows` in your repository if it doesn't already exist.
   - Add the following YAML configuration to a file named `deploy-to-cpanel.yml` in the `.github/workflows` directory:

   ```yaml
   name: Deploy to cPanel
   on:
     push:
       branches:
         - main
   jobs:
     deploy:
       runs-on: ubuntu-latest
       steps:
       - name: Checkout code
         uses: actions/checkout@v2
       - name: Build project
         run: |
           npm install
           npm run build
       - name: Deploy to cPanel
         uses: SamKirkland/FTP-Deploy-Action@4.0.0
         with:
           server: ${{ secrets.FTP_SERVER }}
           username: ${{ secrets.FTP_USERNAME }}
           password: ${{ secrets.FTP_PASSWORD }}
           server-dir: public_html/
           local-dir: ./dist/
   ```

2. **Set Up GitHub Secrets**
   - Go to your repository on GitHub and navigate to **Settings > Secrets and variables > Actions > Secrets**.
   - Add the following secrets:
     - `FTP_SERVER`: Your cPanel FTP server address (e.g., `ftp.yourdomain.com` or an IP address).
     - `FTP_USERNAME`: Your cPanel FTP username.
     - `FTP_PASSWORD`: Your cPanel FTP password.
   - Ensure these credentials have the necessary permissions to access the `public_html/` directory (or your specified deployment directory).

3. **Verify Project Build**
   - Ensure your project has a `package.json` file with `npm install` and `npm run build` scripts configured.
   - The build output should be generated in the `./dist/` directory (or update the `local-dir` in the workflow file to match your build output directory).

4. **Commit and Push Changes**
   - Commit the `deploy-to-cpanel.yml` workflow file to your repository's `main` branch.
   - Push the changes to GitHub to trigger the workflow.

5. **Verify the Deployment**
   - After pushing to the `main` branch, check the **Actions** tab in your repository to monitor the workflow execution.
   - Verify that the built files in the `./dist/` directory are successfully uploaded to the `public_html/` directory on your cPanel server.
   - Visit your website to confirm that the deployed changes are live.

## Configuration Details

- **Trigger**: The action runs on push events to the `main` branch (`on: push: branches: - main`).
- **Checkout Code**: Uses `actions/checkout@v2` to retrieve the repository code.
- **Build Project**: Runs `npm install` to install dependencies and `npm run build` to generate the build output in the `./dist/` directory.
- **Deploy to cPanel**: Uses `SamKirkland/FTP-Deploy-Action@4.0.0` to upload the contents of `./dist/` to the `public_html/` directory on the cPanel server via FTP.
- **Secrets**: The action uses GitHub Secrets (`FTP_SERVER`, `FTP_USERNAME`, `FTP_PASSWORD`) to securely store sensitive FTP credentials.

## Customization Options

- **Change Build Directory**: If your build output is in a different directory (e.g., `build/` instead of `dist/`), update the `local-dir` parameter in the workflow file:
  ```yaml
  local-dir: ./build/
  ```

- **Change Deployment Directory**: If you want to deploy to a different cPanel directory (e.g., a subdirectory like `public_html/myapp/`), update the `server-dir` parameter:
  ```yaml
  server-dir: public_html/myapp/
  ```

- **Additional Build Steps**: Modify the `Build project` step to include additional commands if your project requires a different build process (e.g., for a different framework like Yarn or Vite).

For more configuration options, refer to the [SamKirkland/FTP-Deploy-Action documentation](https://github.com/SamKirkland/FTP-Deploy-Action).

## Troubleshooting

- **Deployment Fails**: Check the workflow logs in the GitHub Actions tab for errors. Common issues include:
  - Incorrect FTP credentials in GitHub Secrets.
  - Wrong `server-dir` or `local-dir` paths.
  - Missing `package.json` or invalid build scripts.
- **Files Not Updating**: Ensure the `server-dir` matches the correct cPanel directory and that the FTP user has write permissions.
- **Workflow Not Triggered**: Verify that the `.github/workflows/deploy-to-cpanel.yml` file is in the correct directory and has valid YAML syntax.
- **Permissions Issues**: Confirm that the GitHub Actions workflow has permission to access the repository and that the FTP credentials have write access to the target directory.

## Security Considerations

- Store FTP credentials securely in GitHub Secrets and avoid hardcoding them in the workflow file.
- Use a dedicated FTP user with limited permissions for deployments to minimize security risks.
- Regularly rotate FTP credentials to maintain security.

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [SamKirkland/FTP-Deploy-Action](https://github.com/SamKirkland/FTP-Deploy-Action)
- [Configuring a Workflow](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions)
- [Managing GitHub Secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets)
