# GitHub PR Verification Bot

This repository contains GitHub Actions workflows to automate PR verification and reporting.
Test

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
- Required permissions for the GitHub token (default `GITHUB_TOKEN` is sufficient)

### Installation
1. Copy the workflow files to your `.github/workflows/` directory
2. Push the changes to your repository
3. The workflows will automatically run on the specified triggers

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
- Scheduled (configurable in the workflow file)
- Can be manually triggered

## 📊 Example PR Report
```
| PR # | Title | Creator | Merged | Status | PR Check Status |
|------|--------|---------|---------|---------|---------------------------|
| 42   | Update README | user1 | Yes | Closed | success (Rules followed) |
| 41   | Fix bug      | user2 | No  | Open   | pending (Verification required) |
```

## 📝 License
This project is open source and available under the [MIT License](LICENSE).
