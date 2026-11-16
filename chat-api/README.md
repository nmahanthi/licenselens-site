# LicenseLens local chat API

This is the local backend for the LicenseLens assistant. It keeps the Microsoft Foundry API key on the server, checks request size and message shape, and limits chat to Microsoft 365 licensing and LicenseLens topics. Unrelated questions receive a fixed redirect message without being sent to Foundry.

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

The existing LicenseLens deployment files are compiled assets. The local server adapts the assistant UI at request time. When the user consents to sharing scan data, it sends aggregate scan figures; when a question names one person, it filters the browser scan locally and sends only that person's name, licence assignments, and activity details to Foundry. If there is no unique match, it returns a not-found or ambiguous response without asking the model to guess. This uses an existing Microsoft Foundry model; it does not train a new model.

In-scope answers use a consistent **Answer**, **Details**, and **Source** format. Savings or cost-saving estimates also include an estimate note. Greetings and out-of-scope refusals keep their short response format.

This is a development-only setup. Do not publish this local bridge or point the hosted site at localhost; a proper hosted backend and original source/build configuration are still needed before deployment.
