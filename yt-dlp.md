**yt-dlp** is the current recommended CLI tool for downloading YouTube videos (and many other sites). It is the actively maintained fork of the older `youtube-dl`.

### Installation

**Linux / macOS (recommended methods):**
```bash
# Using pipx (isolated, clean)
pipx install yt-dlp

# Or with Homebrew (macOS)
brew install yt-dlp

# Or standalone binary
curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o yt-dlp
chmod +x yt-dlp
```

**Windows:**
- Use `winget install yt-dlp`, Scoop, Chocolatey, or download the `.exe` from the [GitHub releases](https://github.com/yt-dlp/yt-dlp/releases).

You also need **FFmpeg** installed (for merging video + audio streams and converting formats).

### Basic Commands

| Task | Command |
|------|---------|
| **Best quality (video + audio)** | `yt-dlp "URL"` |
| **List available formats** | `yt-dlp -F "URL"` |
| **1080p or lower** | `yt-dlp -f "bv*[height<=1080]+ba/b" "URL"` |
| **Audio only (MP3)** | `yt-dlp -x --audio-format mp3 "URL"` |
| **Audio only (best quality, no re-encode)** | `yt-dlp -x --audio-format best "URL"` |
| **Force MP4 container** | `yt-dlp --merge-output-format mp4 "URL"` |
| **Download playlist** | `yt-dlp "PLAYLIST_URL"` |
| **Only first 10 items of playlist** | `yt-dlp -I 1:10 "PLAYLIST_URL"` |
| **Custom filename** | `yt-dlp -o "%(title)s.%(ext)s" "URL"` |
| **With English subtitles** | `yt-dlp --write-subs --sub-langs en "URL"` |
| **Update yt-dlp** | `yt-dlp -U` |

### Useful Tips

- Always put the URL in quotes: `"https://www.youtube.com/watch?v=..."`
- For age-restricted or bot-checked videos, use browser cookies:
  ```bash
  yt-dlp --cookies-from-browser chrome "URL"
  ```
  (Works with `firefox`, `edge`, `brave`, etc.)
- Avoid `-f best` on modern YouTube — it often only gets lower-quality combined streams. Prefer `bv*+ba` (best video + best audio).

### Quick examples

```bash
# Simple best quality download
yt-dlp "https://www.youtube.com/watch?v=dQw4w9WgXcQ"

# 1080p max, MP4, nice filename
yt-dlp -f "bv*[height<=1080]+ba/b" --merge-output-format mp4 -o "%(title)s.%(ext)s" "URL"

# Extract audio as high-quality MP3
yt-dlp -x --audio-format mp3 --audio-quality 0 "URL"
```

Run `yt-dlp --help` for the full list of options. The tool is updated very frequently to keep up with YouTube changes.