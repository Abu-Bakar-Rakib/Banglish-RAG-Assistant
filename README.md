# Banglish RAG Assistant

> A self-hosted multilingual AI assistant for Bangla, English, and Banglish.

Banglish RAG Assistant is a privacy-focused, locally deployable AI application designed to understand and respond in Bangla, English, and Banglish (Bangla typed using English letters). It enables natural conversation and grounded Q&A over PDFs, images, and text documents through a retrieval-augmented generation (RAG) pipeline.

<p align="center">
  <img width="1258" height="685" alt="Banglish RAG Assistant interface" src="https://github.com/user-attachments/assets/2f84feae-eb40-4d48-aaa9-7480ca6729b1" />
</p>

## Overview

The project combines multilingual chat, document understanding, OCR, and voice input into a single workflow. Users can ask questions about PDFs, scanned pages, images, and text files and receive grounded answers with source-based citations.

It is built for users who want a practical, self-hosted alternative to cloud-based AI tools—especially for Bangla-English mixed-language workflows, research, education, and local knowledge systems.

## Why This Project

- Understands Bangla, English, and Banglish in the same conversation
- Answers questions using your own documents and data
- Works with PDFs, text files, images, and scanned content
- Supports voice-based interaction through speech-to-text
- Runs locally with optional public sharing via Gradio

## Key Features

- 💬 Multilingual chat across Bangla, English, and Banglish
- 📄 RAG-powered document Q&A for PDFs, `.txt`, and `.md` files
- 🖼️ Vision support for images and scanned documents using Gemma 3 Vision
- 🎤 Speech-to-text input using Whisper `large-v3`
- 🔎 Smart retrieval with Banglish-aware query rewriting and cross-language search
- 🎛️ Three retrieval modes:
  - Auto: documents + general knowledge
  - My documents only
  - General knowledge only
- 🌐 Shareable Gradio deployment with optional authentication
- 📎 Source-aware responses with citation references such as `[1] [2]`

## Technology Stack

| Component | Tool |
|---|---|
| LLM + Vision | [Ollama](https://ollama.com) · `gemma3:4b` / `gemma3:12b` |
| Embeddings | `embeddinggemma` |
| Vector Database | ChromaDB |
| Speech-to-text | faster-whisper (`large-v3`) |
| PDF parsing | PyMuPDF |
| Interface | Gradio 5 |

## System Workflow

```text
Files / Voice / Text
      │
      ├─ PDF ─► text extraction or OCR for scanned pages ─┐
      ├─ Image ─► Gemma Vision description + visible text ─────┤
      ├─ Audio ─► Whisper transcript ─► question ─────────────┤
      └─ Text ─────────────────────────────────────────────────┤
                                                            ▼
                                           chunk ─► embeddinggemma ─► ChromaDB
                                                            │
Question ─► query rewrite (Bangla + English) ─► vector search ─► top-k chunks
                                                            │
                                          Gemma 3 + citations ─► streamed answer
```

## Quick Start (Kaggle)

1. Create a new Kaggle notebook and upload `banglish_rag_assistant.ipynb`.
2. Open the notebook settings and enable:
   - `Accelerator: GPU`
   - `Internet: On`
   - `T4 x2` is recommended for best performance
3. Run the cells in order:
   - **Cell 1**: installs Ollama and required Python packages
   - **Cell 2**: starts the Ollama server and downloads the models
   - **Cell 3**: launches the Gradio application
4. Open the generated `https://xxxx.gradio.live` URL.

> The public Gradio link may expire after a few days. Re-run Cell 3 to generate a new link.

## Configuration

Adjust the settings near the top of the notebook before launching the app:

| Setting | Default | Notes |
|---|---:|---|
| `CHAT_MODEL` | `gemma3:4b` | Use `gemma3:12b` for stronger Bangla quality |
| `WHISPER_MODEL` | `large-v3` | Use `medium` or `small` on lower-end GPUs |
| `CHUNK_WORDS` / `CHUNK_OVERLAP` | `150` / `30` | Controls chunking behavior during retrieval |
| `DIST_MAX` | `0.65` | Relevance cutoff for Auto mode; lower values are stricter |
| `REWRITE_QUERY` | `True` | Improves Banglish and cross-language search at the cost of one extra LLM call |
| `BANGLISH_REPLY` | `"banglish"` | Set to `"bangla"` to respond in Bangla script |
| `AUTH` | `None` | Set to `("user", "password")` to protect the public link |

## Use Cases

This project is well suited for:

- Academic and research document Q&A
- Local knowledge assistants for PDF and note-based workflows
- Bangla-English mixed-language understanding
- OCR-based exploration of scanned documents
- Privacy-conscious AI deployment without relying on external cloud APIs

## Known Limitations

- 4B-class models may occasionally generate incorrect or incomplete output, especially on OCR-heavy Bangla content. Important results should still be verified.
- Models and the vector database are stored in `/tmp`, so they are re-downloaded on each Kaggle session.
- Anyone who has the public link can access the app unless `AUTH` is configured.

## License

This project is licensed under the [MIT License](LICENSE).

## Acknowledgements

- [Ollama](https://ollama.com)
- [Gemma](https://ai.google.dev/gemma)
- [ChromaDB](https://www.trychroma.com)
- [faster-whisper](https://github.com/SYSTRAN/faster-whisper)
- [Gradio](https://gradio.app)
- [PyMuPDF](https://pymupdf.readthedocs.io)

## Contributing

Contributions, suggestions, and improvements are welcome. If you have ideas for improving Bangla retrieval, OCR quality, or the overall user experience, please open an issue or submit a pull request.
