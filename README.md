# worklows-test

This repository is for testing integrations with n8n. When a review comment is added on a Pull Request, an n8n workflow is automatically triggered.

## Features

- **Automatic n8n Workflow Triggering**: When a review comment is added to a PR, the GitHub Actions workflow automatically triggers an n8n workflow via webhook.

## Setup

### Prerequisites

1. An n8n instance with a webhook node configured
2. The webhook URL from your n8n workflow

### Configuration

1. In your GitHub repository, go to Settings > Secrets and variables > Actions
2. Add a new repository secret named `N8N_WEBHOOK_URL`
3. Set the value to your n8n webhook URL (e.g., `https://your-n8n-instance.com/webhook/your-webhook-id`)

### How It Works

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

### Testing

To test the integration:
1. Create a Pull Request in this repository
2. Add a review comment to the PR
3. Check the Actions tab to see the workflow run
4. Verify that your n8n workflow received the webhook call

## Workflow Details

The workflow triggers on the `pull_request_review_comment` event with the `created` action type, ensuring it only runs when new review comments are added to PRs.