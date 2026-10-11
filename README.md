Telegram AI Customer Support Bot

An AI-powered customer support chatbot built with n8n, Google Gemini, and Telegram. The bot responds to customer messages in Bengali or English and provides friendly, helpful, and context-aware support.

Project Overview

This project demonstrates how to automate customer support using a Telegram bot connected to a Google Gemini AI model through an n8n workflow.

Users can send messages directly to Telegram and receive AI-generated responses without requiring a human to answer every basic question.

Features

AI-Powered Responses: Uses Google Gemini to understand questions and generate helpful answers.

Bengali and English Support: Responds in the same language as the user's message.

Telegram Integration: Receives customer messages and sends replies automatically.

Conversation Memory: Uses n8n Simple Memory to retain relevant context within a conversation.

Error Handling: Includes a fallback response path for AI processing failures.

Workflow Automation: Connects messaging, AI processing, and response delivery through n8n.

Workflow Architecture

Customer Message
       |
       v
Telegram Trigger
       |
       v
     AI Agent <---- Google Gemini Chat Model
       |
       +---- Success ----> Send Telegram Reply
       |
       +---- Error ------> Send Fallback Message

Simple Memory connects to the AI Agent
to provide conversation context.

Technology Stack

Technology

Purpose

n8n

Workflow automation

Telegram Bot API

Receiving and sending messages

Google Gemini

AI-powered response generation

Simple Memory

Conversation context

Docker

Running n8n locally

Cloudflare Tunnel

Exposing the local n8n instance for webhook access

Example

Customer (English):

Hello! Can you help me?

Bot:

Hello! I would be happy to help you. Please let me know what you need assistance with.

Customer (Bengali):

আমি বাংলা ভাষায় কথা বলতে চাই। আপনি কি সাহায্য করতে পারবেন?

Bot:

অবশ্যই! আমি আপনাকে বাংলা ভাষায় সাহায্য করতে পারবো। আপনার কী ধরনের সহায়তা প্রয়োজন?

Project Structure

Telegram-AI-Customer-Support-Bot/
├── workflow/
│   └── telegram-ai-customer-support-workflow.json
├── docs/
├── screenshots/
├── test-data/
└── README.md

Setup Requirements

Docker Desktop

n8n

A Telegram bot created using BotFather

Telegram API credentials configured in n8n

Google Gemini API key and credentials configured in n8n

Cloudflare Tunnel or another suitable HTTPS webhook endpoint

Setup Guide

Install and start Docker Desktop.

Run n8n locally.

Create a Telegram bot using BotFather.

Configure the Telegram credentials in n8n.

Configure Google Gemini credentials.

Import the workflow JSON file into n8n.

Verify the node connections and credentials.

Configure a reachable HTTPS webhook URL.

Publish the workflow and send a test message to the Telegram bot.

Detailed instructions will be provided in docs/setup.md.

Testing

The workflow has been tested with English and Bengali messages. Both tests returned successful AI-generated Telegram replies, and the corresponding n8n executions showed a successful status.

The fallback error path is configured but requires separate verification under an AI processing failure.

Security Notes

Never publish API keys, bot tokens, passwords, or other secrets.

Review the exported workflow JSON before uploading it to a public repository.

Configure credentials directly in your own n8n instance.

Do not commit private environment files or sensitive execution data.

Potential Use Cases

Small business customer support

Frequently asked questions

Bengali and English customer communication

Automated first-line support

AI chatbot prototypes

Future Improvements

Connect a business knowledge base using RAG.

Integrate Google Sheets or a CRM.

Add human-agent escalation.

Improve logging and monitoring.

Add configurable business-specific support instructions.

Author

AI Automation & Integration | n8n | AI Chatbots

This project is part of a practical portfolio focused on AI-powered workflow automation and chatbot integration.