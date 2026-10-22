# Qore Code — a coding assistant that runs on your own computer

**Free · Windows · Mac · Linux · works offline**

<img width="1600" height="1000" alt="image" src="https://github.com/user-attachments/assets/816bb1a8-045e-4701-a9ae-a8ad9a2f204b" />


**A coding assistant that runs on your own computer, with no internet needed.** You tell it in normal English or Arabic what you want done to the files in a folder, and it does the work. It only ever touches the folder you chose.

**What it does for you**
- **Writes and fixes code:** new apps, games, websites and scripts; finds and fixes bugs; explains code simply; adds comments.
- **Checks its own work before saying "finished":** it runs your project's tests. When it makes a game or an app, it opens it and tries it: it presses the keys you asked for (for example "space to jump") and checks the game reacts; for an app that should save things, it adds an entry, closes the app, opens it again and checks the entry is still there. If something is broken it fixes it first. On Windows this happens out of view, so it never interrupts what you are doing.
- **Looks at what it built:** screenshots of games and apps, and it opens web pages and browser games to check for errors, missing files or a blank screen.
- **Suggests code as you type:** in the editor, gray text appears at the cursor with the rest of the line or the next lines; press Tab to insert it, Esc to hide it. A small code model on your own computer does this (Qwen2.5-Coder 1.5B Q4_K_M, or 0.5B Q8_0 on processors with fewer than 8 threads).
- **Undo for everything:** every change gets a restore point; go back with one click.

**Talk instead of typing**
- Press the microphone and speak in English or Arabic; your words appear in the message box so you can check them before sending.
- Hands-free: say "Core, Core" and then your request, and "Hash, Hash, Hash" to send it.
- It can read its replies aloud.
- Speech is turned into text on your own computer; no audio is sent anywhere.

**More built in**
- Reads text from images (English and Arabic).
- Makes pictures: type `/image` and a description (the picture model is a one-time download inside the app).
- Chat mode: just talk with the model, no file changes (`/chat`, back with `/agent`).
- A full desktop window (files, editor, saved chats, restore points) plus a terminal version (`qorecode-cli`).

**Why Qore Code**
- **Nothing else to install:** its own model engine is built in; no Python, no Ollama. Pick a model in its Download window, or use a `.gguf` file you already have (models from Ollama, LM Studio, GPT4All or Jan are found).
- **Works without a graphics card:** fast on a normal laptop processor, and it uses a graphics card when you have one.
- **Runs big models:** a Mixture-of-Experts model bigger than your memory still runs, reading from the disk, and you are told first.
- **Private and offline:** your code and your voice never leave your computer. Cloud models (OpenAI, OpenRouter, DeepSeek, Groq) are optional, with your own key.
- **Proven:** same local models (Qwen3-Coder 30B-A3B Q8_0, Qwen3-Coder-Next 80B Q8_0, Llama 3.3 70B Q8_0), same 24 tasks: Qore Code finished 14, opencode 9, Qwen Code 10.
- **English and Arabic** throughout: chat, voice and text reading.
- **Does ordinary developer work without refusing:** it adds no content rules of its own; the only things it refuses are the limits you set, such as a file you told it not to touch.

## ⬇️ Download (2026.10.22)

| | |
|---|---|
| **Windows 10/11 (64-bit)** | [QoreCode-2026.10.22-windows-setup.exe](https://github.com/Q8core/Qore-Code/releases/download/v2026.10.22/QoreCode-2026.10.22-windows-setup.exe). One installer file, no admin needed. If Windows says *"Windows protected your PC"* (the file is not code-signed), click **More info → Run anyway**. |
| **Mac** (Apple Silicon M1–M4, macOS 14+) | [QoreCode-2026.10.18.3-macos.zip](https://github.com/Q8core/Qore-Code/releases/download/v2026.10.18.3/QoreCode-2026.10.18.3-macos.zip) (the 2026.10.18.3 build). Double-click it and drag **Qore Code.app** into Applications. The first time, macOS says it cannot check the app (it is not signed with a paid Apple certificate): click **Done**, then **System Settings → Privacy & Security → Open Anyway**. |
| **Linux** (Mint 22+ / Ubuntu 24.04+, 64-bit) | [QoreCode-2026.10.22-linux.zip](https://github.com/Q8core/Qore-Code/releases/download/v2026.10.22/QoreCode-2026.10.22-linux.zip). Unzip it and double-click **QoreCode**, or run `./QoreCode`. `./Add-to-applications-menu.sh` adds a menu entry. |

## How it compares — benchmark (2026-10-19)

Qore Code, opencode and Qwen Code, each driving the **same local Q8 models** (Qwen3-Coder 30B-A3B Q8_0, Qwen3-Coder-Next 80B Q8_0, Llama 3.3 70B Q8_0) with the **same plain-language prompts**, the same 10-minute limit and the same automatic grader. Nothing any tool produced was edited.

| Tool | Tasks passed (of 24) |
|---|---|
| **Qore Code** | **14 (58%)** |
| opencode | 9 (38%) |
| Qwen Code | 10 (42%) |

Every task, model and time: **[BENCHMARK.md](BENCHMARK.md)** · full report (Word): **[Qore-Code-Benchmark-2026-10-19.docx](Qore-Code-Benchmark-2026-10-19.docx)**

Each task was run once per tool and model. Speed measurements and later results: **[ROADMAP.md](ROADMAP.md)**

## Compared with other coding tools

Qore Code checked against its 2026.10.22 release; the other tools from their own documentation, October 2026. "Unclear" means we could not confirm it.

| | Qore Code | Continue | VS Code + Copilot | LM Studio |
|---|---|---|---|---|
| **What it is** | Coding agent with its own model engine | IDE extension + terminal agent | Code editor + assistant | Local model runner + chat app |
| Own window (no IDE needed) | ✅ Files, editor, chats, restore points | Partial: terminal agent; chat lives in the IDE | ✅ It is the editor | ✅ Chat window |
| Terminal / script mode | ✅ `qorecode-cli`, `-p`, `--json`, exit codes | ✅ `cn`, `cn -p` | Partial: separate Copilot CLI | ✅ `lms chat` |
| **Models** | | | | |
| Own built-in model engine | ✅ Runs `.gguf` on CPU and GPU | ❌ Needs Ollama, LM Studio or an API | ❌ Cloud, or local through a provider | ✅ llama.cpp, MLX |
| Works without Ollama | ✅ Nothing else to install | Partial: local models need Ollama, LM Studio or similar | Partial: local models need a provider | ✅ |
| Uses your Ollama models | ✅ Runs their files directly, or uses your Ollama server | ✅ Through Ollama | Partial: through Ollama | ❌ Own model library |
| Uses your LM Studio models | ✅ Finds the files, detects a running LM Studio | ✅ | Partial: bring your own model | It is LM Studio |
| Model bigger than memory | ✅ Mixture-of-Experts models run from the disk | Depends on the server | Depends on the server | Unclear |
| Cloud models | ✅ Optional: OpenAI, OpenRouter, DeepSeek, Groq | ✅ Many providers | ✅ Many cloud models | ❌ |
| Works fully offline | ✅ | ✅ With a local model | ❌ Needs a GitHub sign-in | ✅ |
| **Coding** | | | | |
| Suggestions as you type | ✅ Gray text, Tab inserts; offline on the processor | ✅ In the IDE extension | ✅ | ❌ |
| Agent edits files | ✅ | ✅ | ✅ Agent mode | ❌ Chat only |
| Runs commands and tests | ✅ Runs the project's tests before saying done | ✅ | ✅ | ❌ |
| Tries the app it built | ✅ Presses keys, reopens apps, checks web pages | ❌ Not built in | Unclear | ❌ |
| Undo / restore points | ✅ Every change, one click | Partial: editor undo | ✅ Checkpoints | — |
| Understands the codebase | ✅ Ranked file map + language server | ✅ Index + @-context | ✅ Workspace index | ❌ |
| Plain chat mode | ✅ `/chat` | ✅ | ✅ | ✅ |
| **Extras** | | | | |
| Voice in and out | ✅ English + Arabic, offline, hands-free | ❌ | Partial: VS Code Speech add-on | ❌ |
| Built-in text reading from images (OCR) | ✅ English + Arabic, offline | ❌ Only through a vision model | ❌ Only through a vision model | ❌ Only through a vision model |
| Makes pictures | ✅ `/image` | ❌ | ❌ | ❌ |
| Sub-agents | ✅ | Unclear | ✅ | ❌ |
| MCP tools | ✅ | ✅ | ✅ | ✅ |
| Hooks (block / log tool calls) | ✅ | Unclear | ✅ | ❌ |
| Memory across sessions | ✅ | Partial: rules files | Partial | ❌ |
| API for other apps | ❌ Engine is internal | ❌ | ❌ | ✅ `localhost:1234/v1` |
| **Platform and cost** | | | | |
| Windows / Linux | ✅ / ✅ | ✅ / ✅ | ✅ / ✅ | ✅ / ✅ |
| macOS | ✅ Apple Silicon, macOS 14+ (2026.10.18.3 build) | ✅ | ✅ | ✅ |
| Open source | ❌ | ✅ Apache 2.0 | Partial: editor yes, Copilot no | ❌ |
| Price | Free | Free | Free tier; Pro about $10/month | Free |

## Good to know

- How fast and how well it answers depends on the model you pick and on your computer: a bigger model is smarter, but it needs more memory and is slower.
- Your settings and chats stay in your home folder, so a newer version can replace the app folder.

<div dir="rtl">

## بالعربي

**Qore Code** مساعد برمجة يعمل على جهازك وبدون إنترنت: اطلب ما تريد بلغة بسيطة، وهو يقرأ ويعدّل الملفات داخل المجلد الذي تختاره فقط. محرّك النماذج مدمج، فلا حاجة لتثبيت أي شيء آخر. مجاني لويندوز وماك ولينكس. تحدّث بدل الكتابة: اضغط زر الميكروفون وتكلّم بالعربية أو الإنجليزية، ويُحوَّل كلامك إلى نص على جهازك دون إرسال أي صوت إلى الإنترنت.

</div>

---

Made by **[Q8Core](https://q8core.com)**. More apps at [q8core.com](https://q8core.com). Free to use; all rights reserved.
