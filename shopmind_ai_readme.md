# ShopMind AI 🧠🛒

ShopMind AI is an advanced, intelligent shopping assistant built using **n8n**, **LangChain**, **Google Gemini**, **Ollama**, and **Qdrant**. It is designed specifically for an electronics store in Tunisia (handling prices in TND), supporting automated knowledge base ingestion, multi-intent request routing, AI-powered product search, RAG (Retrieval-Augmented Generation), and multi-step output verification.

---

## 📂 Project Structure

This project contains two primary n8n workflow exports:
1. **`ShopMind-RAG-Ingestion (1).json`**: The ETL and vectorization pipeline that reads product CSV files, normalizes currencies to Tunisian Dinars (TND), generates embeddings, and saves them into a Qdrant vector database.
2. **`ShopMind-AI (1).json`**: The core conversational/router workflow that handles customer webhooks/chat inputs, parses images via a Vision Agent, queries the RAG database or live web search (Serper API), verifies results, and returns verified product recommendations or technical answers.

---

## 🛠️ Key Features & Architecture

### 1. RAG Ingestion Pipeline (`ShopMind - RAG Ingestion`)
* **Multi-Format Ingestion**: Handles data sources for smartphones, laptops, TVs, headphones, and smartwatches.
* **Currency Normalization**: Automatically converts various source currencies (INR, USD) into **Tunisian Dinars (TND)** with designated exchange rates.
* **Vector Embeddings**: Processes documents using text embedding models (Google Gemini / Ollama) and indexes them into **Qdrant** under the `shopmind_products` collection.

### 2. AI Router & Processing Pipeline (`ShopMind - Router`)
* **Multi-Intent Classification**: Dynamically routes customer requests based on intent:
  * `recommend`: Recommends products matching specific budgets and use cases.
  * `compare`: Compares specifications between named products.
  * `tech_question` / `compatibility`: Answers technical or compatibility questions (e.g., USB-C power delivery).
* **Vision Agent Support**: Processes user-uploaded product images, spec screenshots, or ports/connectors to extract search hints.
* **Dual Retrieval Strategy**: Queries both the local **Qdrant Vector Store** (RAG) and **Live Web Search** (via Serper API targeting Tunisian e-commerce sites like Tunisianet, SpaceNet, etc.).
* **Self-Correction & Verification Loop**: Includes a rigorous multi-stage verification framework (Code checks, Verifier Agent, and price-scraping enrichment) to filter out-of-budget products, detect ungrounded claims, and retry up to 2 times if validation fails.

---

## 🚀 Getting Started & Installation

### Prerequisites
* **n8n** (Self-hosted or Cloud instance)
* **Qdrant Vector Database** instance
* **Ollama** (running `nomic-embed-text` or similar embedding models, if applicable)
* API Keys configured in n8n credentials:
  * Google Gemini API Key
  * Serper API Key (for live web search)
  * Qdrant API Key

### Importing Workflows into n8n
1. Open your n8n dashboard.
2. Click on **Add workflow** -> **Import from File**.
3. Import `ShopMind-RAG-Ingestion (1).json` to set up your knowledge base ingestion flow.
4. Import `ShopMind-AI (1).json` to set up the main chat/webhook application flow.
5. Update your credentials nodes inside n8n to match your active API accounts (Google Gemini, Qdrant, Serper).

---

## 📌 Usage
Once active, the main workflow exposes a webhook endpoint (`/webhook/shopmind`) capable of receiving customer JSON payloads containing queries, optional session IDs, and image URLs.

## 🎬 Demo Video
Watch the full project demonstration video on [Google Drive]https://drive.google.com/file/d/18c1d_Gm7dj3KD-zDjLPjH6_soF57IHyi/view?usp=sharing
