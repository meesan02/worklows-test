# worklows-test

This repository is for testing integrations with n8n. When a review comment is added on a Pull Request, an n8n workflow is automatically triggered.

## Features 

- **Automatic n8n Workflow Triggering**: When a review comment is added to a PR, the GitHub Actions workflow automatically triggers an n8n workflow via webhook.

## Setup

### Prerequisites

1. An n8n instance with a webhook node configured
2. The webhook URL from your n8n workflow

### Quick Start with Example Workflow

1. Import the example workflow into your n8n instance:
   - Copy the contents of `n8n-workflow-example.json`
   - In your n8n instance, go to Workflows > Import from File
   - Paste the JSON content
   - Activate the workflow
   - Copy the webhook URL from the Webhook node

2. Configure the GitHub repository:
   - Go to Settings > Secrets and variables > Actions
   - Add a new repository secret named `N8N_WEBHOOK_URL`
   - Paste your n8n webhook URL as the value

3. Test the integration:
   - Create a Pull Request in this repository
   - Add a review comment to the PR
   - Check the Actions tab to see the workflow run
   - Verify that your n8n workflow received the webhook call

### Manual Configuration

If you prefer to create your own n8n workflow:

1. In n8n, create a new workflow with a Webhook node
2. Configure the webhook to accept POST requests
3. Copy the webhook URL
4. Add the webhook URL as `N8N_WEBHOOK_URL` secret in GitHub

## How It Works

When a review comment is created on a PR:
1. The GitHub Actions workflow `.github/workflows/n8n-pr-review.yml` is triggered
2. The workflow collects information about the comment, PR, and author
3. A JSON payload is constructed with the event data
4. The payload is sent to the configured n8n webhook URL via HTTP POST
5. Your n8n workflow receives the data and can process it as needed

### Event Data

The webhook receives the following data:
- Event type and action
- Repository information
- PR number, title, and URL
- Comment ID, body, author, and creation time

## Workflow Details

The workflow triggers on the `pull_request_review_comment` event with the `created` action type, ensuring it only runs when new review comments are added to PRs.

## Files

- `.github/workflows/n8n-pr-review.yml` - GitHub Actions workflow
- `n8n-workflow-example.json` - Example n8n workflow to import
