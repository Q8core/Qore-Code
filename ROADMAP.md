# Roadmap

Updated 2026-10-21. Latest version: 2026.10.21 (Windows and Linux; the Mac build is 2026.10.18).

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

## Changes after the re-run (round 2)

1. **Prompt size.** The size estimate counted the whole conversation twice (screenshots included), so it reported 1.7–3.2 times the real size and trimmed the conversation early. Ollama's own log showed no prompt was ever cut; the largest was 23,158 tokens. Fixed; the trimming rules themselves are unchanged.
2. **Trimming.** A new screenshot no longer removes the three most recent tool results; text sent with a screenshot is kept; a short result is never replaced by a longer placeholder; a call whose result is kept keeps its arguments.
3. **Loop guard.** Qore Code's own refusals (copied placeholder, unknown tool, loop guard) are no longer counted as failed edits.
4. **Prompt structure.** Project state attached to an internal note is labelled as not a new request.
5. **Godot check.** Each written scene, resource or script is loaded by Godot headless (about 1 s) and its errors are added to the tool result.
6. **Chat mode** (in 2026.10.20). A switch that sends messages straight to the model: no tools, no project access, no checks.

## Run 3, 2026-10-20 (Qore Code only)

Same tasks, prompts, settings, 10-minute limit and grader as the re-run. Each task ran twice:
- **Agent mode**: Qore Code as normal.
- **Chat mode**: the model alone. The code blocks from its answer were saved into the files it named and graded the same way. Chat mode has no image tool, so the image task is Agent mode only.

All results were checked manually. Three automatic grades were wrong and are corrected below:
- Image full-loop (80B, Agent): graded fail because the game also saves a screenshot to a file. The game's background is the picture made by the image tool. Counted as pass; the grader rule is fixed.
- 3D game (27B, Agent): graded fail by the vision model. The player walks and the view turns (49.6% of pixels changed). Counted as pass under the published rule (view moves or counter goes up). Its pickups do not work.
- 3D game (27B, Chat): graded pass by the vision model. The screen is black and does not change, also in Firefox. Counted as fail.

Two Chat mode rows were run again after fixes to Qore Code and to the benchmark: the 2D game (27B), whose answer had been cut off by Qore Code's repetition check, and the 3D house (80B), whose file type was not recognised. The second run is shown.

| Task | 27B Agent | 27B Chat | 80B Agent | 80B Chat |
|---|---|---|---|---|
| Tip calculator | PASS | PASS | PASS | PASS |
| To-do list that saves | PASS | PASS | PASS | PASS |
| Notes with search + save | PASS | PASS | PASS | PASS |
| 2D jump game | fail | PASS | PASS | PASS |
| 3D walk + pick up + counter | PASS (manual) | fail (manual) | fail | PASS |
| Debug a broken program | PASS | fail | PASS | fail |
| 3D house model file | — | — | PASS | fail |
| Expense tracker + chart | — | — | PASS | fail |
| SSH script | — | — | PASS | fail |
| Image full-loop | — | — | PASS (manual) | — |
| **Total** | **5 / 6** | **4 / 6** | **9 / 10** | **5 / 9** |

Agent mode: **14 / 16**. Chat mode: **9 / 15**. On 80B, Agent mode went from 7 / 10 to 9 / 10: the expense tracker and the image task passed for the first time.

Agent mode failures:

| Task (model) | Cause |
|---|---|
| 2D game (27B) | Pressing Space crashes the game (`self.player.jump` is an image, not a function). The 10-minute limit was reached before Qore Code's app check ran. |
| 3D game (80B) | `scenes/player.tscn` has a parse error. The Godot check reported it 6 times; the model changed other files and the time ran out. |

Chat mode failures:

| Task (model) | Cause |
|---|---|
| Debug a broken program (27B, 80B), SSH script (80B) | The model cannot see the project's files in Chat mode. |
| 3D game (27B) | Black screen. |
| 3D house (80B) | The model wrote a glTF file whose embedded data repeats one fragment until the answer is cut off. Same output on both runs. |
| Expense tracker (80B) | Imports matplotlib without telling the user to install it, so it does not start. |

Setup: RTX PRO 6000 Blackwell 96 GB (re-run rows on the Workstation edition of the same GPU), Ollama, temperature 0.1, seed 20261019, 32k context. Each cell is a single run.

## Picture test, 2026-10-21

Each model was asked for a picture in the real app, once with a short prompt ("make me a picture of a red apple on a wooden table") and once with a longer one (a bakery front-page photo, saved in the project as bakery.png). A run passes when the picture tool was called, the picture finished at the size asked, and, for the longer prompt, bakery.png is the tool's own picture. Each model was also asked which operating system the computer runs.

| Model | Before (2026.10.20) | After (2026.10.21) | OS question |
|---|---|---|---|
| Qwen3.8-27B | 2 / 2 | 2 / 2 | correct |
| Qwen3-Coder 30B-A3B | 2 / 2 | 2 / 2 | correct |
| Qwen3-Coder-Next 80B | 0 / 2 | 2 / 2 | correct |
| Llama 3.3 70B | 0 / 2 | 2 / 2 | correct (was an error) |
| **Total** | **4 / 8** | **8 / 8** | **4 / 4** |

Causes and fixes:
- 80B: the model waited for its picture inside the reply, while the picture waited for the reply to end. Pictures now pause only while the model is writing.
- Llama 3.3: the request check asked a plain picture request to be saved and loaded by a program, and wrong argument names (`cmd`, `filename`) failed without saying why. A plain picture request is now complete when the picture is made; common wrong names are accepted and a missing argument is named.
- OS question: short chat messages had no line saying which operating system the computer runs, so a model could say Linux on Windows. Light chat also sent a thinking value that Ollama 0.40 refuses, so every short chat with Llama 3.3 failed.

## Next

1. **Time limit.** Both Agent mode failures reached the 10-minute limit. Run the app check earlier in long turns, so its result reaches the model while there is time to act on it.
2. **Benchmark v2.** Multiple runs per task, deterministic checks instead of a vision model where possible, more task types (bug fixes in existing code, refactoring, tests), and all prompts, outputs, logs and grader code published.
