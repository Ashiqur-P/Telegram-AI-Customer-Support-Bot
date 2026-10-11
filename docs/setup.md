Setup Guide — Telegram AI Customer Support Bot

1. Prerequisites

Before setting up the project, ensure you have:

Docker Desktop

A working n8n instance

A Telegram account

A Telegram bot created through BotFather

A Google Gemini API key

Access to the n8n editor

2. Start n8n

If you already run n8n through Docker, start Docker Desktop and verify that your n8n container is running.

For a new installation, follow the official n8n Docker setup instructions:

https://docs.n8n.io/hosting/installation/docker/

Open the n8n editor in your browser. For a local installation, the default address used in this project is:

http://localhost:5678

3. Create a Telegram Bot

Open Telegram and search for @BotFather.

Send /newbot.

Follow the prompts to choose a bot name and username.

Copy the generated bot token and store it securely.

Security: Never publish the bot token or commit it to a public repository.

4. Configure Telegram Credentials

Open the n8n editor.

Create or select the Telegram API credentials.

Enter the bot token securely in the credential form.

Save the credentials.

Verify that the Telegram nodes use the correct credential.

5. Configure Google Gemini

Open Google AI Studio: https://aistudio.google.com/

Create or obtain a Gemini API key.

In n8n, configure the appropriate Google Gemini credentials.

Select the Google Gemini Chat Model node and verify its configuration.

Confirm that the AI Agent is connected to the model.

Keep the API key private and store it only in the appropriate credential configuration.

6. Import the Workflow

Open the n8n editor.

Import workflow/telegram-ai-customer-support-workflow.json.

Review the imported node connections.

Select your own Telegram and Google Gemini credentials.

Confirm that the AI Agent, Simple Memory, Telegram Trigger, reply node, and error-handling node are configured correctly.

Exported workflow files may contain credential references from the original environment. Configure your own credentials rather than relying on another user's credential IDs.

7. Configure the Webhook Endpoint

The Telegram Trigger needs a reachable HTTPS webhook endpoint.

For local testing, this project has used Cloudflare Quick Tunnel to expose the local n8n instance.

If you use a Quick Tunnel, keep the tunnel process running while testing. Its URL may change after restarting it.

For production, use a stable, secure deployment and webhook URL.

8. Activate and Test

Save the workflow.

Verify that all required credentials are valid.

Publish the workflow.

Open Telegram and send a test message to your bot.

Test both English and Bengali messages.

Open the n8n Executions view and verify that the execution succeeds.

Test the fallback path separately before relying on it in production.

When the workflow is published and the Telegram Trigger is active, you can test it by messaging the bot directly.

9. Troubleshooting

The bot does not respond

Confirm that the workflow is published.

Check that the Telegram Trigger is active.

Verify the Telegram credentials and webhook configuration.

Review the latest n8n execution.

Google Gemini returns an error

Check the Gemini credentials and API limits.

Review the model configuration.

Retry after a temporary service interruption.

Check the failed execution for the underlying error.

The workflow cannot register its webhook

Confirm that the HTTPS endpoint is reachable.

Check whether the tunnel process is still running.

Verify that the configured public URL has not changed.

Telegram reports a delivery error

Verify the destination chat ID.

Confirm that the bot can message the user.

Review the Telegram node's execution details.

10. Security Checklist

Never share Telegram bot tokens or Gemini API keys.

Do not commit credentials or private environment files.

Review the workflow JSON before public release.

Use your own credentials after importing the workflow.

Avoid including sensitive customer information in screenshots or test data.

11. Official References

n8n Docker installation: https://docs.n8n.io/hosting/installation/docker/

Telegram Bot API: https://core.telegram.org/bots/api

Google AI Studio: https://aistudio.google.com/