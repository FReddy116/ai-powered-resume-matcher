# AI Resume and Jod description Matcher using Embeddings and Vector Search

## Overview

This project is a simple AI-based resume matching system that ranks resumes based on how well they match a given job description.

The goal was to move beyond basic keyword matching and build something that understands the meaning of the content. Instead of checking for exact words, the system compares resumes and job descriptions based on their semantic similarity using embeddings.

---

## How the System Works

The pipeline processes resumes step by step and converts them into a format suitable for intelligent comparison.

### 1. PDF Text Extraction

Resumes are uploaded as PDF files and processed using `pdfplumber`.
Each file is read page by page and converted into raw text.

---

### 2. Section Extraction

Instead of using the entire resume directly, important sections are extracted such as:

* Skills
* Experience
* Projects
* Certifications

This is done using simple pattern matching with regular expressions. The idea is to focus only on the most relevant parts of a resume.

---

### 3. Text Cleaning

The extracted content is then cleaned to make it consistent:

* Converted to lowercase
* Extra spaces and line breaks removed
* Text length limited to avoid memory issues

This step ensures better quality embeddings and stable execution.

---

### 4. Embedding Generation

For converting text into numerical form, a pre-trained model from Hugging Face is used:

`all-MiniLM-L6-v2`

This model generates embeddings, which are dense vector representations of text. These embeddings capture the meaning of the content rather than just the words.

---

### 5. Vector Storage using FAISS

All resume embeddings are stored in a FAISS index.

Before storing, embeddings are normalized so that their magnitude becomes 1. This ensures that similarity comparisons depend only on direction (semantic meaning) and not on vector size.

FAISS is then used with inner product search, which effectively behaves as cosine similarity when vectors are normalized.

---

### 6. Job Description Processing

When a job description is provided:

* It is converted into an embedding using the same model
* The query embedding is also normalized
* This ensures both resumes and the job description are directly comparable in the same vector space

---

### 7. Similarity Search

The system compares the job description embedding with all resume embeddings using cosine similarity.

This is implemented using FAISS `IndexFlatIP` along with L2 normalization of vectors.

Cosine similarity measures how close two vectors are in terms of direction, which makes it more suitable for sentence embeddings compared to raw distance-based methods.

This enables true semantic matching, where resumes are ranked based on meaning rather than exact keyword overlap.

---

### 8. Ranking

Resumes are ranked based on similarity scores returned by FAISS.

Higher similarity score indicates a better match between the resume and the job description.

---

## Relation to RAG Concepts

This project follows the retrieval side of a Retrieval-Augmented Generation (RAG) pipeline.

* The FAISS index acts as the retrieval system
* The embedding model ensures both query and documents are comparable
* Cosine similarity is used to retrieve the most relevant resumes

Although no text generation is included yet, this retrieval mechanism is the same foundation used in RAG-based systems.

---

## Tech Stack

* Python
* pdfplumber
* Sentence Transformers (Hugging Face)
* FAISS

---

## Key Takeaways

* Learned how embeddings capture semantic meaning
* Understood the importance of normalization in similarity search
* Implemented cosine similarity using FAISS
* Built a retrieval pipeline aligned with RAG concepts
* Optimized memory usage for stable execution

---

## Limitations

* Section extraction is rule-based and may not always be accurate
* No deep NLP parsing or entity recognition
* Works better with well-structured resumes
* Does not yet explain *why* a candidate is a good match

---

## Future Improvements

* Integrate a full LLM on top of the retrieval system
* Provide explanations for why a candidate is a good match
* Highlight missing skills or gaps compared to the job description
* Improve section extraction using LLMs instead of regex
* Introduce hybrid search (vector + keyword) for better ranking
* Replace FAISS with a scalable vector database (e.g., Azure AI Search)

---

## Conclusion

This project demonstrates how resume matching can be improved by using embeddings and cosine similarity instead of traditional keyword-based approaches.

It provides a practical understanding of how vector search and retrieval pipelines work, forming the foundation for more advanced systems like RAG-based applications.
