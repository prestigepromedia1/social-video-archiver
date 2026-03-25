# social-video-archiver — CLAUDE.md

## Project
Batch download, rename, and optionally upload social media videos from any platform supported by yt-dlp.

**Stack:** Python · yt-dlp · Google Drive API (optional)

## Commands
```bash
# Download and archive locally
python social_video_archiver.py urls.txt

# Custom output directory and naming
python social_video_archiver.py urls.txt --output-dir ./videos --template "{platform}_{creator}_{date}"

# Upload to Google Drive
python social_video_archiver.py urls.csv --drive-folder YOUR_FOLDER_ID
```

## Pipeline
```
urls.txt/csv → parse → yt-dlp download → extract metadata → apply naming template → save/upload
```
- Failed downloads log error and continue to next URL
- CSV log generated in output dir: url, creator, platform, filename, status, drive_id, link, error

## Key Files
- `social_video_archiver.py` — Main CLI
- Input: text file (one URL per line) or CSV with `url`/`link`/`video` column

## Naming Templates
| Token | Description | Example |
|-------|-------------|---------|
| `{creator}` | Username/uploader | `dancequeen` |
| `{platform}` | Detected platform | `tiktok` |
| `{date}` | Download date (YYYYMMDD) | `20260303` |
| `{id}` | Video ID from platform | `7123456789` |
| `{title}` | Video title from metadata | `my_cool_video` |

Default: `{creator}_{platform}_{date}_{id}`

## Gotchas
- `pip install yt-dlp` is the only hard dependency
- Google Drive upload requires OAuth `credentials.json` (never committed)
- yt-dlp supports 1000+ sites — if a URL works in yt-dlp, it works here
- Output defaults to `./archived/`

## Ecosystem Context
<!-- last_ecosystem_sync: 2026-03-23 -->
This tool is part of the PPM Compounding Delivery System (10 tools across 2 orgs).

- **Role in ecosystem:** Content archival. Batch downloads social media videos from any yt-dlp-supported platform (TikTok, IG Reels, YouTube Shorts, Twitter/X, etc.) for reference and reuse. Feeds the content library that clip-engine and Creative Pipeline can draw from.
- **Reads from:**
  - Social media URLs (text file input)
- **Writes to:**
  - Local archive / Google Drive → organized video files with CSV log
  - clip-engine → archived videos available as source material for clip generation
- **Current gaps:**
  - No automated feed to clip-engine (manual file handoff)
  - No metadata sync to Data Warehouse or any central asset index
- **Ecosystem map:** See `prestigepromedia1/beirut` repo for full architecture
- **Key constraint:** social-video-archiver's north star comes first. Ecosystem connections are additive.
- **Boundary rule:** North star wins inside this tool. Holistic design wins at interfaces between tools.
