# Where Qore Code stands, and what comes next

Last updated 2026-10-20, after a re-run of the [local-model benchmark](BENCHMARK.md) with our first round of fixes.

## Where we are

On the same local Q8 models, prompts, time limit and grader, Qore Code passed **14 of 24** tasks. opencode passed **9 of 24** and Qwen Code **10 of 24**.

These numbers were corrected on 2026-10-20. The first version (12 / 7 / 7) was graded by a grader with mistakes. We found six by checking it by hand, fixed them, and graded every row of every tool again. Every change went from fail to pass, for all three tools. The full report lists each fix.

Qore Code is ahead on this benchmark, but **it still fails tasks it should pass**. We read the run log of every task the latest Qore Code build failed, and this is what they show:

| Task (model) | What went wrong | Whose problem |
|---|---|---|
| Notes app (30B) | Its New Note button crashed (a missing import). Qore Code's own check found it only after all 40 steps were used, so nothing was left to fix it with | **Qore Code** |
| Expense tracker (80B) | The app never opened, because a library it needs was not installed. Qore Code's finish check runs code checks, but it never started the program, and the 40 steps ran out | **Qore Code** |
| 3D game (30B) | All 40 steps were used with most of the 10 minutes left. The project file was written in the wrong format | **Qore Code** (steps), the model (format) |
| 3D game (80B) | The model wrote the same file again, unchanged. Qore Code's repeat detector stopped the run instead of pointing the model somewhere else | **Qore Code** |
| Image full-loop (80B) | Asked to make its own background picture, the model drew one in code and never used Qore Code's picture tool. Nothing checked that the request was met | **Qore Code** |

With Llama 3.3 70B, most runs produced no working program, whatever the tool (Qore Code 2 of 9, opencode and Qwen Code 0 of 9). That part is mostly the model.

## Re-run with the first round of fixes (2026-10-20)

We built the four fixes listed below into Qore Code and ran the Qore Code tasks again on the same GPU, with the same prompts, settings, 10-minute limit and grader. One change: in the 30B slot we used **Qwen3.8-27B (dense, Q8)** instead of Qwen3-Coder 30B-A3B, so those rows show a different model, not a before/after. The 80B rows are a straight before/after.

We checked every result by hand, and corrected the machine grader in two places:
- **Tip calculator (27B):** the grader's vision model said the tip and total stayed at $0.00. Its own screenshot shows a $100 bill, a $10.00 tip and a $110.00 total. We count it as a **pass**.
- **Image full-loop (80B):** the grader said **pass**. By hand it is a **fail**. The model did call the picture tool, but did not wait for the picture. It drew a background in code and saved it under the same file name, so the game shows the drawing. The grader now requires a finished picture that the code did not draw. Qore Code's own check now catches this too.

| Task | Qwen3.8-27B (new) | Qwen3-Coder-Next 80B: before → after |
|---|---|---|
| Simple app: tip calculator | **PASS** (by hand) | PASS → **PASS** |
| Mid app: to-do list that saves | **PASS** | PASS → **PASS** |
| Semi-complex app: notes with search + save | **PASS** | PASS → **PASS** |
| 2D game: jump over obstacles | **PASS** | PASS → **PASS** |
| 3D game: walk + pick up + counter | fail | fail → fail |
| Debug: fix a broken program | **PASS** | PASS → **PASS** |
| 3D modeling: a house file | — | PASS → **PASS** |
| Serious app: expense tracker + chart | — | fail → fail |
| SSH: script that logs in and lists files | — | PASS → **PASS** |
| Image full-loop | — | fail → fail (by hand) |
| **Total** | **5 of 6** | **7 of 10 → 7 of 10** |

**The 80B result did not move.** The new checks did their job in places: Qore Code started the notes app, found it failing, sent it back, and the model fixed it. But the three hard tasks still fail, and the run logs say why:

| Task (model) | What went wrong | Whose problem |
|---|---|---|
| 3D game (80B) | The scene file had a format error. The model rewrote it the same way again and again; the new redirect fired, but did not change its approach. The conversation also grew past the model's 32k context window | **Qore Code** |
| 3D game (27B) | The scene shows, but the player does not move. The 10 minutes ran out (this dense model writes about 46 tokens a second). Qore Code also shrank the conversation every few steps while saving little, and after one of these the model believed the request had been sent again | **Qore Code**, partly the model's speed |
| Expense tracker (80B) | Qore Code started the app, added an expense, closed and reopened it, and found the expense gone. It sent that back, but the repair spent its steps reading the file in pieces while the conversation overflowed the context window. A bug also let one turn use 100 steps instead of the 80 maximum (fixed) | **Qore Code** |
| Image full-loop (80B) | Drew the picture in code over the queued picture's file (see above) | **Qore Code** (now checked) |

In 5 of the 16 runs, the conversation grew past the model's 32k context window. That is now the biggest single problem we see.

## What we built in the first round

1. **Use the program like a person before saying "done".**
   - Qore Code starts the program the way you would and checks that it opens without crashing.
   - It presses the keys a game should react to, and checks that it reacts.
   - It types an entry, saves, closes the window, reopens it, and checks the entry is still there.
   - Anything that fails goes back to the model as a bug report, with the error, within a limited number of repair rounds.
2. **Don't stop while work is still moving.** When the step limit arrives while files are still changing, or while its own checks still fail, Qore Code grants up to two sets of 20 extra steps per turn. This is a count, not a timer: Qore Code still never cuts a model off on time.
3. **When the model repeats itself, redirect it before stopping it.** Show it what is failing, and give it one chance to take a different approach.
4. **Check that what was asked for was done.** If you ask Qore Code to make a picture itself, it checks that the picture tool was used, that the program loads the picture, and (since this re-run) that nothing else wrote over the picture's file.

## What we are building next

1. **Stay inside the context window.** Shrink the conversation in a way that frees real room, without making the request look new. Read large files by searching for what is needed, not in 100-line pages.
2. **Break a repeat for real.** When a game engine file fails to load, check it with the engine right after it is written, and give the model the exact error line. If the same broken file comes back, steer the model to a different way of building it.
3. **Spend fewer steps on overhead,** so slower local models finish inside the time limit.

## How we will measure it

We will run the same benchmark again: same models, prompts, settings, time limit and grader. We will publish the result here whether it goes up or down, and every number will stay as measured.
