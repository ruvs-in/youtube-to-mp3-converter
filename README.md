<div align="center">

<img src="https://img.shields.io/badge/www.ruvs.in-YouTube%20to%20MP3%20&%20MP4%20Converter-FF0000?style=for-the-badge&logo=youtube" alt="ruvs.in YouTube Converter" width="400"/>

# YouTube to MP3 & MP4 Converter

### Fast · Free · No Signup · Transparent

**The most straightforward online YouTube downloader — convert any YouTube video to MP3 or MP4 in seconds.**

🌐 **[Try it Live → www.ruvs.in](https://www.ruvs.in/)**

[![Website](https://img.shields.io/website?url=https%3A%2F%2Fwww.ruvs.in&label=ruvs.in&style=flat-square&color=brightgreen)](https://www.ruvs.in/)
[![Ko-fi](https://img.shields.io/badge/Support%20Me-Ko--fi-FF5E5B?style=flat-square&logo=ko-fi&logoColor=white)](https://ko-fi.com/ruvsin)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=flat-square)](https://opensource.org/licenses/MIT)
[![Python](https://img.shields.io/badge/Python-3.11%2B-blue?style=flat-square&logo=python)](https://python.org)
[![Flask](https://img.shields.io/badge/Flask-3.0-black?style=flat-square&logo=flask)](https://flask.palletsprojects.com/)
[![yt-dlp](https://img.shields.io/badge/yt--dlp-latest-orange?style=flat-square)](https://github.com/yt-dlp/yt-dlp)

</div>

---

## 🚀 What Is This?

This repository contains the **open-source backend** powering **[www.ruvs.in](https://www.ruvs.in/)** — a free, fast, and privacy-friendly YouTube to MP3 and MP4 converter built with Python, Flask, `yt-dlp`, and `ffmpeg`.

> **Just want to convert a video?** 👉 Go straight to **[www.ruvs.in](https://www.ruvs.in/)** — no installation, no sign-up, works instantly in your browser.

---

## ✨ Features

| Feature | Details |
|---|---|
| 🎵 **YouTube → MP3** | 64 / 128 / 192 / 320 kbps quality options |
| 🎬 **YouTube → MP4** | 360p / 480p / 720p / 1080p resolutions |
| ⚡ **Streaming Download** | Chunked streaming — no server-side storage |
| 🔒 **Privacy First** | No login, no tracking, no ads |
| 🛡️ **Bot Protection** | Rate limiting + User-Agent validation |
| 🌍 **CORS Ready** | Configurable allowed origins |
| 🩺 **Health Endpoint** | `/healthz` for uptime monitoring |
| 🐳 **Docker Ready** | One-command deploy with `ffmpeg` pre-bundled |

---

## ☕ Support This Project

If **[www.ruvs.in](https://www.ruvs.in/)** saved you time, consider buying me a coffee! It helps keep the servers running and motivates me to keep building free tools. 🙏

<div align="center">

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/ruvsin)

</div>

Every coffee = more features, better uptime, and a very happy developer. ☕

---

## 🌐 Live Demo

> 🔗 **[https://www.ruvs.in/](https://www.ruvs.in/)**

Paste any YouTube URL → choose MP3 or MP4 → click Download. That's it.

No watermarks. No file size limits on the frontend. No email required.

---

## 🛠️ Tech Stack

```
YouTube URL
    │
    ▼
Flask API  ──►  yt-dlp  ──►  ffmpeg  ──►  Streamed file to browser
    │
    └── Rate Limiting · Bot Blocking · CORS · Security Headers
```

| Layer | Technology |
|---|---|
| Web Framework | Python · Flask 3.0 |
| Video Extraction | yt-dlp (latest) |
| Audio/Video Processing | ffmpeg |
| Production Server | Gunicorn (multi-threaded) |
| Deployment | Docker · Render.com |

---

## 📦 Self-Hosting Guide

Want to run your own instance? Here's how.

### Prerequisites

- Python 3.11+
- `ffmpeg` installed on your system
- Docker (optional, recommended)

### Option 1 — Docker (Recommended)

```bash
# Clone this repo
git clone https://github.com/ruvs-in/yt-converter.git
cd yt-converter

# Build and run
docker build -t yt-converter .
docker run -p 10000:10000 yt-converter
```

Your server is now live at `http://localhost:10000` 🎉

### Option 2 — Manual (Python)

```bash
# Clone and enter
git clone https://github.com/ruvs-in/yt-converter.git
cd yt-converter

# Install dependencies
pip install -r requirements.txt

# Run with Gunicorn
gunicorn app:app --workers 1 --threads 8 --timeout 0 --bind 0.0.0.0:10000

# Or for local dev
python app.py
```

### Environment Variables

| Variable | Default | Description |
|---|---|---|
| `SECRET_KEY` | `change-me-in-prod` | Flask secret key |
| `MAX_DURATION` | `1800` | Max video duration in seconds (30 min) |
| `ALLOWED_ORIGINS` | *(empty = all)* | Comma-separated CORS origins |
| `YOUTUBE_COOKIES_B64` | *(empty)* | Base64-encoded Netscape cookies file |

> ⚠️ Always set a strong `SECRET_KEY` in production.

---

## 📡 API Reference

### `POST /info`

Fetch video metadata without downloading.

**Request:**
```json
{ "url": "https://www.youtube.com/watch?v=xxxxxxxxxxxx" }
```

**Response:**
```json
{
  "title": "",
  "duration": ""
}
```

---

### `GET /stream`

Stream the converted file directly to the browser.

**Query Parameters:**

| Parameter | Required | Values | Default |
|---|---|---|---|
| `url` | ✅ | Any YouTube URL | — |
| `format` | ✅ | `mp3` or `mp4` | `mp3` |
| `quality` | ❌ | MP3: `64`,`128`,`192`,`320` · MP4: `360p`,`480p`,`720p`,`1080p` | `192` / `720p` |

**Example — Download MP3 at 320 kbps:**
```
GET /stream?url=https://youtu.be/dQw4w9WgXcQ&format=mp3&quality=320
```

**Example — Download MP4 at 1080p:**
```
GET /stream?url=https://youtu.be/dQw4w9WgXcQ&format=mp4&quality=1080p
```

**Response:** Streams the binary file with correct `Content-Disposition` headers for auto-download.

---

### `GET /healthz`

Returns `200 ok` — used for uptime checks and load balancer pings.

---

## 🔒 Security Design

- **Rate Limiting:** `/info` → 20 req/min · `/stream` → 5 req/min per IP
- **Bot Blocking:** Rejects known crawler/scraper User-Agents automatically
- **URL Validation:** Only accepts `youtube.com`, `youtu.be`, `music.youtube.com`, `m.youtube.com`
- **Security Headers:** `X-Frame-Options: DENY`, `X-Content-Type-Options: nosniff`, `Referrer-Policy: no-referrer`, `Permissions-Policy`
- **No Storage:** Files are streamed directly — nothing is ever saved on the server

---

## 🐳 Dockerfile Overview

```dockerfile
FROM python:3.11-slim

# ffmpeg bundled at build time
RUN apt-get install -y ffmpeg

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app.py .

EXPOSE 10000
CMD ["gunicorn", "app:app", "--workers", "1", "--threads", "8", "--timeout", "0", "--bind", "0.0.0.0:10000"]
```

Single-stage, slim Python image. Gunicorn runs 1 worker × 8 threads to handle concurrent streaming without memory bloat.

---

## 📂 Project Structure

```
yt-converter/
├── app.py              # Main Flask application
├── Dockerfile          # Docker image definition
├── requirements.txt    # Python dependencies
└── README.md           # You are here
```

---

## ❓ FAQ

**Q: Is ruvs.in free to use?**
A: Yes — 100% free, no account required. Visit **[www.ruvs.in](https://www.ruvs.in/)**.

**Q: What YouTube URLs are supported?**
A: `youtube.com`, `youtu.be`, `music.youtube.com`, and `m.youtube.com`.

**Q: What's the maximum video length?**
A: The default backend limit is 30 minutes. You can increase this via `MAX_DURATION` when self-hosting.

**Q: Is the downloaded file stored on the server?**
A: No. Files are streamed directly to your browser — nothing is ever saved.

**Q: Why am I getting rate-limited?**
A: The API limits stream requests to 5 per minute per IP to prevent abuse. Wait 60 seconds and try again.

**Q: Does this work with YouTube playlists?**
A: No — playlists are intentionally blocked (`--no-playlist`) for single-video safety.

**Q: Can I use this commercially?**
A: This code is MIT licensed. However, you are responsible for complying with YouTube's Terms of Service.

---

## ⚖️ Disclaimer

This project is intended for **educational and personal use only**. Downloading YouTube content may violate [YouTube's Terms of Service](https://www.youtube.com/t/terms). Always respect copyright law and only download content you have the legal right to download. The author assumes no responsibility for misuse.

---


## 👨‍💻 Author

**M. Rahul** — building free tools at **[ruvs.in](https://www.ruvs.in/)**

- 🌐 Website: [www.ruvs.in](https://www.ruvs.in/)
- 🐙 GitHub: [@ruvs-in](https://github.com/ruvs-in)
- ☕ Ko-fi: [ko-fi.com/ruvsin](https://ko-fi.com/ruvsin)

---

## 📄 License

```
MIT License

Copyright (c) 2026 M. Rahul

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

See [LICENSE](LICENSE) for the full text.

---

<div align="center">

**⭐ Star this repo if it helped you — it helps others find it too!**

Made with ❤️ by [M. Rahul](https://www.ruvs.in/) · [ruvs.in](https://www.ruvs.in/)

</div>
