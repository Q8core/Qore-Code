# Where Qore Code stands, and what comes next

Last updated 2026-10-20, after the [local-model benchmark](BENCHMARK.md).

## Where we are

On the same local Q8 models, prompts, time limit and grader, Qore Code passed **12 of 24** tasks. opencode and Qwen Code passed **7 of 24** each. That puts Qore Code ahead on this benchmark, but **half the tasks still fail**, and we are publishing why.

We read the run log of every task Qore Code failed. This is what they show:

| What went wrong | Tasks it hit | Whose problem |
|---|---|---|
| It stopped at its fixed limit of 40 steps while 4–7 of the 10 minutes were still left. The last check found a problem, but no steps were left to fix it | Notes app (both Qwen models), 3D game (30B), expense tracker (80B) | **Qore Code** |
| Its finish check runs the project's tests and code checks, but it **does not use the app like a person would**. An expense tracker that never opened (a missing library), a notes app whose notes were gone after reopening, and a game whose player did not jump were all reported as done | Expense tracker, notes app, 2D game (80B) | **Qore Code** |
| Asked to make its own background picture, the model drew one in code and never used Qore Code's picture tool. Nothing checked that the request was met | Image full-loop | **Qore Code** |
| With Llama 3.3 70B, most runs produced no working program, whatever the tool | Qore Code passed 2 of 9 Llama tasks, the other two tools 0 of 9 | Mostly the model |

Most remaining failures on the Qwen models are Qore Code's to fix, not the models'.

## What we are building next

1. **Run the app like a user before saying "done".**
   - Qore Code starts the program the way you would and checks it opens without crashing.
   - It presses the keys a game is supposed to react to, and checks the game reacts.
   - It types an entry, saves, closes, reopens, and checks the entry is still there.
   - Anything that fails goes back to the model as a bug report to fix, within a limited number of repair rounds.
2. **Don't stop while there's progress being made.** When the step limit arrives while the work is still changing, or while the last check is still failing, Qore Code grants a limited number of extra steps to finish and repair. This is a count, not a timer: Qore Code still never cuts a model off on time.
3. **Check that what was asked for was done.** If you ask Qore Code to make a picture itself, it checks that the picture tool was used and that the picture made it into the project.

## How we will measure it

We will run the same benchmark again: same models, prompts, settings, time limit and grader. We will publish the result here whether it goes up or down. Every result table will stay as measured.
