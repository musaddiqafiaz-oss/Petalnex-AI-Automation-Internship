# Day 21 — RAG Assistant

## Project Overview

This project implements a **Retrieval-Augmented Generation (RAG) Assistant** using n8n.

The assistant is designed as an **HR Policy Assistant** that answers questions only from the company's HR Policy Knowledge Base. The knowledge base is stored in Google Sheets, converted into embeddings, and stored in a Supabase Vector Store.

If the requested information is not available in the knowledge base, the assistant provides a safe fallback response instead of making up an answer.

---

## Objective

Build an AI assistant that can:

* Retrieve relevant information from a company knowledge base
* Answer questions using only retrieved information
* Avoid hallucinating company policies
* Provide a fallback when information is unavailable
* Include the source of the retrieved information in the response

---

## Technologies Used

* **n8n** — Workflow automation
* **Google Sheets** — HR Policy Knowledge Base
* **Google Gemini Embeddings** — Convert documents into vectors
* **Supabase Vector Store** — Store and retrieve document embeddings
* **Groq** — Language model for generating answers
* **JavaScript Code Nodes** — Cleaning, context preparation, fallback, and source citation

---

# Workflow Architecture

## 1. Ingestion Workflow

The ingestion workflow converts the HR policy data into searchable vectors.

```text
Manual Trigger
      ↓
Get Rows from Google Sheets
      ↓
Clean Content
      ↓
Edit Fields
      ↓
Default Data Loader
      ↓
Recursive Character Text Splitter
      ↓
Gemini Embeddings
      ↓
Supabase Vector Store
```

The Google Sheets node reads the **HR Policy Knowledge Base** from the configured spreadsheet.

### Content Cleaning

The `clean content` Code node:

* Removes HTML tags
* Removes unnecessary whitespace
* Trims content
* Cleans title and category fields

### Chunking

The workflow uses a **Recursive Character Text Splitter** with:

* Chunk size: **500**
* Chunk overlap: **50**

This allows larger documents to be divided into smaller searchable pieces.

### Metadata

The documents are stored with metadata including:

* `title`
* `category`
* `source`

This metadata is later used to identify the source of retrieved information.

### Embeddings

Google Gemini embeddings are used to convert the document chunks into vectors before storing them in Supabase.

### Vector Database

The embeddings are inserted into the Supabase `documents` table through the Supabase Vector Store node.

---

# 2. Query / RAG Workflow

When a user asks a question, the workflow follows this path:

```text
Chat Message
     ↓
Supabase Vector Search
     ↓
Score & Build Context
     ↓
Answer Found In KB?
     ↓
   ┌───────────────┐
   │               │
  YES              NO
   ↓               ↓
Groq LLM       Fallback
   ↓
Append Source Citation
```

The chat trigger receives the user's question and sends it to the Supabase Vector Store for retrieval.

---

## Retrieval

The Supabase Vector Store searches the `documents` table using the user's question.

The workflow retrieves up to **5 relevant results** (`topK = 5`).

---

## Context Building

The `Score & Build Context` Code node:

1. Reads the retrieved documents
2. Extracts their content
3. Extracts metadata
4. Combines the retrieved content into a context
5. Creates a list of sources
6. Determines whether relevant information exists

If documents are found:

```text
inKnowledgeBase = true
```

Otherwise:

```text
inKnowledgeBase = false
```

---

# 3. Knowledge Base Check

The `Answer Found In KB?` IF node checks whether relevant information was retrieved.

### If information is found

The workflow sends the retrieved context to the LLM.

### If information is not found

The workflow sends the request to:

**Fallback - Not In KB**

This prevents the assistant from inventing company policies.

---

# 4. LLM Answer Generation

The `Basic LLM Chain` uses a Groq Chat Model with:

```text
Model: openai/gpt-oss-120b
```

The LLM receives:

* Retrieved HR policy context
* User's question
* Instructions to answer only from the provided context

The prompt specifically instructs the assistant not to use outside knowledge or invent company policies.

---

# 5. Source Citation

After generating the answer, the `Append Source Citation` Code node adds the source information to the final response.

Example:

```text
Answer: Employees can apply for leave according to the HR leave policy.

Source: Leave Policy
```

This makes the response more transparent and helps the user understand where the answer came from.

---

# Example Questions

### Question 1

**User:**

```text
What is the company's leave policy?
```

**Expected behavior:**

The assistant retrieves the relevant leave-policy information and answers using the knowledge base.

---

### Question 2

**User:**

```text
What are the working hours?
```

**Expected behavior:**

The assistant retrieves the working-hours policy and provides the answer with a source citation.

---

### Question 3

**User:**

```text
What are the attendance rules?
```

**Expected behavior:**

The assistant searches the vector database and answers using the retrieved attendance information.

---

### Question 4 — Internship

**User:**

```text
What are the internship guidelines?
```

**Expected behavior:**

The assistant retrieves the relevant internship guideline from the knowledge base.

---

### Question 5 — Unknown Information

**User:**

```text
What is the company's maternity leave policy?
```

If this information is not present in the knowledge base, the assistant should **not invent an answer**.

Instead, it uses the fallback response:

```text
I'm sorry, that information is not available in the HR policy knowledge base I have access to. Please check with the HR team directly or consult the full policy handbook.
```

---

# Safety / Grounding Rules

The assistant follows these rules:

1. Answer only from the HR Policy Knowledge Base.
2. Do not use outside knowledge.
3. Do not invent or assume company policies.
4. If information is not clearly available, use the fallback.
5. Keep answers concise and relevant.
6. Include the source when an answer is generated.

---

# Knowledge Base Topics

The current assistant is designed around these HR-related areas:

* Leave Policy
* Working Hours
* Attendance
* Internship Guidelines
* Code of Conduct

The fallback node explicitly identifies these knowledge-base areas when information is unavailable.

---

# Main n8n Nodes

| Node                              | Purpose                              |
| --------------------------------- | ------------------------------------ |
| When clicking ‘Execute workflow’  | Starts ingestion                     |
| Get row(s) in sheet               | Reads HR policies                    |
| clean content                     | Cleans policy text                   |
| Edit Fields                       | Prepares document fields             |
| Default Data Loader               | Converts content into documents      |
| Recursive Character Text Splitter | Splits documents into chunks         |
| Embeddings Google Gemini1         | Creates embeddings                   |
| Supabase Vector Store             | Stores vectors                       |
| When chat message received        | Receives user questions              |
| Supabase Vector Store1            | Retrieves relevant documents         |
| Score & Build Context             | Builds retrieved context             |
| Answer Found In KB?               | Checks whether information was found |
| Basic LLM Chain                   | Generates grounded answer            |
| Groq Chat Model                   | Provides the LLM                     |
| Fallback - Not In KB              | Handles unknown questions            |
| Append Source Citation            | Adds source information              |

---

# Conclusion

This project demonstrates a complete RAG pipeline in n8n:

```text
Google Sheets
     ↓
Extract
     ↓
Clean
     ↓
Chunk
     ↓
Gemini Embeddings
     ↓
Supabase Vector Store
     ↓
User Question
     ↓
Vector Search
     ↓
Retrieve Context
     ↓
Check Knowledge Base
     ↓
Groq LLM
     ↓
Grounded Answer
     ↓
Source Citation
```

The main benefit of this system is that the AI assistant is **grounded in the company's own HR knowledge base** rather than relying only on general model knowledge.
