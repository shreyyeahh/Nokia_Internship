# Nokia_Solutions_and_Networks_Internship
TASK 1 - PDF Document Analysis Experiments with Docling
This repository documents a series of experiments focused on building an intelligent chatbot from complex, table-heavy PDF documents using the docling library. The goal was to explore and evaluate different strategies for extraction, retrieval, and generation to find the most effective pipeline.

Project Objective
The core task was to create a system that could accurately answer user questions based on the content of technical PDFs. This involved tackling two primary scenarios:

Single PDF Chat: Allowing a user to have an interactive Q&A session with one selected document.

Multi-PDF Knowledge Base: Processing an entire folder of documents to create a persistent, searchable knowledge base.

Approaches Explored
Scenario 1: Single PDF Analysis
The initial focus was on perfecting the mechanics for a single document.

1. Extraction & Initial Chunking
Tool: The docling library was used for its robust capabilities in parsing both scanned and digital-native PDFs.

Strategy: The primary method involved identifying all tables within the PDF. Each detected table was then converted into a single, large string. This string served as our foundational "chunk" of information. In cases where docling merged multiple visual tables, the resulting merged block was treated as one chunk.

2. Retrieval Methods Tested
Once the document was chunked, we tested two distinct retrieval methods to find the most relevant information based on a user's query.

Approach A: Keyword Search (TF-IDF)

Mechanism: A TF-IDF matrix was created from the table chunks. This method excels at finding chunks that contain the exact keywords or model numbers from the user's query.

Observation: This approach was highly effective for direct, keyword-specific questions (e.g., "Find specifications for MT-799012/W").

Approach B: Semantic Search (Embeddings)

Mechanism: We used the all-MiniLM-L6-v2 model to generate semantic embeddings for each chunk. This method finds chunks that are conceptually similar to the user's query, even if they don't share the same keywords.

Observation: This was better at understanding the intent behind a question (e.g., matching "cost of equipment" with a chunk about "price").

Scenario 2: Multi-PDF Knowledge Base
This approach evolved the system for a more production-oriented use case where the entire document library is processed in advance.

Mechanism: A script was developed to iterate through a directory containing all 800+ PDFs.

Batch Processing: It applied the same docling extraction and chunking logic to each file.

Data Persistence: The results were consolidated into a single master_database.csv file, and the corresponding search indexes (both the TF-IDF matrix and the semantic embeddings) were saved to disk. This created a persistent knowledge base that could be loaded instantly, separating the slow, one-time setup from the fast, real-time querying.

Answer Generation
For all approaches, the final step was consistent. The top-retrieved chunks were passed as context to a Large Language Model (OpenAI's gpt-4o-mini). The LLM's task was to synthesize this context and the original user query into a final, conversational answer.

Key Findings
Baseline Accuracy: Both keyword and semantic search methods achieved a strong and consistent baseline accuracy of 91% on single-document tasks.

The Chunking Bottleneck: The primary limitation was the initial chunking strategy. Treating an entire table as one chunk often diluted the specific context, making it difficult for the retrieval system to pinpoint precise facts.
