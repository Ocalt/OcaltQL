# OcaltQL grammar

The formal grammar of OcaltQL, in two forms.

| File | Form |
|---|---|
| [`ocaltql.ebnf`](ocaltql.ebnf) | EBNF |
| [`ocaltql.gbnf`](ocaltql.gbnf) | GBNF (GGML BNF), the grammar format of llama.cpp, used to constrain generated text to a grammar |

## Authority

The specification is the OcaltQL tutorial at https://ql.ocalt.com. These grammars are derived from it.

Where a grammar and the tutorial disagree, the tutorial is correct unless the tutorial has a syntax flaw, and the grammar is the defect.

## Keeping them in step

The two files describe the same language. A change to one is made to the other in the same commit.
