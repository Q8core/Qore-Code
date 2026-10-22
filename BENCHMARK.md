# Qore Code vs opencode vs Qwen Code — local Q8 coding-agent benchmark

Same local Q8 models, same plain-language prompts, same 10-minute limit, same automatic grader. Nothing any tool produced was edited. Measured 2026-10-19; grading corrected 2026-10-20 (six grader fixes, every row re-graded — see the Notes in the full report). Full report (Word): [Qore-Code-Benchmark-2026-10-19.docx](Qore-Code-Benchmark-2026-10-19.docx).

## Headline

| Tool | Passed | Run | Rate |
|---|---|---|---|
| Qore Code | 14 | 24 | 58% |
| opencode | 9 | 24 | 38% |
| Qwen Code | 10 | 24 | 42% |

## Results by task

| Task | Model | Qore Code | opencode | Qwen Code |
|---|---|---|---|---|
| Simple app: tip calculator | Qwen3-Coder 30B-A3B (MoE) | **PASS** · 600s (cap) | **PASS** · 14s | **PASS** · 25s |
| Simple app: tip calculator | Qwen3-Coder-Next 80B (MoE) | **PASS** · 107s | **PASS** · 110s | **PASS** · 110s |
| Simple app: tip calculator | Llama 3.3 70B (dense) | **PASS** · 77s | fail · 57s | fail · 81s |
| Mid app: to-do list that saves | Qwen3-Coder 30B-A3B (MoE) | **PASS** · 362s | **PASS** · 40s | fail · 99s |
| Mid app: to-do list that saves | Qwen3-Coder-Next 80B (MoE) | **PASS** · 193s | fail · 34s | **PASS** · 112s |
| Mid app: to-do list that saves | Llama 3.3 70B (dense) | fail · 108s | fail · 116s | fail · 40s |
| Semi-complex app: notes with search + save | Qwen3-Coder 30B-A3B (MoE) | **PASS** · 312s | **PASS** · 190s | **PASS** · 118s |
| Semi-complex app: notes with search + save | Qwen3-Coder-Next 80B (MoE) | fail · 273s | fail · 331s | **PASS** · 98s |
| Semi-complex app: notes with search + save | Llama 3.3 70B (dense) | **PASS** · 104s | fail · 107s | fail · 102s |
| 2D game: jump over obstacles | Qwen3-Coder 30B-A3B (MoE) | **PASS** · 600s (cap) | fail · 38s | **PASS** · 74s |
| 2D game: jump over obstacles | Qwen3-Coder-Next 80B (MoE) | **PASS** · 228s | **PASS** · 38s | **PASS** · 29s |
| 2D game: jump over obstacles | Llama 3.3 70B (dense) | fail · 324s | fail · 79s | fail · 106s |
| 3D game: walk + pick up + counter | Qwen3-Coder 30B-A3B (MoE) | **PASS** · 600s (cap) | fail · 44s | fail · 44s |
| 3D game: walk + pick up + counter | Qwen3-Coder-Next 80B (MoE) | fail · 372s | fail · 28s | **PASS** · 84s |
| 3D game: walk + pick up + counter | Llama 3.3 70B (dense) | fail · 319s | fail · 93s | fail · 173s |
| Debug: fix a broken program | Qwen3-Coder 30B-A3B (MoE) | **PASS** · 17s | **PASS** · 23s | fail · 26s |
| Debug: fix a broken program | Qwen3-Coder-Next 80B (MoE) | **PASS** · 50s | **PASS** · 103s | **PASS** · 81s |
| Debug: fix a broken program | Llama 3.3 70B (dense) | fail · 48s | fail · 58s | fail · 79s |
| 3D modeling: a house file | Qwen3-Coder-Next 80B (MoE) | **PASS** · 117s | **PASS** · 311s | fail · 68s |
| 3D modeling: a house file | Llama 3.3 70B (dense) | fail · 266s | fail · 20s | fail · 43s |
| Serious app: expense tracker + chart | Qwen3-Coder-Next 80B (MoE) | fail · 600s (cap) | fail · 600s (cap) | fail · 453s |
| Serious app: expense tracker + chart | Llama 3.3 70B (dense) | fail · 144s | fail · 114s | fail · 135s |
| SSH: script that logs in and lists files | Qwen3-Coder-Next 80B (MoE) | **PASS** · 133s | **PASS** · 126s | **PASS** · 97s |
| SSH: script that logs in and lists files | Llama 3.3 70B (dense) | fail · 88s | fail · 43s | fail · 34s |

## Qore Code after the fixes this benchmark led to

| Task | Model | Shipped build | Fixed build |
|---|---|---|---|
| Simple app: tip calculator | Qwen3-Coder 30B-A3B (MoE) | **PASS** · 600s (cap) | **PASS** · 68s |
| Simple app: tip calculator | Qwen3-Coder-Next 80B (MoE) | **PASS** · 107s | **PASS** · 51s |
| Mid app: to-do list that saves | Qwen3-Coder 30B-A3B (MoE) | **PASS** · 362s | **PASS** · 192s |
| Mid app: to-do list that saves | Qwen3-Coder-Next 80B (MoE) | **PASS** · 193s | **PASS** · 97s |
| Semi-complex app: notes with search + save | Qwen3-Coder 30B-A3B (MoE) | **PASS** · 312s | fail · 210s |
| Semi-complex app: notes with search + save | Qwen3-Coder-Next 80B (MoE) | fail · 273s | **PASS** · 531s |
| 2D game: jump over obstacles | Qwen3-Coder 30B-A3B (MoE) | **PASS** · 600s (cap) | **PASS** · 232s |
| 2D game: jump over obstacles | Qwen3-Coder-Next 80B (MoE) | **PASS** · 228s | **PASS** · 173s |
| 3D game: walk + pick up + counter | Qwen3-Coder 30B-A3B (MoE) | **PASS** · 600s (cap) | fail · 188s |
| 3D game: walk + pick up + counter | Qwen3-Coder-Next 80B (MoE) | fail · 372s | fail · 427s |
| Debug: fix a broken program | Qwen3-Coder 30B-A3B (MoE) | **PASS** · 17s | **PASS** · 25s |
| Debug: fix a broken program | Qwen3-Coder-Next 80B (MoE) | **PASS** · 50s | **PASS** · 38s |
| 3D modeling: a house file | Qwen3-Coder-Next 80B (MoE) | **PASS** · 117s | **PASS** · 285s |
| Serious app: expense tracker + chart | Qwen3-Coder-Next 80B (MoE) | fail · 600s (cap) | fail · 366s |
| SSH: script that logs in and lists files | Qwen3-Coder-Next 80B (MoE) | **PASS** · 133s | **PASS** · 31s |
| Image full-loop | Qwen3-Coder-Next 80B (MoE) | fail · 2401s (cap) | fail · 442s |

Setup: 1 × RTX PRO 6000 Blackwell 96 GB · Ollama 0.40.6 · temperature 0.1, seed 20261019, 32k context · Qore Code 2026.10.18 (844248d) · opencode 1.19.3 · Qwen Code 0.15.10.

## Built-in engine speed, 2026.10.21.3

Qore Code's own engine (no Ollama), temperature 0, same model file in both columns. Speeds in tokens per second, from the engine's own timings.

| Machine and model | Measure | 2026.10.21.2 | 2026.10.21.3 |
|---|---|---|---|
| Intel N150 laptop, 4 cores, no graphics card · Qwen3-4B Q4_K_M | writing | 0.75 | 3.0–3.4 |
| same | reading a ~500-token request | 3.7 | 6.7–7.1 |
| same, in the app | follow-up question, total | 13.7 s | 4.2 s |
| Cloud server, 128 CPU threads · Qwen3-4B Q4_K_M | writing | 5.4 | 10.3 |
| RTX 4090 machine · Qwen3.8-27B Q4_K_M (attention on the card, the rest on the processor) | writing | 0.76 | 2.67 |

Same answers: perplexity 17.50 → 17.49 on the same text; along the reference answers the new path picks the same token at 59 of 60 steps (the other is a near tie, gap 0.1). The exact path still matches the reference token for token on the processor, on the card, and split between them.

Where Qore Code still fails, why, and what we are fixing next: **[ROADMAP.md](ROADMAP.md)**.
