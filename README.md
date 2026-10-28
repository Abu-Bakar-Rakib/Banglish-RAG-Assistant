# Banglish RAG Assistant

A self-hosted multilingual AI assistant for Bangla, English, and Banglish.

Banglish RAG Assistant is designed to understand natural language across Bangla, English, and Banglish (Bangla typed using English letters). It enables conversational Q&A over PDFs, images, and text using retrieval-augmented generation (RAG), OCR, and voice input in a single local workflow.

<p align="center">
  <img width="1258" height="685" alt="Banglish RAG Assistant interface" src="https://github.com/user-attachments/assets/2f84feae-eb40-4d48-aaa9-7480ca6729b1" />
</p>

## Overview

Banglish RAG Assistant is a privacy-friendly, locally deployable AI application for multilingual document understanding and conversational search. The system can answer questions from PDFs, markdown files, plain text, and images while also supporting voice-based interaction and public sharing via Gradio.

It is built for users who want a practical, self-hosted alternative to cloud-based AI tools, especially in Bangla-English mixed-language workflows.

## Features

- 💬 Multilingual chat in Bangla, English, and Banglish
- 📄 Document Q&A over PDFs, `.txt`, and `.md` files using retrieval-augmented generation
- 🖼️ Vision support for images and scanned PDF pages through Gemma 3 Vision
- 🎤 Voice input using Whisper `large-v3`
- 🔎 Smart retrieval with Banglish-aware query rewriting and cross-language search
- 🎛️ Three operating modes:
  - Auto (documents + general knowledge)
  - My documents only
  - General knowledge only
- 🌐 Public Gradio deployment with optional password protection
- 📎 Source-grounded responses with citation-style references such as `[1] [2]`

## Tech Stack

| Component | Tool |
|---|---|
| LLM + Vision | [Ollama](https://ollama.com) · `gemma3:4b` / `gemma3:12b` |
| Embeddings | `embeddinggemma` |
| Vector Database | ChromaDB |
| Speech-to-text | faster-whisper (`large-v3`) |
| PDF parsing | PyMuPDF |
| UI | Gradio 5 |

## How It Works

```text
Files / Voice / Text
      │
      ├─ PDF ─► text extraction or Vision OCR for scanned pages ─┐
      ├─ Image ─► Gemma Vision description + visible text ───────┤
      ├─ Audio ─► Whisper transcript ─► question ─────────────────┤
      └─ Text ─────────────────────────────────────────────────────┤
                                                                 │
                                                                 ▼
                                                   chunk ─► embeddinggemma ─► ChromaDB
                                                                 │
Question ─► query rewrite (Bangla + English) ─► vector search ─► top-k chunks
                                                                 │
                                             Gemma 3 + citations ─► streamed answer
```

The workflow combines document retrieval with a language-aware LLM pipeline to provide grounded answers while preserving multilingual usability.

## Quick Start (Kaggle)

1. Create a new Kaggle notebook and upload `banglish_rag_assistant.ipynb`.
2. Go to `Settings` and enable:
   - `Accelerator: GPU`
   - `Internet: On`
   - `T4 x2` is recommended for best performance
3. Run the notebook cells in order:
   - **Cell 1**: installs Ollama and required Python packages
   - **Cell 2**: starts the Ollama server and downloads the models
   - **Cell 3**: launches the Gradio app
4. Open the generated Gradio link shown in the notebook output.

> The public share link may expire after about one week. Re-run Cell 3 to generate a new link.

## Configuration

Edit the settings near the top of the notebook before running the application:

| Setting | Default | Notes |
|---|---:|---|
| `CHAT_MODEL` | `gemma3:4b` | Use `gemma3:12b` for stronger Bangla quality |
| `WHISPER_MODEL` | `large-v3` | Use `medium` or `small` on weaker GPUs |
| `CHUNK_WORDS` / `CHUNK_OVERLAP` | `150` / `30` | Controls chunking for retrieval |
| `DIST_MAX` | `0.65` | Relevance cutoff for auto mode; lower values are stricter |
| `REWRITE_QUERY` | `True` | Improves Banglish and cross-language search at the cost of one extra LLM call |
| `BANGLISH_REPLY` | `"banglish"` | Set to `"bangla"` to answer in Bangla script |
| `AUTH` | `None` | Use `("user", "password")` to protect the public link |

## Use Cases

This project is well suited for:

- Academic and research document Q&A
- Local knowledge assistants over PDFs and notes
- Bangla–English mixed-language conversations
- OCR-based exploration of scanned documents
- Privacy-focused AI workflows without sending files to external cloud providers

## Known Limitations

- 4B-class language models may occasionally produce inaccurate or incomplete answers, especially on OCR-heavy Bangla content. Important outputs should still be verified.
- Models and the vector database are stored in `/tmp`, so they are re-downloaded in each Kaggle session.
- Anyone with the public Gradio link can access the app unless `AUTH` is configured.

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

Contributions, suggestions, and improvements are welcome. If you have ideas for better Bangla retrieval, OCR quality, or a stronger user experience, feel free to open an issue or submit a pull request.
