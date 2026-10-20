# Roadmap

Updated 2026-10-20.

## Status

Benchmark ([BENCHMARK.md](BENCHMARK.md)): same local Q8 models, prompts, 10-minute limit and grader for all three tools.

| Tool | Passed (of 24) |
|---|---|
| Qore Code | 14 |
| opencode | 9 |
| Qwen Code | 10 |

Each cell is a single run, so a difference of a few tasks is within run-to-run variation. The first published version (12 / 7 / 7) had grader bugs; six were fixed and all rows re-graded. All changes were fail → pass, for all three tools.

Failures in the shipped Qore Code build (from the run logs):

| Task (model) | Cause |
|---|---|
| Notes app (30B) | New Note button raised an exception (missing import). Qore Code's check found it after the 40-step limit, with no steps left to fix it. |
| Expense tracker (80B) | App did not start: a required library was not installed. The final check ran code checks only and never launched the app. |
| 3D game (30B) | 40-step limit reached with most of the time left. Scene file written in the wrong format (model). |
| 3D game (80B) | Model rewrote the same file unchanged; the repeat detector ended the turn. |
| Image full-loop (80B) | Model drew the background in code instead of calling the image tool; no check covered this. |

Llama 3.3 70B produced no working program in most runs with any tool (Qore Code 2/9, others 0/9).

## Changes after the benchmark (round 1)

1. **App check before finishing.** Qore Code launches the app on a hidden display and checks that it starts. For games it sends the keys the request mentions and checks that the screen changes. For apps that must save data, it adds an entry, closes and reopens the app, and checks the entry is still there. Failures are returned to the model with the error, within the existing repair-round limit.
2. **Step extension.** At the step limit, if files changed in the last 10 steps or Qore Code's own checks still fail, the turn gets 20 more steps, at most twice per turn. Step-based, not time-based.
3. **Repeat redirect.** On the first repeated identical tool call, the model gets the failing output and one chance to change approach. A second repeat ends the turn as before.
4. **Image request check.** If the request asks for a generated image: the image tool must be called with `save_to`, the program must load that file, and the file must not be overwritten by other code.

## Re-run, 2026-10-20 (Qore Code only)

Same GPU, prompts, settings, time limit and grader. The 30B slot used **Qwen3.8-27B (dense, Q8)** instead of Qwen3-Coder 30B-A3B, so those rows are not a before/after comparison. The 80B rows are.

All results were checked manually. Two automatic grades were wrong and are corrected below:
- Tip calculator (27B): graded fail by the vision model ("tip and total $0.00"). The screenshot shows $100 bill, $10.00 tip, $110.00 total. Counted as pass.
- Image full-loop (80B): graded pass. The image job was queued but never finished; the model drew a background in code and saved it under the same file name. Counted as fail. Both the grader and Qore Code's image request check now detect this.

| Task | Qwen3.8-27B | Qwen3-Coder-Next 80B: before → after |
|---|---|---|
| Tip calculator | PASS (manual) | PASS → PASS |
| To-do list that saves | PASS | PASS → PASS |
| Notes with search + save | PASS | PASS → PASS |
| 2D jump game | PASS | PASS → PASS |
| 3D walk + pick up + counter | fail | fail → fail |
| Debug a broken program | PASS | PASS → PASS |
| 3D house model file | — | PASS → PASS |
| Expense tracker + chart | — | fail → fail |
| SSH script | — | PASS → PASS |
| Image full-loop | — | fail → fail (manual) |
| **Total** | **5 / 6** | **7 / 10 → 7 / 10** |

No change on 80B. The app check worked as intended on the notes app (found an exception, model fixed it, check passed). Remaining failures:

| Task (model) | Cause |
|---|---|
| 3D game (80B) | Scene file had a parse error. Mid-turn trimming had replaced the earlier file content with a placeholder; the model copied the placeholder into its write and was refused 8 times, then the loop guard refused its real rewrites 7 times. |
| 3D game (27B) | Scene loads, player does not move. 10-minute limit reached (~46 tokens/s). After a mid-turn trim the model treated an internal note as a re-sent request. |
| Expense tracker (80B) | App check found the expense not saved after reopening. During repair the model read the 434-line file 48 times, because each page was trimmed from context 3 steps after it was read. |
| Image full-loop (80B) | Code-drawn image saved over the queued image's path (see above). |

Fixed after the re-run: a repair round reset the step-extension counter, allowing 100 steps instead of 80.

## Next (round 2)

1. **Context trimming by hardware.** Size the trimming budget from the actual context window, GPU/CPU and memory of the machine; keep recent file contents and the latest version of each written file; measure prompt size with real token counts from the server rather than an estimate.
2. **Loop guard.** Do not count Qore Code's own refusals (placeholder detected, unknown tool) as model failures.
3. **Prompt structure.** Attach project state to the original request, not to internal notes.
4. **Engine file check.** Validate Godot scene/script files right after they are written.
5. **Benchmark v2.** Multiple runs per task, deterministic checks instead of a vision model where possible, more task types (bug fixes in existing code, refactoring, tests), and all prompts, outputs, logs and grader code published.

The next run's results will be added here.
