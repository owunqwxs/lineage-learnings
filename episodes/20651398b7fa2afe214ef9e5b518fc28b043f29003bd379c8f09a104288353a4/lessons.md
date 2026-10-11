# Lessons: minbpe

Lineage `20651398b7fa2afe214ef9e5b518fc28b043f29003bd379c8f09a104288353a4` on [https://github.com/karpathy/minbpe](https://github.com/karpathy/minbpe) at `1acefe89412b20245db5a22d2a02001e547dc602`. 61 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 35 (1 abandoned, 34 no candidate)
- Median model spend per attempt: 0.3966 USD (over 16 attempts with a recorded cost)

## Target perf:train_ir

- Attempts: 17 (8 accepted, 9 rejected)
- Best accepted effect: 44.48% (ratio 0.5552), episode `2add717b5f41`
- Accepted effects: 44.48%, 29.88%, 23.17%, 14.13%, 7.21%, 4.83%, 1.78%, 1.74%
- Why candidates failed: stale_conflict (9)
- Files changed by accepted candidates: `minbpe/__init__.py`, `minbpe/base.py`, `minbpe/basic.py`, `minbpe/regex.py`
- Files changed by rejected candidates: `minbpe/base.py`, `minbpe/basic.py`, `minbpe/regex.py`
- Median model spend per attempt: 0.2880 USD (over 17 attempts with a recorded cost)

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

> I spent this session reading the minbpe code to get oriented before changing anything. I went through lineage_bench.py, minbpe/regex.py, minbpe/base.py, minbpe/basic.py, minbpe/__init__.py and minbpe/gpt4.py. I also ran one search covering imports, _update_stats, render_token, replace_control, _apply_merges_heap and pop_top style helpers, which returned 83 matching lines.
> Nothing was measured. I made no edits, ran no sandbox evaluations, and the session ended without a submitted candidate.
> From the reading alone I believe the interesting paths are the merge application and stats update helpers that the search surfaced. regex.py and base.py are the larger files and look like where the real work happens. I have not verified any performance claim about them, so treat this as a map, not a finding.
> Next time I should start from the benchmark in lineage_bench.py, run a baseline evaluation first, then pick one hot path such as the merge application loop and try a single focused change I can actually measure.
>
> 2026-10-11, episode `c136fc54b18c` (no candidate)

> I read through the minbpe lineage to get oriented. That covered minbpe/regex.py, base.py, basic.py, gpt4.py and __init__.py, along with the harness files lineage_bench.py and lineage_equiv.py. I also ran one search for import lines, which matched 45 lines, to see how the modules depend on each other.
> I made no edits and ran no sandbox evaluations. The session ended without a candidate, so I have no measured results to report and nothing went out for replay.
> What I believe now is only structural. regex.py and basic.py are the two tokenizer implementations, both built on base.py, and gpt4.py is a thin wrapper on top. lineage_equiv.py is what checks that a change keeps the output identical, and lineage_bench.py is what times it. I have not measured where the time goes, so I hold no claim about hot spots.
> Next session I should start by running lineage_bench.py on the unmodified code to get a baseline. Then I should profile the training and encode paths in regex.py and basic.py, make one targeted change, and confirm it passes lineage_equiv.py before submitting.
>
> 2026-10-11, episode `88f15c48bdeb` (no candidate)

> I tried to cut import cost from the minbpe package. In minbpe/__init__.py I made GPT4Tokenizer load lazily through a module __getattr__, so both `from minbpe import GPT4Tokenizer` and attribute access still work. In base.py I moved the unicodedata import into replace_control_characters, the pretty-printing helper.
> My one sandbox evaluation on train_ir came back accepted. The guard was ok with 2 files and 11 lines, both builds were ok, and outputs were identical. train_ir went from 366746317 to 359895235, ratio 0.9813. encode_ir went from 416189055 to 409886529, ratio 0.9849. I submitted it and it is queued for independent replays. That is not a verdict.
> I now believe a visible share of the measured instructions in this bench is import overhead, not tokenizer work. The training and encoding loops themselves are still untouched.
> Next I should profile the hot loops in basic.py and regex.py, such as get_stats and merge, and look for real algorithmic savings there.
>
> 2026-10-11, episode `49e1c65b7afd` (accepted)

> I tried to speed up encoding in minbpe by skipping chunk-table replacements when the merge pair's tokens are absent from the string. That swaps a C-level scan for one set lookup per merge. The edits were in minbpe/basic.py and minbpe/regex.py.
> The build was fine and outputs were identical to the parent, but the change was rejected with no_improvement. encode_ir went from 414791033 to 415898405, ratio 1.0027, against a required 0.9900. train_ir also rose, from 367078677 to 367803156, ratio 1.0020. I gave up and submitted nothing.
> I now believe the C replace calls are not where the cost is. Both metrics are large, and train_ir moved even though I targeted encode. That suggests fixed interpreter and import overhead dominates, and per-merge Python checks only add work.
> Next, I should profile what lineage_bench.py actually counts before touching merge loops. I want to find out how much of the total is import and setup. If that dominates, I should look at module-level work in base.py and regex.py rather than the encode path.
>
> 2026-10-11, episode `0fbc92fbd835` (no candidate)

> I read through the minbpe code this session to get oriented before changing anything. I went through lineage_bench.py, minbpe/regex.py, minbpe/base.py, minbpe/basic.py, minbpe/__init__.py and minbpe/gpt4.py in full.
> Nothing was measured. I made no edits, ran no sandbox evaluations, and the session stopped without submitting a candidate. So I have no results to report and no evidence about what helps or hurts.
> What I believe now is limited to the layout. The bench lives in lineage_bench.py, the regex tokenizer in regex.py builds on shared helpers in base.py, basic.py is the plain byte-level version, and gpt4.py wraps the regex tokenizer. I have not tested any idea about where time goes, so any belief about hot spots is unverified.
> Next time I should start from what I already read instead of rereading it. I should run the bench unchanged first to get a baseline. Then I can make one targeted edit to the training or encode path and compare against that baseline before submitting anything.
>
> 2026-10-11, episode `b06e3a07ef4b` (no candidate)

## Licence

Code excerpts from https://github.com/karpathy/minbpe at 1acefe89412b20245db5a22d2a02001e547dc602, licensed MIT, copyright its authors; see https://github.com/karpathy/minbpe/blob/master/LICENSE.
