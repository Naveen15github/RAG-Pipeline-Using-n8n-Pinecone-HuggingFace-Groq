# RAG-Pipeline-Using-n8n-Pinecone-HuggingFace-Groq
## Overview

This project demonstrates a **fully functional Retrieval-Augmented Generation (RAG) system** built using **n8n** as the orchestration layer. The workflow ingests documents from Google Drive, processes and embeds them using HuggingFace models, stores vectors in Pinecone, and enables intelligent question answering via a Groq-powered AI Agent.

This implementation was designed, configured, and validated end-to-end with a focus on **practical production-style architecture**, modularity, and clarity.

![Alt text](https://github.com/Naveen15github/RAG-Pipeline-Using-n8n-Pinecone-HuggingFace-Groq/blob/ffb5ef99e7b367c1d146fed7ddf1dc38beba1114/Screenshot%20(415).png)
![Alt text](https://github.com/Naveen15github/RAG-Pipeline-Using-n8n-Pinecone-HuggingFace-Groq/blob/ffb5ef99e7b367c1d146fed7ddf1dc38beba1114/Screenshot%20(416).png)
![Alt text](https://github.com/Naveen15github/RAG-Pipeline-Using-n8n-Pinecone-HuggingFace-Groq/blob/ffb5ef99e7b367c1d146fed7ddf1dc38beba1114/Screenshot%20(414).png)
![Alt text](https://github.com/Naveen15github/RAG-Pipeline-Using-n8n-Pinecone-HuggingFace-Groq/blob/ffb5ef99e7b367c1d146fed7ddf1dc38beba1114/Screenshot%20(417).png)


---

## High-Level Architecture

**Ingestion Pipeline**

```
Google Drive → File Download → Data Loader → Text Splitter → Embeddings → Pinecone Vector Store
```

**Query Pipeline**

```
User Chat → AI Agent → Vector Search (Pinecone) → Context Injection → Groq LLM → Final Answer
```

---

## Key Objectives

* Automate document ingestion and indexing
* Enable semantic search over private documents
* Implement context-aware Q&A using RAG
* Maintain clear separation between ingestion and query flows
* Use only configurable, reusable n8n nodes (no hardcoding)

---

## Tools & Technologies Used

| Component                             | Purpose                               |
| ------------------------------------- | ------------------------------------- |
| **n8n**                               | Workflow orchestration & automation   |
| **Google Drive Trigger**              | Detect new/updated documents          |
| **HuggingFace Embeddings**            | Convert text → vector representations |
| **Pinecone Vector DB**                | Store & retrieve embeddings           |
| **Groq Chat Model**                   | Low-latency LLM inference             |
| **Recursive Character Text Splitter** | Chunk documents efficiently           |

---

## Workflow Breakdown (Node-by-Node)

### 1. Google Drive Trigger

* Automatically triggers when a new file is created in Google Drive
* Ensures hands-free ingestion of documents

### 2. Download File

* Downloads the detected file in binary format
* Passes raw content to the processing pipeline

### 3. Default Data Loader

* Extracts readable text from the downloaded file
* Acts as the normalization layer for multiple file types

### 4. Recursive Character Text Splitter

* Splits documents into manageable chunks
* Preserves semantic continuity while avoiding token overflow

**Why this matters:**

* Improves retrieval accuracy
* Prevents context truncation in LLM calls

### 5. HuggingFace Embeddings (Ingestion)

* Generates embeddings for each text chunk
* Embedding dimension kept consistent across ingestion & query flows

### 6. Pinecone Vector Store (Ingestion)

* Stores embeddings with metadata
* Enables fast Approximate Nearest Neighbor (ANN) search

---

## Chat & Query Flow

### 7. When Chat Message Received

* Entry point for user questions
* Accepts natural language queries from the chat UI

### 8. HuggingFace Embeddings (Query)

* Converts user question into vector form
* Uses the **same embedding model** as ingestion to avoid dimension mismatch

### 9. Pinecone Vector Store (Retrieval)

* Performs similarity search against stored document vectors
* Returns top-k relevant chunks as context

### 10. AI Agent

* Central reasoning component
* Responsibilities:

  * Accept user query
  * Retrieve contextual documents
  * Inject context into prompt
  * Call the LLM

### 11. Groq Chat Model

* Executes final response generation
* Optimized for low latency and fast iteration

---

## Why Groq + Pinecone?

* **Groq**: Ultra-fast inference suitable for real-time chat
* **Pinecone**: Managed, scalable vector storage with production-grade performance

This combination ensures both **speed and accuracy**.

---

## Error Handling & Observations

### Handled Issues

* Vector dimension mismatch resolved by standardizing embedding models
* Token overuse prevented via controlled chunk sizes
* Graceful handling of out-of-scope questions

### Observed Behavior

* Context-based questions are answered accurately
* Generic questions without document context are safely handled by the agent

---

## Example Use Cases

* Internal documentation Q&A
* Cost analysis or architecture review assistants
* Knowledge base chatbots
* Private document search engines

---

## Design Decisions (Why This Approach)

* **Event-driven ingestion** → scalable & automated
* **Separate ingestion and query embeddings** → clarity & maintainability
* **AI Agent abstraction** → easy tool/model swapping
* **No custom code** → fully low-code, reproducible setup

---

## Project Status

✅ Ingestion pipeline implemented
✅ Vector indexing validated
✅ Chat-based retrieval working
✅ End-to-end RAG flow verified

---

## Future Enhancements

* Add metadata-based filtering in Pinecone
* Introduce conversation memory
* Add document versioning support
* Implement access control per user

---

## Final Notes

This project reflects a **hands-on, production-minded RAG implementation** using modern LLM tooling and workflow automation. Every component was intentionally chosen, wired, tested, and validated to demonstrate real-world applicability rather than a toy example.

---

**Author:** Naveen G

**Built with:** n8n • Pinecone • HuggingFace • Groq
