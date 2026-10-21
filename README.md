# Qore Code — a coding helper that runs on your own computer

**Free · Windows · Mac · Linux · works offline**

<img width="1600" height="1000" alt="image" src="https://github.com/user-attachments/assets/816bb1a8-045e-4701-a9ae-a8ad9a2f204b" />


Ask for anything you want done to the files in a folder, in plain English: "What does this project do?", "Find any bugs in this code and explain them simply", "Add comments to the main file so I can understand it". Qore Code reads and edits files **only inside the folder you pick**.

- **Its own model engine is built in.** No Python, no Ollama, nothing else to install. Pick a model from the Download window, or add a `.gguf` file you already have (models from Ollama, LM Studio, GPT4All or Jan are found automatically).
- **Runs where it fits:** graphics card + memory, graphics card only, or processor only. A model bigger than your memory still runs from the drive, and you're told first.
- **Undo for every change:** each turn that edits files gets its own restore point.
- **Built-in text reading from images (OCR)** in English and Arabic.
- **Pictures:** ask in the chat (`/image a red lighthouse at sunset`). The picture model is a one-time 20.5 GB download inside the app.
- **Optional cloud models:** already have an OpenAI, OpenRouter, DeepSeek or Groq account? Paste your own API key to use their models.
- **Chat mode:** switch it on to talk to the model directly, with no tools and no checks. Type `/chat` to switch on and `/agent` to switch back.
- Desktop app plus a terminal version (`qorecode-cli`).

## ⬇️ Download (2026.10.21)

| | |
|---|---|
| **Windows 10/11 (64-bit)** | [QoreCode-2026.10.21-windows-setup.exe](https://github.com/Q8core/Qore-Code/releases/download/v2026.10.21/QoreCode-2026.10.21-windows-setup.exe). One installer file, no admin needed. If Windows says *"Windows protected your PC"* (the file is not code-signed), click **More info → Run anyway**. |
| **Mac** (Apple Silicon M1–M4, macOS 14+) | [QoreCode-2026.10.18.3-macos.zip](https://github.com/Q8core/Qore-Code/releases/download/v2026.10.18.3/QoreCode-2026.10.18.3-macos.zip) (the 2026.10.18 build; the Mac build of 2026.10.21 is not ready yet). Double-click it and drag **Qore Code.app** into Applications. The first time, macOS says it cannot check the app (it is not signed with a paid Apple certificate): click **Done**, then **System Settings → Privacy & Security → Open Anyway**. |
| **Linux** (Mint 22+ / Ubuntu 24.04+, 64-bit) | [QoreCode-2026.10.21-linux.zip](https://github.com/Q8core/Qore-Code/releases/download/v2026.10.21/QoreCode-2026.10.21-linux.zip). Unzip it and double-click **QoreCode**, or run `./QoreCode`. `./Add-to-applications-menu.sh` adds a menu entry. |

## How it compares — benchmark (2026-10-19)

Qore Code, opencode and Qwen Code, each driving the **same local Q8 models** (Qwen3-Coder 30B, Qwen3-Coder-Next 80B, Llama 3.3 70B) with the **same plain-language prompts**, the same 10-minute limit and the same automatic grader. Nothing any tool produced was edited. Grading corrected 2026-10-20: six grader mistakes were fixed and every row was graded again (the first version said 12 / 7 / 7).

| Tool | Tasks passed (of 24) |
|---|---|
| **Qore Code** | **14 (58%)** |
| opencode | 9 (38%) |
| Qwen Code | 10 (42%) |

Every task, model and time: **[BENCHMARK.md](BENCHMARK.md)** · full report (Word): **[Qore-Code-Benchmark-2026-10-19.docx](Qore-Code-Benchmark-2026-10-19.docx)**

Each task was run once per tool and model, so a difference of a few tasks is within run-to-run variation.

Later Qore Code runs with the fixes this benchmark led to, where it still fails, and what is next: **[ROADMAP.md](ROADMAP.md)**

## Good to know

- How fast and how well it answers depends on the model you pick and on your computer: a bigger model is smarter, but it needs more memory and is slower.
- Your settings and chats stay in your home folder, so a newer version can replace the app folder.

<div dir="rtl">

## بالعربي

**Qore Code** مساعد برمجة يعمل على جهازك وبدون إنترنت: اطلب ما تريد بلغة بسيطة، وهو يقرأ ويعدّل الملفات داخل المجلد الذي تختاره فقط. محرّك النماذج مدمج، فلا حاجة لتثبيت أي شيء آخر. مجاني لويندوز وماك ولينكس.

</div>

---

Made by **[Q8Core](https://q8core.com)**. More apps at [q8core.com](https://q8core.com). Free to use; all rights reserved.
