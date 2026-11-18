# Casebook

Bugs that were hard to find and are now fixed. Each case is one file, so the next person who meets the same symptom finds the cause in minutes.

A case belongs here when the cause was not obvious from the symptom, even if it turned out to be an ordinary mistake in the script.

## Where each kind of finding goes

| Finding | Goes to |
|---|---|
| A syntax error that looked like a runtime defect | A case here. |
| A script mistake that took a long time to see | A case here. |
| A runtime defect that was fixed in the runtime | A case here, and the changelog at https://ocalt.com/changelog. |
| A tutorial page that was wrong or unclear | Fix the tutorial. A case here records what misled the reader. |
| A construct the grammar rejected or accepted wrongly | Fix [`grammar/tools/grammar.py`](../grammar/tools/grammar.py), and add the snippet to `grammar/tools/examples.py`. |

## Adding a case

Copy [`TEMPLATE.md`](TEMPLATE.md) to `NNNN-short-title.md`, numbered in order. Do not edit a case after it is merged, except to add a link to a later case that supersedes it.

## Index

| No. | Case |
|---|---|
