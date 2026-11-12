# 3 — Cleaning log, AI-code verdicts and reflection

## Cleaning log
| Operation | Result | Evidence / justification |
|---|---|---|
| Preserve raw input | Kept untouched `raw` and `price_raw` | Allows every change to be audited |
| Parse date | Converted to datetime with coercion | Enables chronological checks; parse failures are counted |
| Normalize item case/spacing | Added `item_clean`; original `item` retained | `oil b` becomes `Oil B` without overwriting source |
| Parse price | Added numeric `price_num`; comma removed before conversion | `1,250` becomes 1250; original string retained |
| Exact duplicate | Marked duplicate and excluded one repeated row from analysis | 97 raw rows → 96 analytical rows |
| Missing price | Retained as missing and flagged | No defensible value can be inferred |
| 9999 | Retained and flagged | Extreme relative to other Sweetener D observations; no replacement evidence |
| Oil B City 1 at 108.5 | Retained and flagged | Looks inconsistent with Oil B's other values and resembles Staple A prices; cannot safely infer intended correction |

## AI-suggested cleaning code: line-by-line verdicts
Use these verdicts to evaluate the actual AI code you were given in class. The notebook above is a conservative reference implementation; if your AI-generated code differs, compare each corresponding line.

| Suggested action / code pattern | Verdict | Evidence and reasoning |
|---|---|---|
| `dropna()` on the whole dataframe | **Reject** | Would discard the Pulse C / City 2 row even though only its price is missing; the row still carries useful identifying information |
| `drop_duplicates()` | **Modify** | Exact duplicate exists, but mark it first and preserve the full raw/audit table; remove only one repeated copy from analytical data |
| `pd.to_numeric(price)` without removing commas | **Modify** | `1,250` may parse as missing; remove the thousands separator transparently and preserve original text |
| Replace missing price with mean/median | **Reject** | No source evidence supports imputation; it would create an invented observation |
| Replace 9999 with a guessed typical price | **Reject** | Outlier detection is not proof of the intended value |
| Convert item names to title case | **Keep with modification** | Safe as a separate normalized column; retain original item text |
| Delete every outlier using an IQR rule | **Reject** | Could remove valid observations and hide data-quality issues; flag for review first |
| Delete the low Oil B observation | **Reject** | After case normalization it collides with the other Oil B / City 1 record on the same date; it may instead be a mislabelled Staple A row. The evidence cannot identify which field is wrong |
| Overwrite original columns during cleaning | **Reject** | Destroys auditability; create new cleaned columns instead |

## Before-and-after evidence
- Raw records: **97**.
- Exact duplicate records: **1**; analytical records after removing the repeated copy: **96**.
- Missing prices: **1**, retained and flagged.
- Comma-formatted price: **1**, parsed as **1250** while preserving its raw text.
- Extreme value 9999: **1**, retained and flagged.
- Suspicious Oil B / City 1 value 108.5: **1**, retained and flagged.
- Distinct weekly dates: **12**; four item labels after case normalization; two cities.
- Important key collision: after normalizing `oil b` to `Oil B`, the 2026-02-05 / Oil B / City 1 key appears twice, with prices 108.5 and 318.0. The suspicious 108.5 row therefore creates a conflict and leaves the intended Staple A / City 1 observation for that date absent. Do not drop either record automatically; the notebook reports this collision for investigation.
- A missing price is still a present date-item-city record. The data therefore has 96 records after exact duplicate removal, but it does not represent 96 clean, verified unique observations.

## Reflection — personalize before submission
The strongest difference between a fast automated cleaning approach and my decision was to avoid automatically deleting or replacing unusual values. The missing price, 9999, and 108.5 observation are different problems: the first is absent, the second is an extreme numeric value, and the third may be a mismatch between item and price. Treating all three as generic outliers would lose information.

The comma in `1,250` is a formatting issue, so parsing it as 1250 is reasonable, but the resulting amount is still suspicious and should be checked against the source. I normalized `oil b` for consistent grouping but preserved the original label. I would not change 108.5 to another value or relabel it as Staple A without source documentation.

**Complete this paragraph in your own words:** My own initial decision that differed from the AI suggestion was __________. I chose it because __________. The evidence that best supported my decision was __________. Without checking the data manually, I might have missed __________.

## Limitations
The dataset labels itself as illustrative teaching data, not actual PBS observations. The notebook therefore makes no claim about real market prices. The intended corrections for 9999 and 108.5 cannot be established from this file alone.
