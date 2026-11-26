# OcaltQL projects

A library of complete programs written in OcaltQL. Each project runs from a script you can read in one sitting, and each one shows something OcaltQL does that other stacks need many tools to do.

OcaltQL is a verb-first execution language for the entire stack. The language reference and tutorial, which is the specification, are at https://ql.ocalt.com. Documentation is at https://ocalt.com/docs/oql. This repository holds projects written in the language, and the formal grammar of the language in [`grammar`](grammar/README.md). The tutorial remains the specification.

## Projects

| Project | What it is | Live | Folder |
| --- | --- | --- | --- |
| YouSearch | A multi-source internet search engine in a single OcaltQL file. Web, Wikipedia, news, images, research papers, code and books, with no API keys. | https://yousearch.ocalt.site | [`projects/yousearch`](projects/yousearch) |
| YouSave | A YouTube downloader in a single OcaltQL file. Paste a link, pick a quality (MP4 or MP3) and choose where the file goes: your storage, one of your computers, or the device you are using. Live progress, and a result page that plays the video and hands you the file. | Install it on your own subdomain | [`projects/youtube-downloader`](projects/youtube-downloader) |

## Running a project

- **Console.** https://console.ocalt.com runs a script in the browser and shows the result.
- **Site file.** A project that reads `!GET`, `!POST` or `!COOKIE` is meant to be served as a file on a subdomain, where a path maps to a file and the caller's request is available to the script. See https://ocalt.com/docs/oql, section "Scripts as endpoints".

Each project's own README states how it is run and what it needs. Some projects use more than the script itself: YouSave, for example, keeps its engine and downloads in your Ocalt storage, so read its README before installing it.

## Adding a project

See [CONTRIBUTING.md](CONTRIBUTING.md). Start from [`projects/_template`](projects/_template/README.md).

## Links

- OcaltQL language reference: https://ql.ocalt.com
- OcaltQL documentation: https://ocalt.com/docs/oql
- OcaltQL Console: https://console.ocalt.com
- Ocalt: https://github.com/Ocalt/main
