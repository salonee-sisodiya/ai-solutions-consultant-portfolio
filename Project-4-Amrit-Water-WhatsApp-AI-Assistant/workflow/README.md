# n8n Workflow

This folder contains the sanitized n8n workflow used to implement the Amrit Water WhatsApp AI Sales & Order Assistant.

## Workflow Purpose

The workflow connects WhatsApp customer messages with AI-powered order extraction, business-rule validation, order persistence, and customer confirmation.

## High-Level Flow

WhatsApp Customer Message
↓
Meta WhatsApp Cloud API
↓
n8n Webhook
↓
Message Processing
↓
AI Order Extraction
↓
Structured Order Data
↓
Business Rules & Validation
↓
Order Persistence
↓
Customer Confirmation
↓
Confirmed Order

## AI Responsibility

The AI layer is used primarily for:

- Understanding natural-language customer messages
- Identifying order intent
- Extracting structured order information
- Interpreting quantities and delivery details

Business-critical workflow decisions remain controlled by the n8n workflow rather than being left entirely to the language model.

## Included Workflow

`amrit-water-whatsapp-ai-assistant.json`

The workflow JSON has been sanitized for public portfolio use.

Credentials, access tokens, account-specific identifiers, and other sensitive configuration values have been removed or replaced with placeholders.

## Deployment Notes

To deploy the workflow, the required WhatsApp Cloud API and n8n credentials must be configured separately.

The included JSON is intended as a portfolio/reference implementation and should not be expected to run immediately after import without credential configuration.

## Security

No production access tokens, API keys, passwords, or active credentials are included in this repository.
