sequenceDiagram
  participant U as User
  participant S as Streamlit UI
  participant A as FastAPI Backend
  participant P as PDF Parser
  participant E as Embeddings API
  participant V as Pinecone Vector DB
  participant G as GPT (LLM)

  U->>S: Upload resume PDF + paste job description
  S->>A: POST /extract_resume (PDF)
  A->>P: Extract text
  P-->>A: resume_text
  A-->>S: resume_text

  S->>A: POST /analyze (resume_text, JD, role_hint)
  A->>E: Embed query/inputs
  E-->>A: vectors
  A->>V: similarity search (top-k)
  V-->>A: retrieved docs (benchmarks/resources)
  A->>G: Prompt + retrieved docs (grounding)
  G-->>A: structured JSON (summary, gaps, rewrites, plan)
  A-->>S: JSON result
  S-->>U: Render results + allow PDF download
