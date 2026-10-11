# Lessons: minbpe

Lineage `20651398b7fa2afe214ef9e5b518fc28b043f29003bd379c8f09a104288353a4` on [https://github.com/karpathy/minbpe](https://github.com/karpathy/minbpe) at `1acefe89412b20245db5a22d2a02001e547dc602`. 49 published attempts.

Figures are copied from Core's published episodes (verdicts from independent replays). Nothing here is estimated.

## Target (none chosen)

- Attempts: 24 (1 abandoned, 23 no candidate)
- Median model spend per attempt: 0.4088 USD (over 5 attempts with a recorded cost)

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

## Licence

Code excerpts from https://github.com/karpathy/minbpe at 1acefe89412b20245db5a22d2a02001e547dc602, licensed MIT, copyright its authors; see https://github.com/karpathy/minbpe/blob/master/LICENSE.
