# WhatsApp Chatbot

A multilingual WhatsApp chatbot powered by local LLMs via Ollama. Handles customer support, order tracking, and product inquiries through the WhatsApp Cloud API.

## Features

- **Multilingual** — Detects and responds in the user's language (Turkish, English, Arabic, German, etc.)
- **AI-Powered** — Uses Ollama for local, offline LLM responses (no API costs)
- **Customer Support** — Collects name, order number, and issue description
- **Order Tracking** — Looks up order status by order number
- **Product Catalog** — Answers questions about products and pricing

## Architecture

```
WhatsApp → Meta Cloud API → Webhook (FastAPI) → Ollama (Local LLM) → Reply
```

## Quick Start

### 1. Clone and install

```bash
git clone https://github.com/blitzlabeg/whatsapp-chatbot.git
cd whatsapp-chatbot
pip install -r requirements.txt
```

### 2. Configure environment

```bash
cp .env.example .env
```

Edit `.env` with your Meta WhatsApp API credentials:

| Variable | Description |
|---|---|
| `WHATSAPP_TOKEN` | Meta permanent access token |
| `WHATSAPP_PHONE_ID` | Phone number ID from Meta |
| `VERIFY_TOKEN` | Your custom verification token |
| `OLLAMA_URL` | Ollama server URL (default: `http://localhost:11434`) |
| `OLLAMA_MODEL` | Model to use (default: `llama3`) |

### 3. Start Ollama

```bash
ollama serve
ollama pull llama3
```

### 4. Run the bot

```bash
uvicorn bot:app --host 0.0.0.0 --port 8000
```

### 5. Set up webhook

Use [ngrok](https://ngrok.com) for local development:

```bash
ngrok http 8000
```

Set the webhook URL in Meta Developer Portal:

```
https://your-domain.com/webhook?hub.mode=subscribe&hub.verify_token=whatsapp_verify_2025
```

## API Endpoints

| Method | Path | Description |
|---|---|---|
| `GET` | `/` | Health check |
| `GET` | `/health` | Detailed health status |
| `GET` | `/webhook` | Webhook verification (Meta) |
| `POST` | `/webhook` | Receive WhatsApp messages |

## Project Structure

```
whatsapp-chatbot/
├── bot.py              # Main application
├── requirements.txt    # Python dependencies
├── render.yaml         # Render.com deployment config
├── .env.example        # Environment variables template
└── .gitignore
```

## How It Works

1. User sends a WhatsApp message
2. Meta Cloud API forwards it to the webhook
3. Bot detects the message language
4. Ollama generates a response using the local LLM
5. Bot sends the reply back via WhatsApp API

## License

[MIT](LICENSE) — Blitz Team
