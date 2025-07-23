# Auto PR Summary GitHub Workflow

This GitHub Actions workflow automatically updates Pull Request titles and descriptions with commit message summaries whenever a PR is opened or updated.

## 🚀 Features

- **Automatic PR Title Update**: Sets the PR title to the latest commit message
- **Commit Summary Generation**: Creates a formatted list of all commit messages in the PR description
- **Real-time Updates**: Triggers on PR creation and when new commits are pushed
- **Zero Configuration**: Works out of the box with no additional setup required

## 📋 How It Works

1. **Trigger Events**: The workflow activates when:
   - A new Pull Request is opened
   - New commits are pushed to an existing PR (synchronize event)

2. **Commit Collection**: Fetches all commits associated with the Pull Request

3. **Content Generation**:
   - Extracts the first line of each commit message
   - Formats them as a bulleted list
   - Uses the latest commit message as the PR title

4. **PR Update**: Automatically updates the Pull Request with:
   - New title from the most recent commit
   - Description containing all commit summaries

## 🛠️ Installation

1. Create the workflow directory in your repository:
   ```
   .github/workflows/
   ```

2. Copy the `pr-auto-summary.yml` file to:
   ```
   .github/workflows/pr-auto-summary.yml
   ```

3. Commit and push the workflow file to your repository

## 📁 File Structure

```
.github/
└── workflows/
    └── pr-auto-summary.yml
```

## ⚙️ Configuration

The workflow uses the following configuration:

### Trigger Events
```yaml
on:
  pull_request:
    types: [opened, synchronize]
```

### Permissions Required
- The workflow uses `actions/github-script@v7` which has access to:
  - Read repository content
  - Update Pull Requests
  - Access commit information

## 🔧 Customization Options

### Change PR Title Logic
To modify how the PR title is generated, edit line 29:
```javascript
// Current: Uses the latest commit message
const newTitle = commits.data[commits.data.length - 1].commit.message.split('\n')[0];

// Alternative: Use the first commit message
const newTitle = commits.data[0].commit.message.split('\n')[0];

// Alternative: Use a custom format
const newTitle = `PR: ${commits.data[commits.data.length - 1].commit.message.split('\n')[0]}`;
```

### Customize Description Format
Modify the description template on line 28:
```javascript
// Current format
const newBody = `### 📝 Commit Summary:\n${fullCommitList}`;

// Alternative formats
const newBody = `## Changes Made:\n${fullCommitList}\n\n---\n*Auto-generated summary*`;
const newBody = `**Commits in this PR:**\n${fullCommitList}`;
```

### Add Additional Information
Extend the script to include more details:
```javascript
// Add commit count
const commitCount = commits.data.length;
const newBody = `### 📝 Commit Summary (${commitCount} commits):\n${fullCommitList}`;

// Add author information
const authors = [...new Set(commits.data.map(commit => commit.commit.author.name))];
const authorList = authors.join(', ');
const newBody = `### 📝 Commit Summary:\n${fullCommitList}\n\n**Authors:** ${authorList}`;
```

## 📊 Example Output

### Before (Manual PR):
- **Title**: "Update feature"
- **Description**: *(empty)*

### After (Auto-generated):
- **Title**: "Fix user authentication bug"
- **Description**:
  ```
  ### 📝 Commit Summary:
  - Add user validation middleware
  - Update authentication logic
  - Fix user authentication bug
  ```

## 🔍 Troubleshooting

### Common Issues

1. **Workflow not triggering**:
   - Ensure the file is in `.github/workflows/` directory
   - Check that the file has `.yml` or `.yaml` extension
   - Verify the workflow syntax is valid

2. **Permission errors**:
   - The workflow uses the default `GITHUB_TOKEN` which should have sufficient permissions
   - If using a private repository, ensure Actions are enabled

3. **Script errors**:
   - Check the Actions tab in your repository for detailed error logs
   - Ensure the repository has at least one commit in the PR

### Debugging

To add debugging information, modify the script:
```javascript
console.log(`Processing PR #${prNumber}`);
console.log(`Found ${commits.data.length} commits`);
console.log('Commit messages:', commitTitles);
```

## 🤝 Contributing

Feel free to submit issues and enhancement requests!

## 📄 License

This workflow is provided as-is for educational and practical use.

## 🔗 Related Workflows

- **Auto PR Author Assign**: Automatically assign PR authors
- **Auto Reviewer Assign**: Automatically assign reviewers to PRs
- **Deploy Workflows**: Various deployment automation workflows

---

*This workflow helps maintain consistent PR documentation and improves code review efficiency by providing clear commit summaries.*
