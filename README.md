# GitHub PR Verification Bot

This repository contains GitHub Actions workflows to automate PR verification and reporting.

## 📋 Features

### 1. PR Verification Workflow
- **Automatic Verification Requests**: Posts a verification question when a PR is opened or updated
- **Reviewer Interaction**: Allows reviewers to respond with `/answer [yes/no/n/a]`
- **Status Tracking**: Updates PR status based on reviewer responses
- **Re-verification on Updates**: Automatically requests re-verification when new commits are pushed

### 2. PR Reporting
- Generates a comprehensive report of all PRs with their statuses
- Tracks verification status and PR check results
- Provides an overview of merged and open PRs

## 🚀 Getting Started

### Prerequisites
- GitHub repository with Actions enabled
- Required repository permissions for the GitHub token (default `GITHUB_TOKEN` is sufficient):
  - `contents: read` (for reading workflow files)
  - `pull-requests: write` (for updating PR statuses)
  - `issues: write` (for posting comments)
  - `statuses: write` (for commit status updates)

### Installation
1. Copy the workflow files to your `.github/workflows/` directory
2. Push the changes to your repository
3. The workflows will automatically run on the specified triggers

## ⚙️ Configuration

You can customize the bot's messages by modifying the `env` section in `.github/workflows/pr-check.yml`. Here are the available configurations:

### Message Format
All message values must be valid JSON strings (properly escaped if they contain quotes or newlines). The messages support [GitHub Flavored Markdown](https://github.github.com/gfm/) for rich formatting. For example:

```yaml
env:
  MESSAGE_INITIAL_QUESTION: "### Code Review Question\nPlease verify the following rules were followed"
  MESSAGE_MERGE_BLOCKED: "🚫 **Merge Blocked**\n\nThis PR does not meet our [code standards](URL_TO_STANDARDS)."
```

### Available Configurations

## PR Check Configuration

You can customize the PR check comments by modifying the `env` section in `.github/workflows/pr-check.yml`:

### Initial Question & Options
- `MESSAGE_INITIAL_QUESTION`: The main question asked in PR comments
- `MESSAGE_OPTION_YES`: Text for the 'yes' option
- `MESSAGE_OPTION_NO`: Text for the 'no' option
- `MESSAGE_OPTION_NA`: Text for the 'n/a' option

### Status Messages
- `STATUS_PENDING`: Message shown when waiting for review
- `STATUS_SYNC`: Message shown when new changes are detected
- `STATUS_SUCCESS`: Message for successful verification
- `STATUS_FAILURE`: Message for failed verification
- `STATUS_NA`: Message when rules don't apply

### Event-based Messages
- `MESSAGE_NEW_VERIFICATION`: Message shown when new commits are pushed
- `MESSAGE_REOPENED`: Message shown when a PR is reopened
- `MESSAGE_MERGE_BLOCKED`: Message shown when merge is blocked due to failed verification

## PR Report Configuration

You can customize the PR report generation by modifying the `env` section in `.github/workflows/pr-report.yml`:

### Report Structure
- `REPORT_TITLE`: Main title of the report
- `REPORT_TABLE_HEADER`: Column headers for the report table
- `REPORT_TABLE_DIVIDER`: Divider line for the report table

### Status Messages
- `PR_STATUS_NOT_CHECKED`: Text shown when PR check hasn't been performed
- `PR_STATUS_RULES_DONT_APPLY`: Text shown when rules don't apply to a PR
- `PR_STATUS_OPEN`: Text for open PRs
- `PR_STATUS_MERGED_YES`: Text indicating PR was merged
- `PR_STATUS_MERGED_NO`: Text indicating PR was not merged
- `PR_STATUS_UNKNOWN`: Fallback text for unknown values

### Formatting
- `DATE_FORMAT`: Locale for date formatting (e.g., 'en-US', 'nb-NO')
  ```yaml
  # Example formats:
  # 'en-US': 5/27/2025
  # 'nb-NO': 27.05.2025
  # 'en-GB': 27/05/2025
  # 'ja-JP': 2025/05/27
  ```

### Issue Settings
- `ISSUE_LABEL`: Label for generated report issues
- `ISSUE_TITLE_PREFIX`: Prefix for report issue titles

## 🔒 Branch Protection Rules

To ensure PR checks must pass before merging, set up branch protection rules in your GitHub repository after you have set up the workflows:

1. Go to your repository on GitHub
2. Click on `Settings` > `Branches`
3. Click `Add rule` to create a new rule
4. In the "Branch name pattern" field, enter your branch name (e.g., `main`)
5. Select these required options as a minimum:
   - ✅ Require a pull request before merging
   - ✅ Require status checks to pass before merging
   - Under "Require status checks to pass before merging":
     - Select `PR-Check` from the list of status checks
6. Click `Create` to save the rules

Now, pull requests to the protected branch will be blocked until all required status checks pass.


## 🤖 Available Commands

### For Reviewers
- `/answer yes` - Approve the verification
- `/answer no` - Request changes
- `/answer n/a` - Rules do not apply to this PR

## 🔄 Workflow Triggers

### PR Verification
- On PR open
- On new commits to PR
- On reviewer comments with `/answer` command

### PR Report
- Can be manually triggered using the GitHub Actions UI or API
- Scheduled (can be added in the workflow file)
  ```yaml
  # In .github/workflows/pr-report.yml
  on:
    workflow_dispatch:  # Manual trigger
    schedule:
      # Run at 9 AM (UTC) every weekday (Monday-Friday)
      - cron: '0 9 * * 1-5'
      # Run at 4 PM (UTC) every weekday (Monday-Friday)
      - cron: '0 16 * * 1-5'
  ```

## 📊 Example PR Report
```
| PR # | Title | Creator | Merged | Status | PR Check Status |
|------|--------|---------|---------|---------|---------------------------|
| 42   | Update README | user1 | Yes | Closed | success (Rules followed) |
| 41   | Fix bug      | user2 | No  | Open   | pending (Verification required) |
```

## 🛠️ Troubleshooting

### Common Issues

#### PR Check Not Triggering
- Ensure the workflow file is in the `.github/workflows/` directory
- Check that the workflow has the correct permissions in the repository settings
- Verify that the PR is not from a forked repository (GitHub has restrictions on workflows from forks)

#### Report Not Generating
- Check the workflow run logs for any error messages
- Ensure the GitHub token has the necessary permissions
- Verify that the `ISSUE_LABEL` in the workflow matches an existing label in your repository

#### Invalid Command Responses
- Make sure to use the exact command format: `/answer [yes|no|n/a]`
- Check for any leading/trailing spaces in the command
- The bot will respond with an error message if the format is incorrect

### Checking Logs
All workflow runs generate detailed logs that can help with troubleshooting:
1. Go to the "Actions" tab in your repository
2. Click on the workflow run you want to investigate
3. Click on the job to see the detailed logs
4. Look for any error messages or warnings in red

## 📦 Version Information

### v0.1.0
- Initial release
- PR Check workflow with configurable messages
- PR Report generator with issue tracking
