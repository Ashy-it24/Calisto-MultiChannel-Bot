# Calisto MultiChannel Bot

Calisto is a multi-channel chatbot project for Calisto Eyewear. It combines a Rasa natural-language processing service with a TypeScript integration layer for WhatsApp, Instagram, Messenger, CRM, Google Sheets, and email.

## Components

### Rasa NLP Service

The `calisto_nlp_export/` directory contains the Rasa assistant:

- Intent and entity training data
- Conversation stories and rules
- Custom action server
- Domain, policies, credentials, and endpoints
- Pre-trained Rasa model
- Docker and local startup configuration

Rasa documentation and run instructions are available in [calisto_nlp_export/README.md](calisto_nlp_export/README.md).

### Channel Integration Service

The `chatbot-integrations/` directory contains the Node.js/TypeScript integration server:

- WhatsApp, Instagram, and Messenger clients
- Express webhook routes
- HubSpot CRM integration
- Google Sheets data integration
- Gmail notifications
- Shared message types and utilities

Integration documentation and usage examples are available in [chatbot-integrations/README.md](chatbot-integrations/README.md).

## Architecture

```text
Messaging Channels
  (WhatsApp, Instagram, Messenger)
              |
              v
TypeScript Integration Server
  - Webhook handling
  - Message formatting
  - CRM and notification services
              |
              v
Rasa NLP and Action Servers
  - Intent understanding
  - Conversation management
  - Custom Calisto actions
```

## Prerequisites

- Node.js 18 or later
- npm
- Python 3.10
- Docker and Docker Compose, for the containerized Rasa setup

## Quick Start

### Run the Rasa NLP service

From the repository root:

```bash
cd calisto_nlp_export
docker compose up -d
```

The services are available at:

- Rasa REST API: `http://localhost:5005`
- Rasa action server: `http://localhost:5055`

Stop them with:

```bash
docker compose down
```

### Run the integration server

Create the integration environment file and add the required credentials:

```bash
cd chatbot-integrations
cp .env.example .env
npm install
npm run build
npm start
```

The integration server normally listens on port `3000`.

Webhook endpoints:

- `GET /health`
- `POST /webhooks/whatsapp`
- `POST /webhooks/instagram`
- `POST /webhooks/messenger`

## Test the Rasa API

```bash
curl -X POST http://localhost:5005/webhooks/rest/webhook \
  -H "Content-Type: application/json" \
  -d '{"sender":"test-user","message":"hello"}'
```

## Configuration

Credential examples and environment variables are documented in [chatbot-integrations/README.md](chatbot-integrations/README.md).

Never commit real API keys, access tokens, passwords, or `.env` files. Configure secrets as environment variables or through your deployment platform.

## Repository Structure

```text
.
├── calisto_nlp_export/       Rasa NLP and action server
├── chatbot-integrations/     TypeScript channel integration server
└── README.md                 Project overview
```
