# Telegram AI Customer Support Bot

An AI-powered customer support chatbot built with **n8n**, **Google Gemini**, and **Telegram**. The bot responds to customer messages in Bengali or English and provides friendly, helpful, and context-aware support.

---

## 📌 Project Overview

This project demonstrates how to automate customer support using a Telegram bot connected to Google Gemini through an n8n workflow.

Users can send messages directly to Telegram and receive AI-generated responses. The workflow also handles non-text messages and AI processing failures with customer-friendly fallback messages and owner notifications.

## ✨ Features

- **AI-Powered Responses:** Uses Google Gemini to understand questions and generate helpful answers.
- **Bengali and English Support:** Responds in the same language as the user's message.
- **Telegram Integration:** Receives customer messages and sends replies automatically.
- **Conversation Memory:** Uses n8n Simple Memory to retain relevant context within a conversation.
- **Non-Text Message Handling:** Sends polite fallback replies for messages without text, including image, voice, and sticker messages.
- **Typing Indicator:** Displays a typing action while the AI response is being processed.
- **Customer-Facing Error Handling:** Sends a polite fallback message when Gemini processing fails.
- **Owner Error Notification:** Sends a separate Telegram error notification to the bot owner.

## 📐 Workflow Architecture

```text
Customer Message
       |
       v
Telegram Trigger
       |
       v
    Has text?
       |
       +---- False ----> Send non-text reply
       |
       +---- True
              |
              v
            Typing
              |
              v
           AI Agent <------ Google Gemini Chat Model
              |             (models/gemini-3-flash-preview)
              |             Temperature: 0.3
              |             Simple Memory
              |
              +---- Success ----> Send a text message
              |
              +---- Error ------> Send customer fallback reply
              |
              +---- Error ------> Notify owner
```

**Workflow notes:**

- The `Has text?` node separates text messages from messages without text.
- Any message without text (for example, image, voice, or sticker) is handled by `Send non-text reply`.
- Text messages proceed through the Typing node and AI Agent.
- The AI Agent uses Google Gemini and Simple Memory.
- On AI Agent failure, the Error output connects separately to the customer fallback and owner notification paths.

## 🛠️ Technology Stack

| Technology | Purpose |
| :--- | :--- |
| **n8n** | Workflow automation |
| **Telegram Bot API** | Receiving and sending messages |
| **Google Gemini** | AI-powered response generation |
| **Simple Memory** | Conversation context |
| **Docker** | Running n8n locally |
| **Cloudflare Tunnel** | Exposing the local n8n instance for webhook access |

## 🤖 Gemini Configuration

| Setting | Configuration |
| :--- | :--- |
| Model | `models/gemini-3-flash-preview` |
| Temperature | `0.3` |
| AI Integration | n8n AI Agent |
| Memory | n8n Simple Memory |

**Note:** `gemini-3-flash-preview` is a preview model. Model availability and naming may change over time.

## 💬 Conversation Examples

### Example 1 — English

**Customer:** Hello! Can you help me?

**Bot:** Hello! I would be happy to help you. Please let me know what you need assistance with.

### Example 2 — Bengali

**Customer:** আমি বাংলা ভাষায় কথা বলতে চাই। আপনি কি সাহায্য করতে পারবেন?

**Bot:** অবশ্যই! আমি আপনাকে বাংলা ভাষায় সাহায্য করতে পারবো। আপনার কী ধরনের সহায়তা প্রয়োজন?

### Example 3 — Non-Text Message

**Bot:** দুঃখিত, আমি এখন শুধু লিখিত বার্তা বুঝতে পারি। অনুগ্রহ করে আপনার প্রশ্নটি লিখে পাঠান।

## ⌨️ Typing Indicator

The workflow uses Telegram's typing action to let customers know that the bot is processing their message.

**Known limitation:** Telegram displays the typing indicator for approximately 5 seconds per action. It is not a continuous indicator for the entire AI processing duration.

## ⚠️ Error Handling

When Gemini processing fails:

- The AI Agent follows its Error output.
- One error-handling path sends a polite fallback message to the customer.
- A separate error-handling path sends a Telegram notification to the bot owner.
- The owner notification includes workflow execution information when available.

Example customer fallback:

> দুঃখিত, এই মুহূর্তে উত্তর দিতে পারছি না। কিছুক্ষণ পর আবার চেষ্টা করুন।

Example owner notification:

```text
⚠️ Bot error
Workflow: Telegram AI Customer Support Bot
Execution: [Execution ID]
Error: [Error message]
```

The fallback response confirms that the error-handling path can deliver a customer-facing message. It does not mean Gemini successfully generated an answer.

## 📁 Project Structure

```text
Telegram-AI-Customer-Support-Bot/
│
├── docs/
│   ├── architecture.md
│   └── setup.md
│
├── prompts/
│   └── system-prompt.txt
│
├── screenshots/
│   ├── 01-full-workflow.png
│   ├── 02-telegram-responses.png
│   ├── 03-telegram-memory-test.png
│   └── 04-successful-execution.png
│
├── test-data/
│   └── test-messages.json
│
├── test-results/
│   ├── day1-error-path.png
│   └── day2-error-alert.png
│
├── workflow/
│   ├── telegram-ai-customer-support-workflow-new.json
│   ├── telegram-ai-customer-support-workflow-working.json
│   ├── telegram-ai-customer-support-workflow.json
│   └── telegram-support-bot.json
│
├── .gitignore
└── README.md
```

## ⚙️ Setup Requirements

- Docker Desktop installed
- n8n running locally
- A Telegram bot created using BotFather
- Telegram API credentials configured in n8n
- Google Gemini API key and credentials configured in n8n
- Cloudflare Tunnel or another suitable HTTPS webhook endpoint

## 🚀 Setup Guide

1. Install and start Docker Desktop.
2. Run n8n locally.
3. Create a Telegram bot using BotFather.
4. Configure Telegram credentials in n8n.
5. Configure Google Gemini credentials in n8n.
6. Import the workflow JSON file into n8n.
7. Verify the node connections and credentials.
8. Configure a reachable HTTPS webhook endpoint.
9. Publish the workflow and send a test message to the Telegram bot.

**Known limitation:** The free Cloudflare Tunnel URL changes on each restart, so the webhook must be re-registered (set `WEBHOOK_URL`, then unpublish and publish the workflow).

## 🧪 Testing & Verification

### Day 1 — Basic Conversation and Error Handling

| Test Case | Result |
| :--- | :--- |
| English text message | PASS |
| Bengali text message | PASS |
| Gemini failure using an invalid API key | PASS |
| Customer-facing fallback message | PASS |
| Normal AI response after restoring credentials | PASS |

### Day 2 — Workflow Features and Error Notification

| Test Case | Result |
| :--- | :--- |
| Normal text message | PASS |
| Image message fallback | PASS |
| Voice message fallback | PASS |
| Sticker message fallback | PASS |
| Typing indicator | PASS |
| Owner Telegram error notification | PASS |
| Normal response after error handling tests | PASS |

These results document the completed Day 1 and Day 2 tests. They do not imply that every possible Telegram message type, network condition, or production scenario has been tested.

### Day 12 — RAG Integration and Human Handoff

| Test Case | Result |
| :--- | :--- |
| Bengali answerable question using RAG | PASS |
| English answerable question using RAG | PASS |
| Unanswerable question fallback | PASS |
| Support ticket creation in Google Sheets | PASS |
| Owner notification for new support ticket | PASS |
| English conversation memory | PASS |
| Non-text message fallback (previously verified) | PASS |

The RAG integration uses the separate Advanced RAG Knowledge Base Assistant workflow. Unanswerable questions are escalated through a Google Sheets support ticket and owner notification.

The English and Bengali answer tests confirm the tested examples only. They do not establish overall answer accuracy or production reliability.

## 🔒 Security Notes

- Never publish API keys, bot tokens, passwords, or other secrets.
- Review the exported workflow JSON before uploading it to a public repository.
- Configure credentials directly in your own n8n instance.
- Do not commit private environment files or sensitive execution data.
- Keep the Cloudflare Tunnel URL and other private configuration details out of the repository.
- Review screenshots before publishing and redact chat IDs, personal names, Telegram usernames, and other identifying information.
- Use dummy customer information in public test data.

## 💡 Potential Use Cases

- Small business customer support
- Frequently asked questions (FAQs) automation
- Bengali and English customer communication
- Automated first-line support
- AI chatbot prototypes

## 🔮 Future Improvements

Potential future improvements include:

- Connect a business knowledge base using **RAG (Retrieval-Augmented Generation)**.
- Integrate Google Sheets or a CRM.
- Add a human-agent escalation path.
- Improve logging and monitoring.
- Add configurable business-specific support instructions.
- Use a stable HTTPS endpoint instead of a temporary tunnel URL.

RAG knowledge base integration is not currently connected to this Telegram bot. These items are potential extensions, not completed features.

## ✍️ Author

**AI Automation & Integration** | n8n | AI Chatbots

This project is part of a practical portfolio focused on AI-powered workflow automation and chatbot integration.