# Local RAG System for Academic Research: Child Marriage in Zimbabwe

## Overview
This repository contains a production-grade, local Retrieval-Augmented Generation (RAG) pipeline designed to ingest, clean, and synthesize peer-reviewed literature, university theses, and legal case law regarding child marriage in Zimbabwe (covering literature from 2016 to 2026). The primary objective of the system is to provide researchers, NGO staff, and policymakers with highly context-grounded answers that tightly couple claims with verifiable document snippets while rigorously preventing data hallucinations and out-of-scope leakages.

## Key System Architecture
* **Automated Log Generation:** Scans and inventories raw text, generating a canonical index data sheet (`data/collection_log.csv`) mapping document files to DOIs, page counts, word counts, and automatically discovered open-access/Creative Commons terms.
* **Targeted Pattern Normalization:** Clean layers eliminate repeated layout elements like running headers, footers, and journal banners via frequency analysis. Unicode normalizations (NFKC) mend ligatures and odd glyphs, while whitespace operations repair hyphenated text splits without removing in-text citation signatures needed for retrieval attribution.
* **Token-Bounded Chunking:** Deploys a recursive character text-splitting strategy bounded tightly at a 1,000-character maximum with a 150-character overlap. This prevents oversized structural elements (such as tabular matrices or appendix references) from causing silent clipping during downstream embedding.
* **Vector Isolation Strategy:** Implements an enterprise vector engine using local ChromaDB storage running cosine distance metrics. The database persists locally inside the target directory path, enabling immediate system reloads without repeating computing steps.
* **Strict Attribution and Abstention Engine:** Integrates structured validation formats that use role-prompting and XML data delimiters. The underlying language model architecture enforces absolute constraints to prevent interpolation, generating explicit data omissions rather than guessing.
* **Interactive Relevance Filter:** Features a loop architecture equipped with numerical similarity thresholds to block out-of-scope or irrelevant queries before they consume third-party API tokens.

## Tech Stack and Dependencies
* **Local Embedding Model:** `sentence-transformers/all-MiniLM-L6-v2` (384 dimensions, 256-token maximum context window)
* **Local Vector Database:** ChromaDB client instance initialized with a cosine space configuration
* **Hosted Large Language Model:** `openai/gpt-oss-120b` executed with a low temperature of 0.2 via the Groq SDK client
* **Processing Utilities:** `pypdf`, `pandas`, `nltk`, `langchain-text-splitters`

To configure your Python compute environment, run the installation sequence below:
```bash
pip install -q pypdf pandas nltk langchain-text-splitters sentence-transformers chromadb groq tf-keras
```

## Data Corpus Specifications
The default pipeline evaluates document metrics over a fixed academic cluster of 45 open-access PDFs gathered from institutional hubs and Google Scholar.
* **Domain Focus:** Peer-reviewed journal research, university dissertations, and explicit Constitutional Court reviews (*Mudzuru & Anor v. Minister of Justice*).
* **Footprint Volume:** A comprehensive parsing pool tracking over 1,191 text layers representing roughly 437,596 distinct words.
* **Compliance Control:** Missing license metadata is treated conservatively by blocking public file distribution, isolating data processing tasks to local fair-use research execution boundaries only.

## Step-by-Step Execution Guide
1. **Workspace Initialization:** Load the project workbook file via an active Jupyter server, VS Code environment, or Google Colab container.
2. **Asset Allocation:** Build the target filesystem path `data/raw/` and populate it with your source document files. Alternatively, supply the file archive `data_raw.zip` directly inside the primary folder workspace when prompted by the runtime loop.
3. **API Key Setup:** Obtain an access token from the Groq console. Set this credential as an environment variable named `GROQ_API_KEY` on your machine. If you are using Google Colab, add the token through the Secrets panel sidebar and enable notebook access.
4. **Pipeline Execution:** Run the script code cells sequentially from top to bottom. The first system sweep automatically downloads the 90 MB local embedding model assets onto the local disk space.
5. **Interactive Console Usage:** Once the persistent user execution loop initializes at the base of the pipeline notebook, submit text inquiries through the input prompt or pass the exact string `exit` to disconnect the socket interface.

## System Evaluation and Performance Validation
The architecture was tested across a multi-tier framework consisting of single-document factual inquiries, multi-document synthesis challenges, and completely out-of-scope baseline injections.
* **Semantic Proximity Accuracy:** Proven through paired cosine distance testing, demonstrating accurate mathematical differentiation between overlapping themes (e.g., separating religious customs from standard educational metrics).
* **Prompt Integrity Control:** Grounding structures overrode standard vector parameters. Out-of-scope injections targeting alternate geographies (e.g., Nigerian statistical questions) or alternate industries (e.g., stock market pricing) were systematically rejected by the prompt engine rather than answered with hallucinations.
* **Traceability Indexing:** Final output answers successfully attached systematic `[filename, chunk_id]` source identifiers to every statement, allowing instant auditing of structural source documents.

## Contributing and Licensing
This system is provided exclusively for non-commercial academic study, international child-welfare policy assessment, and educational research. Code changes and extraction adaptations should follow standard version-control branching guidelines. All automation scripts utilize baseline open workspace permissions.
