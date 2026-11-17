# YouSearch

A multi-source internet search engine written in OcaltQL, in a single file: [`index.oql`](index.oql).

## Live

https://yousearch.ocalt.site

## Run

Serve `index.oql` as a site file. The script reads its input from the request's query string and emits a complete HTML page.

## Input

| Parameter | Meaning |
|---|---|
| `q` | The search query. Without it, the landing page is shown. |
| `news=1` | News tab. |
| `images=1` | Images tab. |
| `research=1` | Research tab. |
| `code=1` | Code tab. |
| `books=1` | Books tab. |

With no tab parameter, the All tab is shown. Use one tab parameter per request; the script does not define combinations.

Examples:

```
?q=ocaltql
?q=ocaltql&news=1
?q=quantum%20error%20correction&research=1
```

A query over 250 characters is refused with the message "Your search query must not exceed 250 characters."

## Output

A complete HTML page, sent with `Content-Type: text/html` and `Cache-Control: no-store`. The page title is `<query> - YouSearch`.

## Sources

Only the sources the active tab needs are fetched. There are no API keys.

| Tab | Source | Results | Read as |
|---|---|---|---|
| All | DuckDuckGo | up to 12 | HTML page, sponsored results removed, redirect links unwrapped to the real address |
| All | Mojeek | up to 10 | HTML page |
| All | Wikipedia | up to 6, shown as a knowledge panel with thumbnail and a three-sentence extract | JSON API |
| All and News | GDELT | up to 15, English sources, hybrid relevance, with date | JSON API |
| Images | Wikimedia Commons | up to 30 bitmap images, 400 px thumbnails | JSON API |
| Research | arXiv | up to 15, by relevance, with abstract and authors | HTML page |
| Code | GitHub repository search | up to 10, with stars and language | JSON API |
| Code | Stack Overflow, through the Stack Exchange API | up to 10, with votes, answer count and answered state | JSON API |
| Books | Open Library | up to 12, with up to three authors and first publication year | JSON API |

The News tab shows GDELT only.

## How it works

The script has five sections.

1. **Read input and initialise.** Reads `q`, trims it, URL-encodes it, and creates an empty array and a zero count for every source.
2. **Tab state.** A tab is a GET flag. The flag sets which sources are fetched, which tab link is active, the hidden field that keeps the tab when the user searches again, and the class on `<body>`.
3. **Search execution.** For each source in use, the script opens a `BROWSER` session on the source's address, waits, runs a page-side `EVALUATE` that reduces the page to a JSON array of records, parses it with `PARSE ... AS JSON`, closes the session and counts the records.
4. **Construct the page.** Builds the page with the `HTML` verbs: head and stylesheet, landing page or results page, a sticky header with wordmark, search box and tabs, the result lists, and the footer.
5. **Return the page.** `EMIT` sends it.

Every source is reduced to the same record: `title`, `url`, `site`, `path`, `description` and `meta`, with `image` where a source has one. One result renderer per tab then draws the records.

## Dependencies

- The OcaltQL `BROWSER` verbs, used to read every source.
- Third-party services: DuckDuckGo, Mojeek, Wikipedia, GDELT, Wikimedia Commons, arXiv, GitHub, Stack Exchange and Open Library.
- DuckDuckGo, Mojeek and arXiv are read from their HTML pages by CSS selector. The other sources are read from JSON APIs.

## Page

- Light and dark colour schemes, following the visitor's system setting.
- A layout adjustment at 800 px width.
- A favicon embedded in the page as a data address.
- No client-side requests: the script's header states "No POST body parsing. No AJAX. No API keys."

## Files

| File | Purpose |
|---|---|
| [`index.oql`](index.oql) | The whole project. |
