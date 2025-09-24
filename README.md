# TypeB-MiniRAG-Movies

A lightweight Retrieval-Augmented Generation (RAG) system built by **Yaseen Munowwar** as a take-home assignment for **TypeB**.  
The project demonstrates a minimal end-to-end pipeline for answering questions about movie plots using embeddings, retrieval, and LLMs, with structured JSON output.

---

## 📖 Overview

This project uses a subset of the **Wikipedia Movie Plots** dataset to build a simple RAG pipeline:

1. **Load & preprocess data** (500 movie plots).  
2. **Chunk plots** into ~300-word segments with overlap.  
3. **Generate embeddings** with OpenAI.  
4. **Store vectors** in Chroma (in-memory persistent vector store).  
5. **Retrieve top-k chunks** relevant to a user query.  
6. **Answer questions** with Groq’s Llama 3 model.  
7. **Return structured JSON** with:
   - `answer`: natural language answer  
   - `contexts`: retrieved plot snippets  
   - `reasoning`: explanation of how the answer was formed  

---

## 📂 Notebook

All steps are implemented in the notebook:

**`TypeB_MiniRag.ipynb`**

This notebook is available in the same GitHub repo.  
Open it (e.g., in **Google Colab** or locally with Jupyter) and run cells sequentially.

---

## ⚙️ How to Run

### 1. Install dependencies
The notebook installs required libraries automatically.  
Key libraries: `pandas`, `langchain`, `langchain-openai`, `langchain-groq`, `chromadb`, `tiktoken`.

### 2. Set up API keys
You will need:
- `OPENAI_API_KEY` → for embeddings  
- `GROQ_API_KEY` → for LLM inference  

Set them in the notebook (do **not** commit keys to GitHub).

### 3. Load dataset
The notebook downloads the **Wikipedia Movie Plots** dataset from Kaggle and loads the first 500 rows.

### 4. Preprocess & chunk
Plots are cleaned and split into ~300-word chunks with 50-word overlap.

### 5. Generate embeddings
Each chunk is embedded using **OpenAI `text-embedding-3-small` (1536-dim)**.

### 6. Vector store
Chunks + embeddings are stored in a **Chroma** vector store for fast similarity search.

### 7. Configure LLM
The **Groq Llama 3 70B** model is set up with temperature=0 for deterministic outputs.

### 8. Inference
Ask a question about movies. The system:
- Retrieves the most relevant chunks  
- Passes them to the LLM  
- Returns a structured JSON:

```json
{
  "answer": "Kansas Saloon Smashers",
  "contexts": [
    "Kansas Saloon Smashers :: A bartender is working at a saloon... Carrie Nation and her followers burst inside..."
  ],
  "reasoning": "The context snippet explicitly mentions Carrie Nation and her followers smashing up a saloon, which directly answers the question."
}
