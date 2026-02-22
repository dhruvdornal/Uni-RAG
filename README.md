# University Admissions RAG

A simple end-to-end Retrieval Augmented Generation (RAG) system built for practice. It lets you ask plain English questions about a curated list of German universities and get back answers without manually scanning a spreadsheet.

This is not a production system.

---

## What this does

I have an Excel sheet with German universities shortlisted for Masters admissions, containing details like course names, IELTS requirements, GPA requirements, application deadlines, and DAAD links. Instead of ctrl+F-ing through the sheet every time, this RAG lets you just ask a question and get an answer.

---

## Tech stack

- **Google Colab** for the runtime environment
- **ChromaDB** as the vector database, runs locally in the notebook with no server setup needed
- **sentence-transformers** (all-MiniLM-L6-v2) for generating embeddings, free and runs on the Colab machine itself
- **GitHub Marketplace Models** for the LLM, specifically Meta's Llama 3.3 70B Instruct

I chose GitHub Marketplace Models because it gives access to powerful models like Llama 3.3 70B using just a GitHub personal access token, with a free tier generous enough for practice and experimentation. No OpenAI billing, no separate API account, no credit card for testing. It is a practical low-cost option when you are learning and do not want to spend money just to build something small.

---

## How it works

Each row in the spreadsheet is converted into a readable text string and stored as an embedding in ChromaDB. When you ask a question, the question gets embedded using the same model and ChromaDB finds the most semantically similar rows. Those rows are passed as context to the LLM, which then answers your question based only on that context.

Retrieval uses a hybrid approach: semantic search through ChromaDB and keyword search over the raw text, with results merged. This handles both vague natural language questions and specific lookups like exact course names.

---

## What RAG is actually good at

RAG solves one specific problem really well: it gives users a natural language interface over a body of information that would otherwise require manual searching. Instead of teaching everyone how to use spreadsheet filters or write database queries, you let them just ask a question in plain English.

For this project that means a student can ask something like "which universities accept IELTS 6.5 for artificial intelligence and have deadlines after April" and get a direct answer, rather than scanning 25 rows themselves.

RAG is the right tool when:
- The questions are varied and hard to predict in advance
- The information source is semi-structured or unstructured (documents, sheets, PDFs)
- You want a conversational interface over existing data without rebuilding it as a database

RAG is not the right tool when:
- You need precise aggregations like "count all rows where X equals Y"
- You need guaranteed recall of every matching record
- The questions are always structured and predictable (in that case a simple filter or SQL query does the job better)

Understanding this distinction is part of what this project was built to explore.

---

## Limitations

The dataset is small and manually maintained. The IELTS and GPA columns have inconsistent formatting in some rows which affects retrieval accuracy. Some questions that require scanning the entire dataset (like listing every university with IELTS 5.5) are better handled by a direct filter than by RAG, and that is by design, not a bug.

---

## Setup

Upload the Excel file to Colab, add your GitHub PAT to secrets in Google colab then run Cell 2, and run all cells in order. Re-run the last cell to test with new questions.
