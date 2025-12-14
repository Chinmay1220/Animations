flowchart LR
  U[User] --> S[Streamlit UI]

  %% Resume extraction
  S -->|POST /extract_resume (PDF)| A[FastAPI Backend]
  A --> P[PDF Parser\n(PyMuPDF / pdfplumber)]
  P -->|resume_text| S

  %% Analysis (RAG + LLM)
  S -->|POST /analyze\n(resume_text + JD + role_hint)| A

  A --> R[Preprocess & Chunk\nresume + job description]
  R --> E[Embedding Client\n(OpenAI Embeddings API)]
  E -->|vectors| V[(Pinecone Vector DB)]

  A -->|semantic query| V
  V -->|top-k retrieved docs| A

  A --> L[LLM Orchestrator\nPrompt Builder + Safety]
  L --> C[Chat Completion\n(OpenAI GPT)]
  C -->|structured JSON\n(summary, gaps, rewrites, plan)| A

  A -->|JSON response| S

  %% Report generation
  S --> D[PDF Report Generator\n(ReportLab)]
  D -->|Download| U
