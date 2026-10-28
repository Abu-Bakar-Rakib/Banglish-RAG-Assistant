# Banglish RAG Assistant

A free, self-hosted **ChatGPT-style assistant** that understands **বাংলা (Bangla), English and Banglish** (Bangla typed in English letters). Chat with your **PDFs, images and text**, or talk to it with your **voice**. It runs entirely on a Kaggle GPU using open models, with no paid APIs.

## ✨ Features

- 💬 **Multilingual chat**: replies in the same language you write (Bangla script, English, or Banglish)
- 📄 **Document Q&A (RAG)**: upload PDFs, `.txt`/`.md` files, or paste long text, with cited sources `[1] [2]`
- 🖼️ **Vision**: reads images and **scanned PDF pages** (OCR) using Gemma 3 vision
- 🎤 **Voice input**: speech-to-text with Whisper `large-v3` (Bangla + English)
- 🔎 **Smart retrieval**: LLM query rewriting for Banglish and cross-language search, plus relevance filtering
- 🎛️ **Three modes**: Auto (documents + general knowledge) · My documents only · General knowledge only
- 🌐 **Shareable public link** via Gradio (optional password)
<img width="1258" height="685" alt="image" src="https://github.com/user-attachments/assets/2f84feae-eb40-4d48-aaa9-7480ca6729b1" />

## 🧱 Tech Stack

| Part | Tool |
|---|---|
| LLM + Vision | [Ollama](https://ollama.com) · `gemma3:4b` (or `gemma3:12b`) |
| Embeddings | `embeddinggemma` |
| Vector DB | ChromaDB |
| Speech-to-text | faster-whisper (`large-v3`) |
| PDF parsing | PyMuPDF |
| UI | Gradio 5 |

## 🚀 Quick Start (Kaggle)

1. Create a new Kaggle notebook and upload `banglish_rag_assistant.ipynb`.
2. Settings → **Accelerator: GPU** (T4 x2 recommended) and **Internet: On**.
3. Run the cells in order:
   - **Cell 1**: installs Ollama and Python packages
   - **Cell 2**: starts the Ollama server and pulls the models
   - **Cell 3**: launches the Gradio app
4. Open the printed `https://xxxx.gradio.live` link.

> The share link expires after about 1 week. Re-run Cell 3 to get a new one.

## ⚙️ Configuration

Edit these at the top of the notebook cells:

| Setting | Default | Notes |
|---|---|---|
| `CHAT_MODEL` | `gemma3:4b` | Use `gemma3:12b` for better Bangla quality |
| `WHISPER_MODEL` | `large-v3` | Use `medium` or `small` on weaker GPUs |
| `CHUNK_WORDS` / `CHUNK_OVERLAP` | 150 / 30 | Chunk size for retrieval |
| `DIST_MAX` | 0.65 | Auto-mode relevance cutoff (lower = stricter) |
| `REWRITE_QUERY` | `True` | Improves Banglish search, costs one extra LLM call |
| `BANGLISH_REPLY` | `"banglish"` | Or `"bangla"` to answer Banglish in Bangla script |
| `AUTH` | `None` | Set `("user", "password")` to protect the public link |

## 🧠 How It Works

```
Files / Voice / Text
      │
      ├─ PDF ─► text (or Gemma vision OCR for scanned pages) ─┐
      ├─ Image ─► Gemma vision description + visible text ─────┤
      ├─ Audio ─► Whisper transcript ─► question                │
      └─ Text ────────────────────────────────────────────────┤
                                                                ▼
                               chunk ─► embeddinggemma ─► ChromaDB
                                                                │
 Question ─► query rewrite (BN + EN) ─► vector search ─► top-k chunks
                                                                │
                              Gemma 3 + citations ─► streamed answer
```

## ⚠️ Known Limitations

- 4B models can make mistakes, especially OCR on Bangla text. Verify important answers.
- Models and the vector DB live in `/tmp`, so they are re-downloaded each Kaggle session.
- Anyone with the public link can use the app unless `AUTH` is set.


## 📜 License

MIT, see [LICENSE](LICENSE).

## 🙏 Acknowledgements

[Ollama](https://ollama.com) · [Gemma](https://ai.google.dev/gemma) · [ChromaDB](https://www.trychroma.com) · [faster-whisper](https://github.com/SYSTRAN/faster-whisper) · [Gradio](https://gradio.app) · [PyMuPDF](https://pymupdf.readthedocs.io)
