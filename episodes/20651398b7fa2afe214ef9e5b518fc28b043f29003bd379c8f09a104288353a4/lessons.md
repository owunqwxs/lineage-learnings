# Lessons: minbpe

Lineage `20651398b7fa2afe214ef9e5b518fc28b043f29003bd379c8f09a104288353a4` on [https://github.com/karpathy/minbpe](https://github.com/karpathy/minbpe) at `1acefe89412b20245db5a22d2a02001e547dc602`. 50 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 25 (1 abandoned, 24 no candidate)
- Median model spend per attempt: 0.4032 USD (over 6 attempts with a recorded cost)

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

> I tried speeding up encode in minbpe/regex.py by replacing the regex split for special tokens with a str.find loop. That meant one edit of 7 lines grown to 39, later reverted.
> My sandbox run gave identical outputs. encode_ir went from 421486490 to 420108179, ratio 0.9967, and train_ir rose from 378139621 to 379890339, ratio 1.0046. It was rejected for no improvement, since the worst ratio was 0.9967 against a required 0.9900. I reverted the edit, so the source matches the parent, and I submitted no candidate.
> What I now believe is that the special-token split is not where encode spends its time. Rewriting that path buys well under the 1% bar and can cost training a little. The real cost is likely in the merge loop itself.
> Next I want to try the untested idea in the str.replace loop over the merge table. I would keep a running set of characters present and skip any replacement whose two characters are not in it. Then I would measure encode_ir and train_ir again.
>
> 2026-10-11, episode `4bef3eb6e15f` (no candidate)

## Licence

Code excerpts from https://github.com/karpathy/minbpe at 1acefe89412b20245db5a22d2a02001e547dc602, licensed MIT, copyright its authors; see https://github.com/karpathy/minbpe/blob/master/LICENSE.
