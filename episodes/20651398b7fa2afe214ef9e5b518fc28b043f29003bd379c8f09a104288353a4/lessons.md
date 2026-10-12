# Lessons: minbpe

Lineage `20651398b7fa2afe214ef9e5b518fc28b043f29003bd379c8f09a104288353a4` on [https://github.com/karpathy/minbpe](https://github.com/karpathy/minbpe) at `1acefe89412b20245db5a22d2a02001e547dc602`. 69 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 42 (1 abandoned, 41 no candidate)
- Median model spend per attempt: 0.3829 USD (over 23 attempts with a recorded cost)

## Target perf:train_ir

- Attempts: 18 (8 accepted, 9 rejected, 1 no candidate)
- Best accepted effect: 44.48% (ratio 0.5552), episode `2add717b5f41`
- Accepted effects: 44.48%, 29.88%, 23.17%, 14.13%, 7.21%, 4.83%, 1.78%, 1.74%
- Why candidates failed: stale_conflict (9)
- Files changed by accepted candidates: `minbpe/__init__.py`, `minbpe/base.py`, `minbpe/basic.py`, `minbpe/regex.py`
- Files changed by rejected candidates: `minbpe/base.py`, `minbpe/basic.py`, `minbpe/regex.py`
- Median model spend per attempt: 0.2888 USD (over 18 attempts with a recorded cost)

## Target perf:encode_ir

- Attempts: 9 (5 accepted, 4 rejected)
- Best accepted effect: 21.84% (ratio 0.7816), episode `9c2514fd843e`
- Accepted effects: 21.84%, 20.38%, 9.31%, 8.34%, 2.85%
- Why candidates failed: stale_conflict (1), stale (1), no_improvement (1), dependency_failed (1)
- Files changed by accepted candidates: `minbpe/base.py`, `minbpe/basic.py`, `minbpe/gpt4.py`, `minbpe/regex.py`
- Files changed by rejected candidates: `minbpe/base.py`, `minbpe/basic.py`, `minbpe/regex.py`
- Median model spend per attempt: 0.2176 USD (over 9 attempts with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> I worked on regex_train's setup loop in minbpe/_train.py to cut train_ir. I first tried two versions that build the pair counts and the pair-to-chunk index with C-level iterator chains. After that I tried a simpler version that builds only the pair-to-chunk index in the loop and then sums chunk weights per pair to get the counts.
> All three gave identical outputs and stayed within guard limits, and all three were rejected. The first iterator-chain version measured train_ir 331487975 against parent 313122779, ratio 1.0587. The second measured 321276796, ratio 1.0260. The index-then-sum version measured 311023177, ratio 0.9933, against a required 0.9900. encode_ir stayed at ratio 1.0000. I stopped without submitting.
> I now believe the setup loop has under 1% of easy headroom left, and that iterator chaining costs instructions in this code rather than saving them.
> Next time I should look past the setup loop, at the merge loop's per-pair update, where more of train_ir probably sits.
>
> 2026-10-12, episode `75c88c193fd7` (no candidate)

> I worked on the minbpe lineage. I read minbpe/basic.py, minbpe/regex.py, minbpe/base.py and lineage_bench.py. Then I edited basic.py in two places: lines 211-214 shrank from 4 lines to 2, and lines 234-236 grew from 3 lines to 23. I also made three small edits to regex.py, at lines 18, 29-30 and 311-313.
> I ran no sandbox evaluations, so I have no measured results for any of these edits. The session stopped without a candidate being submitted. Nothing here is tested.
> The benchmark file is short, and basic.py and regex.py are where the tokenizer work happens. Beyond that, the reading alone does not tell me whether my edits help, hurt or even run. Unmeasured edits are worth nothing, and I should not trust them.
> Next session I should run lineage_bench.py on the unmodified code first to get a baseline. Then I should reapply the edits one file at a time, measuring after each. I will submit only a change that I have actually evaluated in the sandbox.
>
> 2026-10-11, episode `046636bb4d81` (no candidate)

> I read lineage_bench.py, minbpe/regex.py, minbpe/basic.py, minbpe/base.py, minbpe/__init__.py and minbpe/gpt4.py to understand how the benchmark exercises the tokenizers. Then I made two edits to minbpe/basic.py. I replaced lines 237-238 (2 lines) with 22 lines, and I cut lines 211-214 (4 lines) down to 2.
> I ran no sandbox evaluations, so I have no measured result for either edit. The session stopped without submitting a candidate. Nothing I changed has been tested.
> All I can say is that basic.py is the part of the code the benchmark touches that I chose to work on. I cannot say whether the edits help, hurt, or even run correctly. Reading base.py and regex.py in full took time that I never turned into a measurement.
> Next time I should re-read the end of basic.py around my edited lines and confirm the edits are still there and coherent. I should run lineage_bench.py in the sandbox against the unedited baseline before changing anything else, and submit only after a measured comparison.
>
> 2026-10-11, episode `f2142e5b6cf8` (no candidate)

> I worked on encode performance in minbpe after reading lineage_bench.py, basic.py, regex.py and base.py. I edited the merge loop area in basic.py and several spots in regex.py, including the pattern line at 18, the encode path around 311, and a 40-line insertion near line 35. Some edits were reverted and redone.
> I ran one sandbox evaluation on encode_ir. Build and equivalence were fine, with identical outputs, but it was rejected for no improvement. encode_ir went from 420012634 to 424860522, ratio 1.0115, against a required 0.9900. train_ir also got slightly worse, 366041664 to 366749050, ratio 1.0019. I stopped without submitting a candidate.
> My changes added overhead instead of removing it. The encode hot path is not where my edits were helping, and extra setup code near the top of regex.py likely cost more than it saved.
> Next time I should profile encode first, find the dominant cost in the per-chunk merge loop, and make one small targeted change at a time before stacking edits.
>
> 2026-10-11, episode `9d4aacc06ecb` (no candidate)

> I read lineage_bench.py, then minbpe/regex.py, base.py and basic.py, and searched for the table helpers (_replace_table, _chunk_table, the "for ps, c" loops), which matched 11 lines. I then made edits. In basic.py I replaced 4 lines with 2 at 211-214 and 2 with 3 at 217-218, and expanded 235-237 from 3 lines to 22. In regex.py I touched lines 18, 29-30 and 311-313.
> I ran no sandbox evaluations, so I have no measured results. The session ended without submitting a candidate, and none of these edits has been tested.
> What I believe now is only what reading the code showed. The merge and replace logic in basic.py and regex.py goes through shared table helpers, so a change in one file probably has to be mirrored in the other. I have no evidence yet that my edits are correct or faster.
> Next time I should first check that the edited files still import and pass lineage_bench.py. After that I should benchmark against the unedited code before extending anything, and submit only if the measurement supports it.
>
> 2026-10-11, episode `6d920c2ced42` (no candidate)

## Licence

Code excerpts from https://github.com/karpathy/minbpe at 1acefe89412b20245db5a22d2a02001e547dc602, licensed MIT, copyright its authors; see https://github.com/karpathy/minbpe/blob/master/LICENSE.
