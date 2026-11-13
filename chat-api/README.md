# LicenseLens local chat API

This is a local-only backend scaffold for the LicenseLens assistant. It keeps the Microsoft Foundry API key on the server, checks request size and message shape, and limits chat to Microsoft 365 licensing and LicenseLens topics. Unrelated questions receive a fixed redirect message without being sent to Foundry.

## Requirements

- Node.js 20.6 or newer
- A Microsoft Foundry chat model deployment that supports the OpenAI-compatible v1 Chat Completions API
- The deployment endpoint, model deployment name, and API key

## Configure

Copy `.env.example` to `.env`, then fill in the endpoint, deployment name, and key. Use the Foundry project endpoint in the form `https://RESOURCE.services.ai.azure.com/api/projects/PROJECT`. The server appends `/openai/v1/chat/completions` for project endpoints. A model resource v1 base URL ending in `/openai/v1` is also supported. Do not include `/chat/completions` in the value. Keep `.env` out of Git.

## Run locally

From the `licenselens-site` project root:

```powershell
# If needed, create and edit chat-api/.env first:
Copy-Item chat-api/.env.example chat-api/.env
# Enter your Foundry values in chat-api/.env
npm start
```

Open `http://localhost:8081`. The same Node server serves the app, `/health`, and `/api/chat`; there is only one process and one local URL. It rewrites the bundled assistant request in memory and leaves the checked-in deployment bundle unchanged.

The existing LicenseLens deployment files are compiled assets. This local server provides a development-only bridge for the existing assistant UI. Do not publish this local bridge or point the hosted site at localhost; a proper hosted backend and original source/build configuration are still needed before deployment.
