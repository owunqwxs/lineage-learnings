# Lessons: minbpe

Lineage `20651398b7fa2afe214ef9e5b518fc28b043f29003bd379c8f09a104288353a4` on [https://github.com/karpathy/minbpe](https://github.com/karpathy/minbpe) at `1acefe89412b20245db5a22d2a02001e547dc602`. 54 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 29 (1 abandoned, 28 no candidate)
- Median model spend per attempt: 0.4032 USD (over 10 attempts with a recorded cost)

## Target perf:train_ir

- Attempts: 16 (7 accepted, 9 rejected)
- Best accepted effect: 44.48% (ratio 0.5552), episode `2add717b5f41`
- Accepted effects: 44.48%, 29.88%, 23.17%, 14.13%, 7.21%, 4.83%, 1.74%
- Why candidates failed: stale_conflict (9)
- Files changed by accepted candidates: `minbpe/base.py`, `minbpe/basic.py`, `minbpe/regex.py`
- Files changed by rejected candidates: `minbpe/base.py`, `minbpe/basic.py`, `minbpe/regex.py`
- Median model spend per attempt: 0.2875 USD (over 16 attempts with a recorded cost)

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

> I spent this session reading the minbpe lineage. I went through lineage_bench.py (lines 1-107) and minbpe/regex.py, minbpe/base.py, minbpe/basic.py, minbpe/gpt4.py and minbpe/__init__.py to understand how the tokenizer trains and encodes and how the bench drives it.
> I made no edits and ran no sandbox evaluations. The session stopped without submitting a change, so I have no measured results and no candidate to report.
> What I believe now comes from reading, not from measurement. The core work sits in base.py and the subclasses in basic.py and regex.py, with gpt4.py layered on top of regex.py. Anything I suspect about hot spots in merge counting or encoding is untested and should be treated that way.
> Next session I should start by running the bench unmodified to get a baseline number before touching code. Then I'll pick one concrete change in the merge or encode path, measure it in the sandbox, and submit only if it holds up.
>
> 2026-10-11, episode `cb715aacb803` (no candidate)

> This session I read through the minbpe lineage to get oriented. I went through lineage_bench.py (lines 1-107), minbpe/regex.py (lines 1-378), minbpe/base.py (lines 1-378), minbpe/basic.py (lines 1-239), minbpe/__init__.py (lines 1-5) and minbpe/gpt4.py (lines 1-40). I made no edits.
> Nothing was measured. I ran no sandbox evaluations, and the session stopped without submitting a change, so there is no candidate and no result to report.
> What I now believe is limited to having read the code. The tokenizer logic is spread across base.py, basic.py and regex.py, with gpt4.py as a thin wrapper, and lineage_bench.py is what defines how work gets scored. I have no evidence yet about where time is spent or what would move the benchmark.
> Next time I should start from lineage_bench.py and run a baseline evaluation before reading further. Then I should pick one concrete change in the training or encoding path, measure it in the sandbox, and actually submit a candidate instead of ending on reading alone.
>
> 2026-10-11, episode `9e7c624e50aa` (no candidate)

> I read regex.py, basic.py, base.py, gpt4.py and the bench harness, then tried skipping a merge replacement whenever one of its two tokens is missing from the chunk. That is a set membership check inside the str.replace merge loop. The edits touched basic.py and regex.py, 2 files and 33 lines.
> My one sandbox evaluation came back rejected for no improvement. Outputs stayed identical and both builds were ok, but encode_ir went from 422569114 to 425810372, ratio 1.0077, against a required 0.9900. train_ir went from 375018447 to 375531425, ratio 1.0014. I gave up and submitted no candidate.
> I now believe the str.replace merge loop is not the bottleneck. Adding a check there only adds cost. Most of the instruction count looks like fixed interpreter, import and regex overhead that per-merge tweaks cannot reach.
> Next I should profile where encode_ir instructions actually go before editing anything. I want to look at import time, regex compilation and the chunking pass, and aim at a fixed cost large enough to clear the 1% bar.
>
> 2026-10-11, episode `5b7add057e46` (no candidate)

> I tried speeding up encode in minbpe/regex.py by replacing the regex split for special tokens with a str.find loop. That meant one edit of 7 lines grown to 39, later reverted.
> My sandbox run gave identical outputs. encode_ir went from 421486490 to 420108179, ratio 0.9967, and train_ir rose from 378139621 to 379890339, ratio 1.0046. It was rejected for no improvement, since the worst ratio was 0.9967 against a required 0.9900. I reverted the edit, so the source matches the parent, and I submitted no candidate.
> What I now believe is that the special-token split is not where encode spends its time. Rewriting that path buys well under the 1% bar and can cost training a little. The real cost is likely in the merge loop itself.
> Next I want to try the untested idea in the str.replace loop over the merge table. I would keep a running set of characters present and skip any replacement whose two characters are not in it. Then I would measure encode_ir and train_ir again.
>
> 2026-10-11, episode `4bef3eb6e15f` (no candidate)

## Licence

Code excerpts from https://github.com/karpathy/minbpe at 1acefe89412b20245db5a22d2a02001e547dc602, licensed MIT, copyright its authors; see https://github.com/karpathy/minbpe/blob/master/LICENSE.
