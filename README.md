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


### Connecting Slack API and Slack oAuth2 API Credentials in n8n

#### Step 1: Create a Slack Bot
1. Create a new app.
2. Name the app **LinkedIn Bot**.
3. Once the bot is created, you can proceed with configuring the bot's permissions and credentials.

#### Step 2: Set Scopes for Slack Bot
1. Under **OAuth & Permissions** in the Slack API, configure the **Bot Token Scopes**. Add the following scopes to define the permissions your bot requires:
   - `app_mentions:read`: View messages that directly mention @LinkedIn Bot in conversations where the bot is present.
   - `channels:history`: View messages and other content in public channels that LinkedIn Bot is a part of.
   - `channels:read`: View basic information about public channels in the workspace.
   - `chat:write`: Send messages as @LinkedIn Bot.
   - `groups:read`: Read information about private groups.

#### Step 3: Get the Redirect URL for Slack OAuth2 API
1. In **n8n**, create a new credential for **Slack OAuth2 API**.
2. The **Redirect URL** will be generated in the **Slack OAuth2 API credential node** in n8n.
3. Go to **Slack API → OAuth & Permissions**, and in the **Redirect URL** field, add the **Redirect URL** obtained from the n8n Slack OAuth2 API credential node.

#### Step 4: Install App to Your Workspace
1. In the **Slack API** page, under **OAuth & Permissions**, click on **Install to "Workspace Name"**.
2. After installing, a **Bot User OAuth Token** will be generated.
3. This **Bot User OAuth Token** is added to **Slack Api** Credential in n8n under **Access Token**.

#### Step 5: Configure Slack OAuth2 API Credentials in n8n
1. In n8n, under the **Slack OAuth2 API** credentials, input the following:
   - **Client ID** and **Client Secret Key**, which can be found under **Basic Information** on the Slack API page.
   

#### Step 6: Enable Event Subscription
1. In the **Slack API** settings, go to **Event Subscriptions** and enable events.
2. In the **Request URL** field, provide the **test or production URL** that you get from the **Slack Trigger node** in n8n under **Webhook URLs**.
3. Under **Subscribe to Bot Mentions**, add the following event:
   - **Event Name**: `app_mention`



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


