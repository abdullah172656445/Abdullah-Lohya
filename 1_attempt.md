# 1 — Attempt: diagnosis and planned decisions

## Dataset overview
The pasted CSV contains 97 data rows and six columns: `date`, `item`, `city`, `price`, `unit`, and `data_label`. The expected grain appears to be one observation per date–item–city combination. There are 12 weekly dates, four items and two cities, so a complete rectangular panel would contain 12 × 4 × 2 = 96 unique combinations. The extra row therefore needs a duplicate check.

## Problems observed before cleaning
1. **Exact duplicate:** the final record repeats the 2026-01-08 / Staple A / City 1 observation (99.5).
2. **Missing price:** 2026-01-01 / Pulse C / City 2 has a blank price.
3. **Numeric formatting:** 2026-01-15 / Staple A / City 2 has the text value `1,250`, which may be a legitimate number formatted with a thousands separator. It must be parsed carefully, not treated as 1.25 or silently discarded.
4. **Extreme price:** 2026-01-22 / Sweetener D / City 1 has price 9999, far outside the surrounding values for this item. Flag for investigation; do not guess a replacement.
5. **Inconsistent category case and suspicious pairing:** 2026-02-05 has item `oil b` in lowercase and price 108.5 for City 1. Other Oil B values are around 298–344, while Staple A values are around 99.5–122. This could be an item-label error or a price error. Standardize the case for grouping, but preserve the original row and flag the unusual value/pairing for source verification.

## Planned decisions
| Issue | Planned decision | Reason |
|---|---|---|
| Duplicate row | Exclude one exact duplicate from analytical table; retain raw copy | Duplicate would double-count the same observation |
| Blank price | Retain row and missing value; flag | No evidence supports imputing a price |
| `1,250` | Remove thousands separator and parse as 1250; flag as implausible for follow-up | Parsing is a formatting correction, not proof that the value is correct |
| `9999` | Retain but flag; exclude from ordinary summaries unless verified | Do not invent a corrected value |
| `oil b` casing | Normalize to `Oil B` in a cleaned category column, preserve original | Case inconsistency is safe to standardize |
| Oil B / City 1 = 108.5 | Flag for investigation; do not auto-correct | It resembles the Staple A price range, but that is not sufficient evidence to change the item or price |
| Date/unit/data label | Parse date; inspect consistency; retain unit and provenance label | No evidence of inconsistent values |

## Initial commitment
This file is a teaching dataset, not official PBS data. I will preserve the raw input, make only transparent formatting changes, and keep uncertain observations visible with flags rather than silently deleting or imputing them.

**Attempt-before-AI requirement:** This document is a structured draft based on the dataset supplied in the conversation. Replace or annotate it with the timestamped notes/hash from your own independent first attempt if your instructor requires proof of that step.
