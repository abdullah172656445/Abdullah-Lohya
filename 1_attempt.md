# 1 — Attempt: method and planned decisions

## Dataset and cleaning carried forward

The actual uploaded source contains 97 rows. I reapplied the cleaning decisions from Assignment 2: preserve the original columns and raw price text; remove one exact duplicate only from the analytical table; parse commas before numeric conversion; retain the missing price; normalize `oil b` in a separate item column; and retain/flag the unusual values 9999 and 108.5 without guessing replacements.

## Method

* Mean and median are calculated for each item from all available numeric prices in the analytical table. Missing values are omitted; flagged numeric values remain included.
* For first-to-last percentage change, calculate each item's mean across available cities on its first date and last date, then divide the difference by the first-date mean and multiply by 100.
* Use Sweetener D for the flagged-value comparison because it contains the flagged 9999 observation. Compare its mean and median with 9999 included and with that one value excluded. The second calculation is a sensitivity check, not a correction.

## Why compare mean and median?

The mean uses every numeric value and can be strongly affected by an extreme value. The median is the middle observation and is more resistant to extremes. Neither statistic by itself explains why prices changed or proves what a typical shopper paid.

## Attempt-before-AI record

I did not make a separate attempt before using AI assistance. The method and calculations documented above were developed with AI assistance, and I have reviewed the results in Jupyter Notebook. This document describes the analysis method; it does not pretend to be a timestamped independent attempt.



\## Draft referee's note (O/E/C)



O: The teaching dataset contains 97 original rows, with 96 rows remaining after one exact duplicate was excluded from analysis.



O: Oil B has a mean of PKR 316.18 and a median of PKR 324.00.



O: Pulse C has a mean of PKR 205.91 and a median of PKR 206.00.



O: Staple A has a mean of PKR 160.89 and a median of PKR 112.50.



O: Sweetener D has a mean of PKR 576.81 and a median of PKR 167.75.



O: Sweetener D's mean falls to PKR 167.15 when the flagged 9999 observation is excluded for a sensitivity check.



E: The large difference between Sweetener D's mean and median is consistent with the extreme value pulling the mean upward.



E: The suspicious Oil B value of 108.5 may indicate a category mismatch, but this cannot be confirmed from the available data.



C: The data prove that a real-world event caused prices to rise. (Unsupported causal claim; reject.)



O: The dataset is labelled illustrative teaching data, not actual PBS observations.



SHA-256 hash:

8768C2098AE68128E5C56F23AC7D9D5A67921C0B3D6FB043AB3A5EBCC2862ADE

