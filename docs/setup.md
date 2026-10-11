# Setup Guide — Telegram AI Customer Support Bot

## 1. Prerequisites
Before setting up the project, ensure you have:
* **Docker Desktop** installed and running
* A working **n8n** instance
* An active **Telegram account**
* A **Telegram bot** created through BotFather
* A **Google Gemini API key**
* Access to the **n8n editor**

---

## 2. Start n8n
* If you already run n8n through Docker, start **Docker Desktop** and verify that your n8n container is running.
* For a new installation, follow the official n8n Docker setup instructions:
  👉 [n8n Docker Installation Guide](https://docs.n8n.io/hosting/installation/docker/)
* Open the n8n editor in your browser. For a local installation, the default address used in this project is:
  👉 `http://localhost:5678`

---

## 3. Create a Telegram Bot
1. Open Telegram and search for **@BotFather**.
2. Send the command `/newbot`.
3. Follow the prompts to choose a **bot name** and **username**.
4. **Copy the generated bot token** and store it securely.

> 🔒 **Security Note:** Never publish the bot token or commit it to a public repository.

---

## 4. Configure Telegram Credentials
1. Open the **n8n editor**.
2. Create or select the **Telegram API** credentials.
3. Enter your **bot token** securely in the credential form.
4. Save the credentials.
5. Verify that your Telegram nodes are using this correct credential.

---

## 5. Configure Google Gemini
1. Open [Google AI Studio](https://aistudio.google.com/).
2. Create or obtain a **Gemini API key**.
3. In n8n, configure the appropriate **Google Gemini credentials**.
4. Select the **Google Gemini Chat Model** node and verify its configuration.
5. Confirm that the **AI Agent** is properly connected to the model.

> 🔒 **Security Note:** Keep the API key private and store it only inside n8n's credential configuration.

---

## 6. Import the Workflow
1. Open the **n8n editor**.
2. Import the file: `workflow/telegram-ai-customer-support-workflow.json`.
3. Review the imported node connections.
4. Select your own **Telegram** and **Google Gemini** credentials for the respective nodes.
5. Confirm that the **AI Agent**, **Simple Memory**, **Telegram Trigger**, **reply node**, and **error-handling node** are configured correctly.

> ⚠️ *Note: Exported workflow files may contain credential references from the original environment. Always assign your own credentials rather than relying on default credential IDs.*

---

## 7. Configure the Webhook Endpoint
* The **Telegram Trigger** node requires a reachable **HTTPS webhook endpoint** to receive messages.
* For local testing, this project uses **Cloudflare Quick Tunnel** to expose the local n8n instance to the web.
* If you use a Quick Tunnel, keep the tunnel process running while testing. Remember that its URL may change after restarting.
* For production deployments, always use a **stable and secure webhook URL**.

---

## 8. Activate and Test
1. **Save** the workflow.
2. Verify that all required credentials are valid.
3. **Publish / Activate** the workflow.
4. Open Telegram and send a test message to your bot.
5. Test the bot using both **English** and **Bengali** messages.
6. Open the n8n **Executions view** and verify that the execution log shows a success status.

> 💡 *Tip: Test the fallback/error path separately (e.g., by temporarily disconnecting the AI model) before relying on it in production.*

---

## 9. Troubleshooting

### 🔴 The bot does not respond
* Confirm that the workflow is fully **published/activated**.
* Check that the **Telegram Trigger** node is active.
* Verify your Telegram credentials and webhook configuration.
* Review the latest n8n execution logs for errors.

### 🔴 Google Gemini returns an error
* Check your Gemini credentials and API rate limits.
* Review the model configuration inside the node.
* Retry after a temporary service interruption.
* Check the failed execution details for the underlying error message.

### 🔴 The workflow cannot register its webhook
* Confirm that your HTTPS endpoint is publicly reachable.
* Check whether your tunnel process (e.g., Cloudflare) is still running.
* Verify that the configured public webhook URL has not changed.

### 🔴 Telegram reports a delivery error
* Verify the destination **chat ID**.
* Confirm that the target user has started a conversation with the bot first.
* Review the Telegram node's execution details for specific API error codes.

---

## 10. Security Checklist
* [ ] Never share Telegram bot tokens or Gemini API keys in plain text.
* [ ] Do not commit credentials, `.env` files, or private environment files to Git.
* [ ] Review the workflow JSON file to remove sensitive data before public release.
* [ ] Always use your own local credentials after importing a workflow.
* [ ] Avoid including sensitive customer information or chat logs in screenshots/test data.

---

## 11. Official References
* **n8n Docker Installation:** [n8n Docs](https://docs.n8n.io/hosting/installation/docker/)
* **Telegram Bot API:** [Telegram Core](https://core.telegram.org/bots/api)
* **Google AI Studio:** [Google Developers](https://aistudio.google.com/)