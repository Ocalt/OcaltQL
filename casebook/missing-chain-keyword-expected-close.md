# Case: "Expected CLOSE" when the real problem is a missing AFTER

**Error message:** `Line N: Expected CLOSE.`
**Category:** Syntax / statement chaining
**Severity:** Script will not parse
**Status:** Resolved

## Summary

A script fails to parse with `Expected CLOSE` even though every `OPEN` has a
matching `CLOSE`. The cause is two statements placed one after the other with
no chain keyword (`AFTER`, `AND` or `OR`) between them. The parser ends the
block early, then complains that it did not find `CLOSE`.

## The rule

A newline does not end a statement in OcaltQL. Statements in a script are
joined only by `AFTER`, `AND` or `OR`. Blank lines and indentation carry no
meaning.

```
script ::= statement? (chain statement)*
chain  ::= "AFTER" | "AND" | "OR"
block  ::= "OPEN" script "CLOSE"
```

## Broken code

A heading followed directly by an `IF`:

```
HTML "h2"
  ATTR "class" WITH "section-title"
  TEXT "Research papers"

IF ?arxiv_count IS GREATER THAN 0
OPEN
  FOREACH ?arxiv_results SET ?item
  OPEN
    HTML "p"
      TEXT ?item("title")
  CLOSE
CLOSE
```

Result:

```
Line 5: Expected CLOSE.
```

(The line number is the line of the `IF`, not of any `OPEN` or `CLOSE`.)

## Why the message is misleading

The parser finishes the `HTML "h2"` statement after its `TEXT` argument. The
next token is `IF`, which is a statement keyword and not a chain keyword, so
the parser decides the current block's script has ended. It then requires
`CLOSE` and finds `IF`. The message reports what the parser wanted at the point
where it gave up. It does not report what the author left out, which sits one
token earlier.

## Fixed code

```
HTML "h2"
  ATTR "class" WITH "section-title"
  TEXT "Research papers"

AFTER IF ?arxiv_count IS GREATER THAN 0
OPEN
  FOREACH ?arxiv_results SET ?item
  OPEN
    HTML "p"
      TEXT ?item("title")
  CLOSE
CLOSE
```

## How to diagnose

1. Go to the line in the error message.
2. Look at the statement immediately before it.
3. Check whether the two are joined by `AFTER`, `AND` or `OR`.

If you want to find every occurrence in a large file, look for any statement
keyword (`HTML`, `IF`, `FOREACH`, `WHILE`, `LOOP`, `SWITCH`, `RETURN`) whose
previous token is not `OPEN`, `AFTER`, `AND` or `OR`.

Things that look like hits but are not errors:

- `SET ?variable` at the end of a statement is a trailing modifier, not a new
  statement.
- `NEW HTML` is a single verb, so the `HTML` there does not start a statement.
- A leading `AFTER` as the first thing in a block is valid, because the first
  statement is optional in the grammar.

## Convention that avoids it

Start the first statement in a block bare, and put `AFTER` in front of every
statement after it.

## Origin

Found in a search-results page script (`index.oql`, about 1,700 lines). The same
mistake appeared five times, once in each result section that used the pattern
"heading, then `IF` on a result count". The pattern was copied between
sections, so fixing it once and then searching for it caught all five.

## Notes

- `AND` and `OR` have two roles. Between statements they are chain keywords.
  Inside a condition they are logical operators. The parser tells them apart
  by position.
- This case was checked against the published grammar (EBNF/GBNF derived from
  the language documentation), and the corrected script was confirmed working
  on the runtime by its author.
