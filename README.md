# RAG FAQ System - n8n Workflow

![architecture.jpg](architecture.jpg)

A Retrieval-Augmented Generation (RAG) system built with n8n that automatically processes documents from Google Drive and provides an AI-powered chat interface for answering questions.
 
our workflow consists of two main flows:

1. **Document Ingestion**: Monitors Google Drive folder for new files, processes them, and stores embeddings in Pinecone
2. **Chat Interface**: Provides an AI agent that can answer questions using the stored documents
 
## Required Credentials

Before importing this workflow, set up the following credentials in your n8n instance:

### 1. Google Drive OAuth2
- **Node**: Google Drive Trigger, Download file
- **Type**: OAuth2
- **Setup**: Follow n8n's Google Drive credential setup guide

### 2. Pinecone API
- **Node**: Pinecone Vector Store nodes
- **Type**: API Key
- **Required**: Pinecone API key and environment

### 3. Google Gemini (PaLM) API
- **Node**: Google Gemini Chat Model, Embeddings nodes
- **Type**: API Key
- **Required**: Google AI API key

## Setup Instructions

### 1. Import the Workflow

1. Copy the contents of `n8n-workflow-sanitized.json`
2. In n8n, go to **Workflows** → **Import from File** or **Import from URL**
3. Paste the JSON content

### 2. Configure Credentials

Replace the placeholder credential IDs with your actual credentials:

- `YOUR_GOOGLE_DRIVE_CREDENTIAL_ID` → Your Google Drive OAuth2 credential
- `YOUR_PINECONE_CREDENTIAL_ID` → Your Pinecone API credential  
- `YOUR_GOOGLE_GEMINI_CREDENTIAL_ID` → Your Google Gemini API credential

### 3. Configure Google Drive Folder

1. Create a folder in Google Drive for document uploads
2. Get the folder ID from the URL: `https://drive.google.com/drive/folders/FOLDER_ID_HERE`
3. Replace `YOUR_GOOGLE_DRIVE_FOLDER_ID` in the "Google Drive Trigger" node

### 4. Configure Pinecone Index

1. Create a Pinecone index (recommended: 768 dimensions for Gemini embeddings)
2. Update the `pineconeIndex` value if not using "demo"
3. Update the `pineconeNamespace` if not using "faq"

### 5. Activate the Workflow

1. Test the workflow manually first
2. Set `"active": true` to enable automatic monitoring
3. The webhook will be generated automatically for the chat interface

## Usage

### Adding Documents

1. Upload PDF, TXT, or other supported documents to your configured Google Drive folder
2. The workflow will automatically:
   - Download the file
   - Process and chunk the content
   - Generate embeddings
   - Store in Pinecone vector database

### Using the Chat Interface

1. Find your chat webhook URL in the "When chat message received" node
2. Access the chat interface at: `https://your-n8n-instance.com/webhook/YOUR_WEBHOOK_ID/chat`
3. Ask questions about your documents

## Customization Ideas

- Add error handling nodes
- Implement notification on successful document processing
- Add support for multiple namespaces/categories
- Integrate with Slack/Discord for chat interface
- Add authentication to chat webhook
- Implement rate limiting
 