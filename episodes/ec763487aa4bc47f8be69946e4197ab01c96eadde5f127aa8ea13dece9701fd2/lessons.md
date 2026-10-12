# Lessons: minbpe-heavy

Lineage `ec763487aa4bc47f8be69946e4197ab01c96eadde5f127aa8ea13dece9701fd2` on [https://github.com/karpathy/minbpe](https://github.com/karpathy/minbpe) at `1acefe89412b20245db5a22d2a02001e547dc602`. 1 published attempt.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target perf:train_regex_ir

- Attempts: 1 (1 rejected)
- Why candidates failed: stale (1)
- Files changed by rejected candidates: `minbpe/regex.py`
- Median model spend per attempt: 0.1069 USD (over 1 attempt with a recorded cost)

## In the agent's own words

The agent's journal entries on this lineage, newest first, verbatim. They are its own notes: claimed, not checked.

> I read minbpe/regex.py and the top of base.py, then rewrote lines 44-60 of regex.py so RegexTokenizer.train deduplicates identical regex chunks in first-occurrence order and weights each unique chunk by its count. Pairs enter the stats dict in the same order, so max() ties break the same way. The merge step also skips chunks that cannot contain the pair.
> My sandbox run on train_regex_ir was accepted with identical outputs. train_regex_ir went from 6815654494 to 2486948482, ratio 0.3649. The other metrics barely moved: train_basic_ir 1.0008, encode_ir 1.0002, decode_ir 1.0000, save_load_ir 0.9999. Guard was ok with 1 file and 19 lines. I submitted it and it is queued for replay. No verdict yet.
> Most of the regex training cost came from recounting duplicate chunks. Keeping insertion order is what keeps the merges identical.
> Next I would look at encode_ir, maybe caching per-chunk encodings, and check whether basic training can use a similar skip in merge.
>
> 2026-10-12, episode `5fe580618602` (rejected)

## Licence

Code excerpts from https://github.com/karpathy/minbpe at 1acefe89412b20245db5a22d2a02001e547dc602, licensed MIT, copyright its authors; see https://github.com/karpathy/minbpe/blob/master/LICENSE.
