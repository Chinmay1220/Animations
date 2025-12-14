┌──────────────┐        ┌──────────────────────────┐
│     User     │        │        Streamlit UI       │
│ (Resume+JD)  │───────▶│ Upload PDF + Paste JD     │
└──────────────┘        │ Calls FastAPI endpoints   │
                        └─────────────┬─────────────┘
                                      │
                                      │ 1) /extract_resume (PDF)
                                      ▼
                           ┌──────────────────────────┐
                           │      FastAPI Backend      │
                           │  - Resume extractor       │
                           │  - RAG pipeline           │
                           │  - LLM prompt + safety    │
                           └─────────────┬─────────────┘
                                         │
                          PDF text parse  │
                                         ▼
                           ┌──────────────────────────┐
                           │   PDF Parser (PyMuPDF)    │
                           └──────────────────────────┘

                                      │
                                      │ 2) /analyze (resume_text + JD)
                                      ▼
                           ┌──────────────────────────┐
                           │     RAG Retrieval Step    │
                           │ Embed query + search      │
                           └─────────────┬─────────────┘
                                         │
                                         ▼
                         ┌───────────────────────────────┐
                         │   Pinecone Vector Database     │
                         │ Role expectations / benchmarks │
                         │ Learning resources             │
                         └───────────────────────────────┘
                                         ▲
                                         │ embeddings
                                         ▼
                           ┌──────────────────────────┐
                           │ OpenAI Embeddings API     │
                           └──────────────────────────┘

                                      │ retrieved docs
                                      ▼
                           ┌──────────────────────────┐
                           │        OpenAI GPT         │
                           │ Uses retrieved evidence   │
                           │ Returns structured JSON   │
                           └─────────────┬─────────────┘
                                         │
                                         ▼
                           ┌──────────────────────────┐
                           │ Streamlit Results + PDF   │
                           │ Summary + gaps + rewrites │
                           │ Learning plan + download  │
                           └──────────────────────────┘
