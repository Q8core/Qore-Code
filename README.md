# Qore Code — a coding assistant that runs on your own computer

**Free · Windows · Mac · Linux · works offline**

<img width="1600" height="1000" alt="image" src="https://github.com/user-attachments/assets/816bb1a8-045e-4701-a9ae-a8ad9a2f204b" />


**A coding assistant that runs on your own computer, with no internet needed.** You tell it in normal English or Arabic what you want done to the files in a folder, and it does the work. It only ever touches the folder you chose.

**What it does for you**
- **Writes and fixes code:** new apps, games, websites and scripts; finds and fixes bugs; explains code simply; adds comments.
- **Checks its own work before saying "finished":** it runs your project's tests. When it makes a game or an app, it opens it and tries it: it presses the keys you asked for (for example "space to jump") and checks the game reacts; for an app that should save things, it adds an entry, closes the app, opens it again and checks the entry is still there. If something is broken it fixes it first. On Windows this happens out of view, so it never interrupts what you are doing.
- **Looks at what it built:** screenshots of games and apps, and it opens web pages and browser games to check for errors, missing files or a blank screen.
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
- **Runs big models:** a model bigger than your memory still runs from the disk, and you are told first.
- **Private and offline:** your code and your voice never leave your computer. Cloud models (OpenAI, OpenRouter, DeepSeek, Groq) are optional, with your own key.
- **Proven:** same models, same 24 tasks: Qore Code finished 14, opencode 9, Qwen Code 10.
- **English and Arabic** throughout: chat, voice and text reading.
- **Does ordinary developer work without refusing:** it adds no content rules of its own; the only things it refuses are the limits you set, such as a file you told it not to touch.

## ⬇️ Download (2026.10.21.3)

| | |
|---|---|
| **Windows 10/11 (64-bit)** | [QoreCode-2026.10.21.3-windows-setup.exe](https://github.com/Q8core/Qore-Code/releases/download/v2026.10.21.3/QoreCode-2026.10.21.3-windows-setup.exe). One installer file, no admin needed. If Windows says *"Windows protected your PC"* (the file is not code-signed), click **More info → Run anyway**. |
| **Mac** (Apple Silicon M1–M4, macOS 14+) | [QoreCode-2026.10.18.3-macos.zip](https://github.com/Q8core/Qore-Code/releases/download/v2026.10.18.3/QoreCode-2026.10.18.3-macos.zip) (the 2026.10.18 build; the Mac build of 2026.10.21 is not ready yet). Double-click it and drag **Qore Code.app** into Applications. The first time, macOS says it cannot check the app (it is not signed with a paid Apple certificate): click **Done**, then **System Settings → Privacy & Security → Open Anyway**. |
| **Linux** (Mint 22+ / Ubuntu 24.04+, 64-bit) | [QoreCode-2026.10.21.3-linux.zip](https://github.com/Q8core/Qore-Code/releases/download/v2026.10.21.3/QoreCode-2026.10.21.3-linux.zip). Unzip it and double-click **QoreCode**, or run `./QoreCode`. `./Add-to-applications-menu.sh` adds a menu entry. |

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

**Qore Code** مساعد برمجة يعمل على جهازك وبدون إنترنت: اطلب ما تريد بلغة بسيطة، وهو يقرأ ويعدّل الملفات داخل المجلد الذي تختاره فقط. محرّك النماذج مدمج، فلا حاجة لتثبيت أي شيء آخر. مجاني لويندوز وماك ولينكس. تحدّث بدل الكتابة: اضغط زر الميكروفون وتكلّم بالعربية أو الإنجليزية، ويُحوَّل كلامك إلى نص على جهازك دون إرسال أي صوت إلى الإنترنت.

</div>

---

Made by **[Q8Core](https://q8core.com)**. More apps at [q8core.com](https://q8core.com). Free to use; all rights reserved.
