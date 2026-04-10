# Project Name: LexGuardAI-Lite
# Team Name: The Outsiders
# Team Members: Cotîrlă Darius-Alexandru, Bota Mihnea-Cătălin, Someșan Eduard

---

## Description of the Architecture

LexGuardAI Lite is a lightweight, locally deployable AI legal assistant built on a Retrieval-Augmented Generation (RAG) architecture. To mitigate the risk of "hallucinations" inherent in general Large Language Models, our system restricts the model's generation exclusively to retrieved, verified legal texts.

The architecture is designed as a streamlined Minimum Viable Product (MVP) optimized for a 2-month development cycle, focusing on execution speed and high-reliability guardrails.

### 1. Data Ingestion & Processing
* **Dataset:** We utilize a pre-cleaned, publicly available legal dataset hosted on Hugging Face, specifically focusing on structured Statutory Law. This avoids the overhead of custom scraping and parsing raw legal PDFs.
* **Chunking Strategy:** Documents are processed using **Recursive Character Text Splitting** combined with basic semantic grouping. Chunk sizes are optimized (~500 tokens with significant overlap) to ensure complete legal clauses are preserved within the Small Language Model's (SLM) context window.

### 2. Retrieval System
* **Vector Database:** The project utilizes **ChromaDB** as the vector store. It is lightweight, requires zero external infrastructure, and runs efficiently in-memory for local development.
* **Embeddings:** Text chunks are vectorized using `BAAI/bge-small-en-v1.5`, a highly efficient embedding model that runs easily on consumer hardware while providing strong semantic matching capabilities.
* **RAG Strategy:** The primary retrieval method is dense vector search enhanced by **Metadata Filtering**. User queries are pre-filtered by legal domain/topic before the similarity search, ensuring the retrieval is highly contextualized.

### 3. Generation & Guardrails
* **Language Model (SLM):** We deploy a highly efficient Small Language Model—such as **Microsoft Phi-3-Mini** or **Llama-3-8B-Instruct**—run locally via Ollama. 
* **Safety & Alignment:** Instead of complex and time-consuming RLHF, we enforce strict guardrails through **Advanced Prompt Engineering** and **Few-Shot Prompting**. The system prompt mandates that the SLM must base its answers *only* on the retrieved context and must respectfully decline to answer if the context does not contain the necessary information. 

## Other Related Details
* **Local Deployment:** A core goal of this project is accessibility. By utilizing ChromaDB, lightweight embeddings, and quantized SLMs via Ollama, the entire pipeline can be spun up and tested on standard consumer-grade laptops without relying on expensive cloud GPU APIs.
* **Phase 2 (Stretch Goals):** If time permits after the core pipeline is validated, we plan to implement Hybrid Search (BM25 + Vector) for exact-match legal terminology and Supervised Fine-Tuning (SFT) using LoRA to further improve the model's citation formatting.
