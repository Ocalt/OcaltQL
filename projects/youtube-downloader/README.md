# YouSave: a YouTube downloader in OcaltQL

Paste a YouTube link, pick a quality, and choose where the file goes. A progress screen follows the download, and the result page plays the video and hands you the file.

The whole app is one file, `index.oql`: the form, the progress screen, the result page, the folder picker and the download engine.

## What it does

- **Qualities:** Best, 1080p, 720p and 480p as MP4, or the sound alone as MP3.
- **Where the file can go:**
  - **Large storage:** `/mounted/<folder>`.
  - **My files:** `/root/<folder>`.
  - **One of my computers:** any folder on a machine running the OcaltQL Client, chosen in a folder picker that lists your computers, their drives and their folders.
  - **This device:** a download button and a link for the phone or computer you are using.
- **Progress:** live, with percentage, size, speed and time left.
- **Result page:** plays the video or the MP3, and offers Download, Copy link, and Stop sharing (or Remove from the cloud).

## Install

1. Put `index.oql` in a folder of your own, for example `/mounted/yousave`. Upload it with Ocaltio or FTP, or write it from a script.
2. Give the folder an address:

   ```
   SUBDOMAIN ADD "yousave" AT "/mounted/yousave" SET ?site
   AFTER EMIT ?site("url")
   ```

3. Open `https://yousave.ocalt.site`. Use your own subdomain name if `yousave` is taken.

The first download takes about a minute longer than the rest, because YouSave installs its engine (yt-dlp and FFmpeg) into your storage. After that it starts straight away.

## Settings

The settings are at the top of `index.oql`:

| Setting | Default | What it does |
|---|---|---|
| `?access_pin` | `""` | Set a PIN and the site asks for it before anything else. **Set one.** A site answers anyone who has its address, and every download is saved to your storage and can be sent to your computers. |
| `?home` | `/mounted/.yousave` | Where the engine, the progress files and the copies made for a device or on their way to a computer are kept. |
| `?default_folder` | `youtube` | The folder suggested under `/root` or `/mounted`. |
| `?device_only` | `""` | Set it to `"1"` for a copy anyone may use. Visitors can then only download to their own device. They can't reach your storage or your computers, and their copies are removed within a day. |

## How it works

| Step | What happens |
|---|---|
| `step=work` | Checks the link, looks up the title and thumbnail, gives the download a job number and shows the progress screen. |
| `step=run` | Called by the progress screen. Runs yt-dlp in a `WEBASSEMBLY` Python sandbox, then shares the file (`FILE SHARE`) or sends it to a computer (`DIRECTIVE ... UPLOAD`). |
| `step=progress` | Returns what the engine last wrote: state, percentage, bytes, speed and time left. |
| `step=status` | Follows a copy to a computer, then removes the copy kept in the outbox. |
| `step=done` | The result page. |
| `step=pick` | The folder picker for your computers (`DIRECTIVE LIST`, `LIST DRIVES`, `LIST FROM`). |
| `step=forget` | Turns the link off, and deletes a copy that was made only for a device. |

If YouTube changes and a download fails, YouSave updates yt-dlp and tries once more by itself.

## Requirements

- An Ocalt account. Large storage (`/mounted`) needs a paid plan. On Free, set `?home` to a folder under `/root` and choose My files; `/root` holds 1 GB.
- To save to a computer, that computer needs the OcaltQL Client, signed in and online.

## Please note

Download only videos you have the right to keep, such as your own uploads or videos whose licence allows it. YouTube's terms apply to what you download.
