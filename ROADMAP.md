# Where Qore Code stands, and what comes next

Last updated 2026-10-20, after the [local-model benchmark](BENCHMARK.md).

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

## What we are building next

1. **Use the program like a person before saying "done".**
   - Qore Code starts the program the way you would and checks that it opens without crashing.
   - It presses the keys a game should react to, and checks that it reacts.
   - It types an entry, saves, closes the window, reopens it, and checks the entry is still there.
   - Anything that fails goes back to the model as a bug report, with the error, within a limited number of repair rounds.
2. **Don't stop while work is still moving.** When the step limit arrives while files are still changing, or while its own checks still fail, Qore Code grants a limited number of extra steps to finish and repair. This is a count, not a timer: Qore Code still never cuts a model off on time.
3. **When the model repeats itself, redirect it before stopping it.** Show it what is failing, and give it one chance to take a different approach.
4. **Check that what was asked for was done.** If you ask Qore Code to make a picture itself, it checks that the picture tool was used and that the program loads the picture.

## How we will measure it

We will run the same benchmark again: same models, prompts, settings, time limit and grader. We will publish the result here whether it goes up or down, and every number will stay as measured.
