# BFSI Call Center AI Assistant

📌 Project Overview

The BFSI Call Center AI Assistant is a lightweight, compliant AI system designed to handle common customer queries in the Banking, Financial Services, and Insurance (BFSI) domain.

Use cases:
1. Loan eligibility and application status
2. EMI details and schedules
3. Interest rate information
4. Payment and transaction queries
5. Basic account support

📌 System Architecture

![BFSI Call Center AI Assistant System Architecture](https://github.com/user-attachments/assets/776f2081-372f-4b7a-a4a3-59debe18e59f)

📌 Response Priority Logic

Tier 1 -- Strong dataset similarity	Return stored response

Tier 2 -- Complex financial query -- Use RAG retrieval

Tier 3 -- Generate via local SLM

This ensures a safety-first design.

📌 Core Components

1️⃣ Dataset Layer (Primary Response Layer)
1. 200+ BFSI conversation samples
2. Alpaca format (Instruction, Input, Output)
3. Professional and compliant tone
4. Embedded using all-mpnet-base-v2

Role:
If cosine similarity score exceeds threshold →
Return stored response directly.
This ensures:
1. Deterministic answers
2. Zero hallucination
3. Policy-safe responses

2️⃣ Small Language Model (Local SLM)
1. Lightweight instruction-based model
2. Runs locally via Ollama
3. Model: phi3:mini

Role:
Used only when:
No strong dataset similarity match is found
This provides flexibility while maintaining safety.

3️⃣ RAG Layer (Knowledge Retrieval)
Used for complex financial or policy-related queries such as:
1. Interest rate explanations
2. EMI breakdowns
3. Prepayment penalties
4. Foreclosure policies

Implementation:
1. Structured knowledge base (knowledge_base.json)
2. Embedded into vectors
3. Retrieved via cosine similarity
4. Injected into controlled prompt

Role:
Provide grounded, policy-based responses.

🔎 How Embeddings Are Generated

We use the model:

all-mpnet-base-v2

which converts each text input into a 768-dimensional vector that captures its semantic meaning.

Process:

Prepare Text:

1. For dataset matching: instruction + input are combined.
2. For knowledge retrieval: JSON content is flattened into text chunks.
   
Generate Embeddings:

1. embeddings = model.encode(text_list, convert_to_numpy=True)
2. Each text entry is converted into a fixed-size vector.
   
Store Embeddings:

1. Saved as .npy files for fast loading.
2. Original text/metadata stored separately in JSON.
3. Both are index-aligned for accurate retrieval.

At runtime, the user query is converted into an embedding and compared with stored embeddings using cosine similarity to find the most relevant match.

📌 Guardrails & Compliance

The assistant enforces strict BFSI safety rules:
1. No guessing financial numbers
2. No fake interest rates
3. No fabricated policies
4. No exposure of customer-specific data
5. Reject out-of-domain queries

Low-temperature generation (0.3) is used to reduce randomness.

📌 Technical Stack

Embeddings → Sentence Transformers

Model → all-mpnet-base-v2

Local → LLM	Ollama

SLM Example	→ phi3:mini

Similarity → Cosine Similarity

Data Format	→ Alpaca

📌 Project Structure

main.py                     → Orchestrates full pipeline

generate_embeddings.py      → Creates dataset embeddings

prepare_rag_embedding.py    → Creates knowledge base embeddings

rag_embedding_service.py    → Semantic retrieval logic

prompt_builder.py           → Structured prompt construction

slm_service.py              → Local model inference via Ollama

similarity_check.py         → Dataset similarity evaluation

knowledge_base.json         → Policy knowledge documents

alpaca_bfsi_sample.json     → BFSI dataset

📌 Outcome

A modular, production-style BFSI AI assistant that demonstrates:

1. Safe and deterministic response routing
2. Retrieval-Augmented Generation (RAG) integration
3. Semantic similarity–based query handling
4. Local LLM deployment with compliance guardrails
5. Cost-efficient, threshold-based AI architecture
