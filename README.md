# Local AI Email Auto-Responder

A fully local, privacy-preserving AI email auto-responder built with n8n and Ollama. This system reads incoming emails, processes them through a locally hosted Large Language Model (LLM), and automatically sends context-aware replies without relying on any external cloud APIs.

## Requirements

- Docker
- Docker Compose

## Quick Start Guide

### 1. Configure Credentials

Before starting the system, you must define the necessary environment variables for your email accounts.

1. Copy the example environment file:
   ```bash
   cp .env.example .env
   ```
2. Open `.env` in your preferred text editor and fill in your IMAP and SMTP credentials. Do not commit your real `.env` file to version control.

### 2. Start the Services

Start the n8n and Ollama containers in detached mode:

```bash
docker-compose up -d
```

### 3. Load the Language Model

The prompt configuration requires the `llama3:8b` model. Once the containers are healthy, execute the following command to download the model directly into your local Ollama instance:

```bash
docker exec -it ollama_responder ollama pull llama3:8b
```

### 4. Import the n8n Workflow

1. Navigate to `http://localhost:5678` in your web browser.
2. If this is your first time starting n8n, create an owner account.
3. Once logged in, open the Workflows panel.
4. Click on the drop-down menu in the top right corner and select **Import from File**.
5. Select the `workflow.json` file provided in this repository.

### 5. Configure Workflow Credentials

The imported workflow nodes ("Email Read (IMAP)" and "Send Email") require credential profiles to operate.
1. Open the **Email Read (IMAP)** node, configure a new IMAP credential using the values you defined in your `.env` file, and save it.
2. Open the **Send Email** node, configure a new SMTP credential using the same details.

Once the credentials are set, toggle the workflow to **Active**. The system will now automatically check your inbox and process relevant unread emails.
