# AI Swiss Legal Assistant 🇨🇭 👩‍⚖️ 🤖

This is a simple conversational-ui RAG (retrieval augmented generation) based on the Swiss Code of Obligations.

> It was created a starting point of the Ginetta Challenge at the women++ [Hack'n'Lead hackathon](www.womenplusplus.ch/hackandlead) November 2023

## ℹ️ Instructions

1. Use this repository as a template (or Fork it)
2. Add your team members as contributors
3. Put your presentation in the `docs/` folder
4. This repository **must** be open source (and licensed) in order to submit
5. Add the tag `hack-n-lead` to the repo description

## ▶️ Setup

There is two different ways to setup this project:

1. Install Ollama & Qdrant locally _(Ollama desktop app is currently is only available for Mac and Linux)_ - Ollama will take advantage of your GPU to run the model
1. Use the [Docker Compose](https://docs.docker.com/compose/) file to run Ollama & Qdrant in containers (just run in a terminal in the project directory) - easier setup, but Ollama will run on CPU

### Option 1: 🐳 Run Docker Compose

1. `docker compose up -d` to pull & run the containers
1. `docker compose exec ollama ollama run mistral` to download & install the mistral model

### Option 2: 🖐🏼 Manual installation

1. 🦙 [Download Ollama](https://ollama.ai/download) and install it locally
2. `ollama run mistral` to download and install the model locally (Requires 4.1GB and 8GB of RAM)
3. Open <http://localhost:11434> to check if _Ollama is running_
4. `docker pull qdrant/qdrant`
5. `docker run --name qdrant -p 6333:6333 -v ${PWD}/snapshots:/snapshots qdrant/qdrant`

Both Option 1 and 2 continue with the following setup:

### 💾 Setup Qdrant Vector Database

1. Open the Qdrant dashboard console <http://localhost:6333/dashboard#/console>
1. Create a new collection running this:
   ```curl
   PUT collections/swiss-or
   {
     "vectors": {
       "size": 384,
       "distance": "Cosine"
     }
   }
   ```
1. Download the [snapshot file](https://huggingface.co/datasets/brunnolou/swiss-code-of-obligations/resolve/main/swiss-code-of-obligations-articles-gte-small-2023-10-18-12-13-25_qdrant-v1-6-1.snapshot.zip)
1. Unzip the file using the terminal (⚠️ **_not with Finder on Mac_** ⚠️) with `unzip <file_name>`
1. Upload the file using the following command. Adapt the fields accordingly and run it from the same directory, as where your snapshot lies

```shell
curl -X POST 'http://localhost:6333/collections/swiss-or/snapshots/upload' \
    -H 'Content-Type:multipart/form-data' \
    -F 'snapshot=@swiss-code-of-obligations-articles-gte-small-2023-10-18-12-13-25.snapshot'
```

## 👩🏽‍💻 Run the App

1. Copy the file `.env.local.example` in the project and rename it to `.env`. Verify if all environment variables are correct
1. `yarn install` to install the required dependencies
1. `yarn dev` to launch the development server
1. Go to <http://localhost:3000> and try out the app

## 👩🏽‍🏫 Learn More

To learn more about LangChain, OpenAI, Next.js, and the Vercel AI SDK take a look at the following resources:

- [LangChain Documentation](https://js.langchain.com/docs) - learn about LangChain
- [Ollama](https://ollama.ai/) - learn about Ollama features, models, and API
- [Qdrant Documentation](https://qdrant.tech/documentation/) - learn about Qdrant
- [Vercel AI SDK docs](https://sdk.vercel.ai/docs) - learn mode about the Vercel AI SDK
- [Next.js Documentation](https://nextjs.org/docs) - learn about Next.js features and API
