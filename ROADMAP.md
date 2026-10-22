# Roadmap

Updated 2026-10-22. Latest version: 2026.10.22 (Windows and Linux; the Mac build is 2026.10.18).

## Benchmark

Same local Q8 models, prompts, 10-minute limit and grader for all three tools ([BENCHMARK.md](BENCHMARK.md)).

| Tool | Passed (of 24) |
|---|---|
| **Qore Code** | **14** |
| opencode | 9 |
| Qwen Code | 10 |

## Agent mode vs the model alone, 2026-10-20

The same tasks, run once with Qore Code as normal (Agent mode) and once with the model alone (Chat mode: the code from its answer saved into the files it named and graded the same way). All results checked by hand.

| Model | Agent mode | Model alone |
|---|---|---|
| Qwen3.8-27B Q8_0 | 5 / 6 | 4 / 6 |
| Qwen3-Coder-Next 80B Q8_0 | **9 / 10** | 5 / 9 |
| **Total** | **14 / 16** | 9 / 15 |

Setup: RTX PRO 6000 Blackwell 96 GB, temperature 0.1, seed 20261019, 32k context.

## Pictures, 2026-10-21

Each model was asked for a picture in the real app, once with a short prompt and once with a longer one saved in the project as bakery.png.

| Model | Pictures made | Knows the operating system |
|---|---|---|
| Qwen3.8-27B Q8_0 | 2 / 2 | yes |
| Qwen3-Coder 30B-A3B Q8_0 | 2 / 2 | yes |
| Qwen3-Coder-Next 80B Q8_0 | 2 / 2 | yes |
| Llama 3.3 70B Q8_0 | 2 / 2 | yes |
| **Total** | **8 / 8** | **4 / 4** |

## Speed without a graphics card, 2026.10.21.3

The built-in engine multiplies the model's packed weights with 8-bit integer instructions. Measured on a Windows 11 laptop with an Intel N150 and no graphics card:

| Qwen3-4B Q4_K_M on the N150 processor | 2026.10.21.2 | 2026.10.21.3 |
|---|---|---|
| Writing the answer | 0.75 tokens/s | 3–4 tokens/s |
| Reading the request | 3.7 tokens/s | 7 tokens/s |
| A follow-up question in the app | 13.7 s | 4.2 s |

Same answers: the integer path picks the same word as the exact path at 59 of 60 steps (the other is a near tie of 0.1). Qwen3.8-27B Q4_K_M writes 3.5 times as fast (0.76 → 2.67 tokens/s on an RTX 4090 machine).

## Real requests on Windows 11

| Request | Result |
|---|---|
| "Make a pygame game … run it" | The game ran in the background with the project's own `.venv`; the turn finished and nothing was left running |
| "A notes app that keeps my notes after I close it" | Qore Code typed a note, closed the app, reopened it and found the note before saying done |
| "A browser game in index.html … check it in the browser" | Opened in a headless browser; the picture and the page's errors came back to the model |
| "Start a Flask app and leave it running" | Started in the background; the turn ended instead of waiting |
| A program that prints Arabic | Printed correctly |

On Windows, the game and app checks run out of sight, without taking the keyboard (checked while typing in another window).

## Code suggestions while typing, 2026.10.22

The editor suggests code as you type: gray text at the cursor, Tab inserts it. A small fill-in-the-middle code model runs in its own engine process on the processor, so chat answers do not slow down. Qwen2.5-Coder 1.5B Q4_K_M is used on processors with 8 threads or more, Qwen2.5-Coder 0.5B Q8_0 on smaller ones.

Checked in the installed app, typing real keys (time from the last key to the first gray text):

| Machine and model | Python: `def calculate_total(` | JavaScript: `for ` | HTML: `<li>` |
|---|---|---|---|
| Intel N150 laptop, Windows 11 · Qwen2.5-Coder 0.5B Q8_0 | 1.47 s | 0.70 s | 1.38 s |
| Intel i5-12400F, Linux · Qwen2.5-Coder 1.5B Q4_K_M | 0.60 s | 0.37 s | 0.66 s |

In every case Tab inserted a complete, correct block (a whole function, a `for` loop, a list item with its closing tag) and the file saved as shown.

## Next

1. **Qwen3.8 on the graphics card.** Move the feed-forward part of every layer to the card when it fits.
2. **Earlier app checks in long turns**, so the result reaches the model with more time to act on it.
3. **Benchmark v2.** Multiple runs per task, more task types (bug fixes in existing code, refactoring, tests), and all prompts, outputs, logs and grader code published.
