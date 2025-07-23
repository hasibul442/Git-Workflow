# PR Commit Messages GitHub Workflow

This GitHub Actions workflow automatically adds commit message summaries to Pull Request descriptions while preserving any existing content. It enhances PR documentation by providing a clear overview of all changes made.

## 🚀 Features

- **Automatic Commit Summary**: Adds a formatted list of all commit messages to PR descriptions
- **Content Preservation**: Maintains existing PR description content
- **Smart Positioning**: Places commit summary at the top, followed by original description
- **Real-time Updates**: Triggers when PRs are opened or updated with new commits
- **Non-destructive**: Preserves manual PR descriptions and comments

## 📋 How It Works

1. **Trigger Events**: The workflow activates when:
   - A new Pull Request is opened
   - New commits are pushed to an existing PR (synchronize event)

2. **Commit Collection**: Fetches all commits associated with the Pull Request using GitHub API

3. **Message Processing**:
   - Extracts the first line (title) of each commit message
   - Formats them as a bulleted list under "📝 Commit Summary" heading
   - Combines with existing PR description content

4. **Description Update**: Updates the PR description by:
   - Adding commit summary at the top
   - Preserving any existing description below
   - Maintaining proper formatting and spacing

## 🛠️ Installation

1. Create the workflow directory in your repository:
   ```
   .github/workflows/
   ```

2. Copy the `pr-commit-messages.yml` file to:
   ```
   .github/workflows/pr-commit-messages.yml
   ```

3. Commit and push the workflow file to your repository

## 📁 File Structure

```
.github/
└── workflows/
    └── pr-commit-messages.yml
```

## ⚙️ Configuration

The workflow uses the following configuration:

### Trigger Events
```yaml
on:
  pull_request:
    types: [opened, synchronize]
```

### Required Permissions
- The workflow uses `actions/github-script@v7` with access to:
  - Read repository content and pull requests
  - Update pull request descriptions
  - Access commit information via GitHub API

## 🔧 Customization Options

### Change Summary Format
Modify the commit summary format on line 24:
```javascript
// Current format
const body = `### 📝 Commit Summary:\n${messages}`;

// Alternative formats
const body = `## 📋 Changes Made:\n${messages}`;
const body = `**Commits in this PR:**\n${messages}`;
const body = `### 🔄 Updates:\n${messages}\n\n---`;
```

### Customize Commit Message Processing
Change how commit messages are extracted and formatted:
```javascript
// Current: Only first line of commit message
const messages = commits.data.map(commit => `- ${commit.commit.message.split('\n')[0]}`).join('\n');

// Alternative: Include commit hash
const messages = commits.data.map(commit => 
  `- \`${commit.sha.substring(0, 7)}\` ${commit.commit.message.split('\n')[0]}`
).join('\n');

// Alternative: Include author name
const messages = commits.data.map(commit => 
  `- ${commit.commit.message.split('\n')[0]} (by ${commit.commit.author.name})`
).join('\n');

// Alternative: Include full commit message
const messages = commits.data.map(commit => 
  `- **${commit.commit.message.split('\n')[0]}**\n  ${commit.commit.message.split('\n').slice(1).join('\n  ')}`
).join('\n\n');
```

### Change Content Positioning
Modify where the commit summary appears:
```javascript
// Current: Summary at top, existing content below
const newBody = body + '\n\n' + existingBody;

// Alternative: Existing content at top, summary below
const newBody = existingBody + '\n\n' + body;

// Alternative: Insert in middle with separator
const newBody = existingBody + '\n\n---\n\n' + body;
```

### Add Conditional Logic
Add conditions for when to update:
```javascript
// Only update if no existing body
if (!existingBody) {
  const newBody = body;
  // ... update logic
}

// Only update if commit summary doesn't already exist
if (!existingBody.includes('📝 Commit Summary:')) {
  const newBody = body + '\n\n' + existingBody;
  // ... update logic
}
```

## 📊 Example Output

### Before (Manual PR):
- **Description**: 
  ```
  This PR fixes the login bug and improves user experience.
  
  Please review the authentication changes.
  ```

### After (Auto-enhanced):
- **Description**:
  ```
  ### 📝 Commit Summary:
  - Add user input validation
  - Fix authentication token handling
  - Update login form styling
  - Add error message display

  This PR fixes the login bug and improves user experience.
  
  Please review the authentication changes.
  ```

### For Empty PR Description:
- **Description**:
  ```
  ### 📝 Commit Summary:
  - Add user input validation
  - Fix authentication token handling
  - Update login form styling
  - Add error message display
  ```

## 🔍 Troubleshooting

### Common Issues

1. **Workflow not triggering**:
   - Ensure the file is in `.github/workflows/` directory
   - Check file extension is `.yml` or `.yaml`
   - Verify workflow syntax is valid YAML

2. **Permission errors**:
   - Workflow uses default `GITHUB_TOKEN` with standard permissions
   - Ensure repository has Actions enabled
   - Check if branch protection rules affect the workflow

3. **Duplicate summaries**:
   - The workflow may add multiple summaries if triggered repeatedly
   - Consider adding logic to check for existing summaries

4. **Missing commits**:
   - Ensure PR has commits
   - Check if commits are properly associated with the PR

### Debugging

Add debugging information to the script:
```javascript
console.log(`Processing PR #${context.payload.pull_request.number}`);
console.log(`Found ${commits.data.length} commits`);
console.log('Existing body length:', existingBody.length);
console.log('Commit messages:', messages);
```

## 🔧 Advanced Features

### Add Commit Statistics
```javascript
const commitCount = commits.data.length;
const authors = [...new Set(commits.data.map(commit => commit.commit.author.name))];
const body = `### 📝 Commit Summary (${commitCount} commits by ${authors.length} author${authors.length > 1 ? 's' : ''}):\n${messages}`;
```

### Filter Specific Commit Types
```javascript
// Only include commits that start with specific keywords
const filteredCommits = commits.data.filter(commit => {
  const message = commit.commit.message.toLowerCase();
  return message.startsWith('feat:') || message.startsWith('fix:') || message.startsWith('docs:');
});
```

### Add Timestamp
```javascript
const timestamp = new Date().toISOString().split('T')[0];
const body = `### 📝 Commit Summary (Updated: ${timestamp}):\n${messages}`;
```

## 🤝 Contributing

Feel free to submit issues and enhancement requests!

## 📄 License

This workflow is provided as-is for educational and practical use.

## 🔗 Related Workflows

- **Auto PR Summary**: Complete PR title and description automation
- **Auto PR Author Assign**: Automatically assign PR authors
- **Auto Reviewer Assign**: Automatically assign reviewers to PRs

---

*This workflow enhances PR documentation by providing clear commit summaries while preserving existing content, making code reviews more efficient and informative.*
