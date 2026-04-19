# YouTube Playlist Player

**[Live Site](https://louis.au/yt-player/)**

A self-hosted, always-on YouTube playlist player designed for unattended 24/7 playback. Paste a YouTube playlist URL to generate a persistent player link that automatically recovers from errors, buffering stalls, and other interruptions.

## Features

- **Continuous playback** — automatically advances through a playlist and loops when finished
- **Shuffle on loop** — optionally re-shuffle the playlist order each time it loops
- **Resume on reload** — saves progress (video index and timestamp) to `localStorage` so playback resumes where it left off after a page refresh or crash
- **Watchdog recovery** — detects buffering stalls, stuck playback, and unresponsive player states; automatically reloads to keep playback running
- **Error skipping** — skips videos that can't be embedded (error 101/150) and continues with the next track
- **Background playlist refresh** — periodically re-fetches the playlist from YouTube so newly added or removed videos are picked up without interrupting playback
- **Uptime Kuma integration** — optional heartbeat URL that pings an [Uptime Kuma](https://github.com/louislam/uptime-kuma) push monitor every 60 seconds while playing, including the current video title

## Usage

1. Open the [generator page](https://louis.au/yt-player/)
2. Paste a YouTube playlist URL
3. Configure options (loop, shuffle, heartbeat)
4. Click **Generate** to get a player URL
5. Open the player URL in a browser — it will play continuously

## How It Works

- **`index.html`** — URL generator UI. Extracts the playlist ID from a YouTube URL and builds a `player.html` link with the selected options as query parameters.
- **`player.html`** — The player itself. Loads the YouTube IFrame API, manages playlist advancement manually, and runs a 1-second watchdog loop to detect and recover from failures.

## Query Parameters (player.html)

| Parameter   | Description                          | Default |
|-------------|--------------------------------------|---------|
| `list`      | YouTube playlist ID (required)       | —       |
| `loop`      | Loop playlist (`1` or `0`)           | `1`     |
| `shuffle`   | Shuffle on each loop (`1` or `0`)    | `0`     |
| `heartbeat` | Uptime Kuma push URL                 | —       |
