# Banglish RAG Assistant

A free, self-hosted AI assistant for Bangla, English, and Banglish (Bangla typed in English letters). It brings together conversational AI, document understanding, OCR, and voice input in a single local workflow.

Whether you're asking questions from PDFs, images, or plain text, this app is designed to answer in the same language you use — Bangla script, English, or Banglish.

<p align="center">
  <img width="1258" height="685" alt="Banglish RAG Assistant interface" src="https://github.com/user-attachments/assets/2f84feae-eb40-4d48-aaa9-7480ca6729b1" />
</p>

## Overview

Banglish RAG Assistant is a lightweight, privacy-friendly alternative to cloud-based chat tools. It runs locally on your machine and supports:

- Multilingual chat in Bangla, English, and Banglish
- Retrieval-augmented generation (RAG) over PDFs, text, and markdown files
- Vision-based understanding for images and scanned PDFs
- Speech-to-text transcription for voice input
- Public sharing via Gradio with optional authentication

## Key Features

- 💬 Multilingual chat: replies in the same language you write, including Bangla script, English, and Banglish
- 📄 Document Q&A: upload PDFs, `.txt` files, `.md` files, or paste long text and get source-grounded answers with citations like `[1] [2]`
- 🖼️ Vision support: reads images and scanned PDF pages using Gemma 3 Vision
- 🎤 Voice input: converts speech to text using Whisper `large-v3`
- 🔎 Smart retrieval: performs Banglish-aware query rewriting and cross-language search for better matching
- 🎛️ Three operating modes:
  - Auto (documents + general knowledge)
  - My documents only
  - General knowledge only
- 🌐 Shareable public link: optional password protection for Gradio deployment

## Tech Stack

| Component | Tool |
|---|---|
| LLM + Vision | [Ollama](https://ollama.com) · `gemma3:4b` (or `gemma3:12b`) |
| Embeddings | `embeddinggemma` |
| Vector Database | ChromaDB |
| Speech-to-text | faster-whisper (`large-v3`) |
| PDF parsing | PyMuPDF |
| UI | Gradio 5 |

## How It Works

```text
Files / Voice / Text
      │
      ├─ PDF ─► text (or Gemma Vision OCR for scanned pages) ─┐
      ├─ Image ─► Gemma Vision description + visible text ─────┤
      ├─ Audio ─► Whisper transcript ─► question                │
      └─ Text ────────────────────────────────────────────────┤
                                                              ▼
                                               chunk ─► embeddinggemma ─► ChromaDB
                                                              │
Question ─► query rewrite (Bangla + English) ─► vector search ─► top-k chunks
                                                              │
                                             Gemma 3 + citations ─► streamed answer
```

The assistant combines retrieval with a language-aware LLM pipeline to ground answers in your documents while still supporting general knowledge queries.

## Quick Start (Kaggle)

1. Create a new Kaggle notebook and upload `banglish_rag_assistant.ipynb`.
2. Go to Settings and set:
   - Accelerator: GPU
   - Internet: On
   - T4 x2 is recommended for best performance
3. Run the notebook cells in order:
   - Cell 1: installs Ollama and required Python packages
   - Cell 2: starts the Ollama server and downloads the models
   - Cell 3: launches the Gradio app
4. Open the generated Gradio URL in the output.

> The public Gradio link typically expires after about one week. Re-run Cell 3 to generate a fresh link.

## Configuration

Edit the settings near the top of the notebook before running the app:

| Setting | Default | Notes |
|---|---:|---|
| `CHAT_MODEL` | `gemma3:4b` | Use `gemma3:12b` for stronger Bangla quality |
| `WHISPER_MODEL` | `large-v3` | Use `medium` or `small` on lower-end GPUs |
| `CHUNK_WORDS` / `CHUNK_OVERLAP` | `150` / `30` | Controls chunking for retrieval |
| `DIST_MAX` | `0.65` | Auto-mode relevance threshold; lower values are stricter |
| `REWRITE_QUERY` | `True` | Improves Banglish and cross-language search at the cost of one extra LLM call |
| `BANGLISH_REPLY` | `"banglish"` | Set to `"bangla"` to answer in Bangla script |
| `AUTH` | `None` | Set to `("user", "password")` to protect the public link |

## Use Cases

This project is useful for:

- Academic and research document Q&A
- Local knowledge assistants over PDFs and notes
- Bangla-English mixed-language workflows
- OCR-based document exploration
- Private, self-hosted AI experiences without sending documents to third-party APIs

## Known Limitations

- 4B-class models may occasionally produce incorrect or incomplete answers, especially on OCR-heavy Bangla text. Always verify important output.
- Models and the vector database are stored in `/tmp`, so they are re-downloaded in each Kaggle session.
- Anyone with the public link can access the app unless `AUTH` is configured.

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

Contributions, suggestions, and improvements are welcome. If you have ideas for better Bangla retrieval, OCR quality, or UI improvements, feel free to open an issue or submit a pull request.
