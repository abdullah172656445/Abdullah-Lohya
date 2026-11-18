# 3 — Referee's note, AI audit and reflection

## Referee's note (O / E / C labels)

**O:** The uploaded teaching dataset contains 97 rows, four item categories after case normalization, two cities and twelve weekly dates.  
**O:** One exact duplicate is removed from the analytical table, leaving 96 rows.  
**O:** The item means are Oil B: PKR 316.18, Pulse C: PKR 205.91, Staple A: PKR 160.89, Sweetener D: PKR 576.81.  
**O:** The item medians are Oil B: PKR 324.00, Pulse C: PKR 206.00, Staple A: PKR 112.50, Sweetener D: PKR 167.75.  
**O:** The first-to-last changes, based on each item's first-date and last-date averages across available cities, are Oil B: 15.28%, Pulse C: 5.25%, Staple A: 12.98%, Sweetener D: 19.41%.  
**O:** Sweetener D's mean with the flagged 9999 value is PKR 576.81, compared with PKR 167.15 when that value is excluded for a sensitivity check.  
**O:** Sweetener D's median is PKR 167.75 with 9999 and PKR 167.50 without it.  
**E:** The difference between Sweetener D's mean and median is consistent with an extreme value pulling the mean upward.  
**E:** The parsed 1250 value may be a source or formatting problem, but the dataset does not establish its intended value.  
**E:** The 108.5 value labelled `oil b` may be a category mismatch because it is far below other Oil B prices and creates a normalized key collision.  
**C:** These data prove that a particular real-world event caused prices to rise.  
**O:** The source is explicitly labelled illustrative teaching data, not actual PBS observations, and contains no variables that identify a cause.

## Statistics table

|Item|Numeric n|Mean (PKR/kg)|Median (PKR/kg)|First-date mean|Last-date mean|Change|
|-|-:|-:|-:|-:|-:|-:|
|Oil B|25|316.18|324.00|301.00|347.00|15.28%|
|Pulse C|23|205.91|206.00|200.00|210.50|5.25%|
|Staple A|23|160.89|112.50|104.00|117.50|12.98%|
|Sweetener D|24|576.81|167.75|152.00|181.50|19.41%|



## Flagged-value comparison: Sweetener D

|Calculation|n|Mean (PKR/kg)|Median (PKR/kg)|
|-|-:|-:|-:|
|Including flagged 9999|24|576.81|167.75|
|Excluding 9999 for sensitivity check|23|167.15|167.50|

The 9999 value remains in the main dataset because no evidence supports a replacement. The exclusion is shown only to illustrate its influence on the averages.

## AI classmate transcript excerpt

**Classmate:** The mean is always the typical price, right?  
**Response:** Not always. The mean is the arithmetic average and can be pulled away from most observations by an extreme value.

**Classmate:** Why not just use the mean for Sweetener D?  
**Response:** With 9999 included, the mean is PKR 576.81, while the median is PKR 167.75. Both are mathematically valid summaries, but the mean is heavily influenced by the flagged extreme value.

**Classmate:** So the median is always the right answer?  
**Response:** No. The median is resistant to extremes, but it does not reflect the size of every observation. The useful summary depends on the distribution and question.

**Classmate:** Does the rise prove why prices went up?  
**Response:** No. These descriptive statistics show a change in the teaching data, not its cause.

## Claim audit

|Claim|Colour|How checked|Decision|
|-|-|-|-|
|Item means and medians in the table|Green|Recomputed from the uploaded CSV after Assignment 2 cleaning|Keep|
|First-to-last changes|Green|Recomputed first-date and last-date city averages|Keep|
|9999 affects Sweetener D mean strongly|Green|Recomputed with and without exactly 9999|Keep as sensitivity comparison|
|1250 may be a source/formatting issue|Amber|Confirmed raw text and parsed number; intended value unknown|Keep only as possible explanation|
|108.5 may be a category mismatch|Amber|Checked item normalization, comparison and key collision|Keep only as possible explanation|
|Data prove a real-world cause|Red|Dataset is illustrative and has no causal variables|Reject|

**Colour key:** Green = recomputable observation; amber = possible explanation, not verified; red = causal claim unsupported by this dataset.

## Reflection — personalize before submission

The main limit of a first draft is that it can describe a numerical change without explaining why it happened. The data support statements about averages, medians and first-to-last differences, but they do not contain evidence about supply, demand, policy, transport or other causes. Flagged values also remain unresolved: excluding them can show how sensitive a statistic is, but it does not establish that the value is wrong. Comparing mean and median makes the effect of the 9999 value easier to see.

**Complete honestly in your own words:** My first draft said more than the data allowed when I wrote That the increase in prices was caused by a particular factor. I changed it because The data only shows changes in prices and averages, not what caused those changes.

