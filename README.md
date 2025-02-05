# linkedIn-post-creator
AI agent that creates a LinkedIn post when user send a URL to a Slack channel.

# n8n LinkedIn Post Creator Workflow

## Overview
This workflow processes web URLs shared in a Slack channel or Google Drive links and generates LinkedIn post text using AI.

## Prerequisites
Ensure you have the following before setup:
- **n8n (self-hosted-ai-starter-kit)** installed locally or on a server.
- **Slack workspace access** with bot creation permissions.
- **Google Drive API access** (if processing files from Google Drive).
- **OpenAI API key** for AI-generated content.

## Setup Instructions
### 1. Start n8n
Run self-hosted-ai-starter-kit or n8n on your local machine or server.

### 2. Content Processing Setup
#### HTML Processing (if not using self-hosted-ai-starter-kit)
Add the following to your `.env` file:
```bash
NODE_FUNCTION_ALLOW_EXTERNAL=axios,url,path,file-type,jsdom,@mozilla/readability
```
Add the following to your **Dockerfile**:
```dockerfile
RUN npm install -g axios url file-type neo4j-driver @mozilla/readability \
    jsdom marked pdf.js-extract pdf-parse-fork tiktoken firebase-admin \
    canvas json5 katex marked-katex-extension pdfjs-dist ioredis amqplib \
    pdf-img-convert flexsearch puppeteer turndown sanitize-html \
    commonmark path
```

#### PDF Processing (Docling Integration)
1. **Build and Run the Docling API Container** using the provided `docling-api` directory.
2. **Create a Docker Network** (e.g., `n8n-network`) and connect both the **Docling API** and **n8n** containers.
3. **Configure n8n HTTP Node** to send requests to:
   ```
   http://docling-api:5000/process
   ```
   Pass in the PDF URL or content to be parsed.

### 3. Import the Workflow
Import the provided workflow JSON file into n8n.

### 4. Configure Credentials
#### Slack API:
- **OAuth & Permissions:** Authenticate using your Slack bot token and configure the redirect URL.
- **Events & Subscription:** Enable events, add the Webhook URL from the Slack trigger node, and grant necessary bot permissions (`app_mentions:read`, `chat:write`).

#### Using ngrok (if n8n is locally hosted)
1. Install **ngrok** (if not installed).
2. Run:
   ```bash
   ngrok http 5678  # Assuming n8n runs on port 5678
   ```
3. Use the generated HTTPS URL in Slack credentials.

#### Google Drive API:
- Create and authenticate a Google Drive credential.

#### OpenAI API:
- Add your **OpenAI API key** for AI-powered text generation.

## Testing
Use the **Testing URL** from the **Slack Trigger Node’s Webhook URL**.

## Deployment
For production, use the **Production URL** from the **Slack Trigger Node’s Webhook URL**.


