# DocuTrace — Document Q&A with Source Citations (RAG)

Ask questions about a PDF and get answers grounded in the document, with the source file shown for every answer.

> Add a screenshot or GIF here: `![Demo](assets/demo.gif)`

## Problem
LLMs can hallucinate when asked about documents they have not seen. DocuTrace retrieves the most relevant parts of a PDF first, then asks the LLM to answer from that context, and shows which document the answer came from.

## How it works

```
PDF → load & parse → split into chunks → embeddings → ChromaDB (vector store)
                                                           │
User question → retriever (top matching chunks) → Groq LLM → answer + source
```

1. **Load:** the PDF is loaded with `UnstructuredFileLoader`.
2. **Chunk:** `CharacterTextSplitter` (chunk size 2000, overlap 400).
3. **Embed and store:** Hugging Face embeddings stored in a persistent ChromaDB folder (`vector_db`).
4. **Retrieve and answer:** LangChain `RetrievalQA` ("stuff" chain) with `openai/gpt-oss-20b` on Groq, temperature 0.
5. **Trace:** LangSmith tracing to inspect retrieval and prompts.
6. **UI:** Gradio web interface that returns the answer plus the source document.

## Tech Stack
Python, LangChain, Groq (`openai/gpt-oss-20b`), Hugging Face Embeddings, ChromaDB, LangSmith, Gradio

## Features
- Answers grounded in retrieved document chunks
- Source document shown with every answer
- Persistent vector store (no need to re-embed on every run)
- LangSmith tracing for debugging the LLM workflow
- Simple Gradio chat interface

## Results
<!-- Add real numbers only, for example: -->
- Tested on: [01] documents / [01] questions
- Average response time: _[x]_ seconds

## How to Run

1. Clone the repo
   ```bash
   git clone https://github.com/PravashisBiswal/DocuTrace-using-RAG.git
   cd DocuTrace-using-RAG
   ```
2. Install packages
   ```bash
   pip install -r requirements.txt
   ```
3. Create a `.env` file (copy `.env.example`) and add your own keys
   ```
   GROQ_API_KEY=your_groq_key
   LANGCHAIN_API_KEY=your_langsmith_key
   ```
4. Open `DocuTrace_RAG_Project.ipynb` and run all cells. The Gradio app launches at the end.

> Never commit your `.env` file or API keys.

## Links
- Live Demo: [https://www.linkedin.com/in/pravashisbiswal]
- Demo Video: [https://www.linkedin.com/posts/pravashisbiswal_generativeai-rag-langchain-activity-7513585463203389440-NXB5?utm_source=share&utm_medium=member_desktop&rcm=ACoAADkWWUYB5cnJ2PJLirJ_0r0TqPUtBV19K7w]

## Limitations and Future Improvements
- Currently loads one PDF defined in the notebook; add a file-upload option in the UI
- Add hybrid search (BM25 + vector) and a reranker
- Evaluate answer quality with RAGAS (faithfulness, context precision)
- Move code from the notebook into a clean `app.py`
- Add chat memory for follow-up questions

## Author
Pravashis Biswal — [LinkedIn](https://www.linkedin.com/in/pravashisbiswal) · [GitHub](https://github.com/pravashisbiswal)
