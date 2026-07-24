<div align="center">

# Deckmate

**A beginner-friendly two-deck YouTube DJ mixer — beatmatching, crossfading, and tempo control, all client-side with no backend.**

![Version](https://img.shields.io/badge/version-1.0.0-00D4C8)
![HTML](https://img.shields.io/badge/-HTML-E34F26?logo=html5&logoColor=white)
![Zeabur](https://img.shields.io/badge/-Zeabur-6C5CE7)
![License](https://img.shields.io/badge/license-AGPLv3%20%2F%20Commercial-00D4C8.svg)

</div>

---

## What it does

Deckmate is a single-file, static DJ mixer that runs entirely in the browser. Paste YouTube links, video IDs, or playlist URLs to build a library, then load tracks across two decks with independent volume, tempo, and cue controls. An equal-power crossfader and timed auto-crossfade handle transitions. No login, no build step, no backend — just serve `index.html` over HTTP.

## Features

- Import tracks from YouTube links, bare video IDs, or full playlist URLs — no API key needed
- Shuffle-by-default library with search/filter and manual track picking
- Two independent decks: play/pause, cue, seek, per-deck volume, and real `setPlaybackRate` tempo control (pitch shifts with it — like a real turntable)
- Manual tap-BPM per deck for rough beatmatching
- Equal-power crossfader, master volume, and timed auto-crossfade with optional auto-pause of the outgoing deck

## Tech Stack

| Layer | Choice |
|---|---|
| Frontend | Single-file HTML (`index.html`) — no build step, no dependencies |
| Hosting | Zeabur static hosting (GitHub CI/CD, feature → dev → main) |

## Why it needs real hosting

The app uses YouTube's IFrame Player API, which validates its `postMessage` handshake against a real `http(s)` origin. That handshake breaks under:

- `file://` (double-clicking the HTML file locally) — browsers give every local file a unique, opaque origin
- Claude's in-chat artifact preview — its sandboxed iframe's CSP doesn't allow loading `youtube.com` scripts

So it needs to be served over plain `http(s)` — any static host works. This repo is set up for the usual GitHub → Zeabur pipeline.

## Local development

Local `file://` won't work (see above). Serve it over HTTP instead:

```bash
python -m http.server 8000
# then open http://localhost:8000
```

or with Node:

```bash
npx serve .
```

## Deploy (Zeabur)

Zeabur auto-detects static sites — no config file needed. If a repo isn't tied to a language runtime and has an `index.html` at the root, Zeabur serves it as-is.

1. Push this repo to GitHub.
2. In Zeabur: **Create Project → Deploy from GitHub → select this repo**.
3. Zeabur detects static mode automatically and deploys `index.html`.
4. Generate a domain (or bind your own) from the service's **Networking** tab.

Branching: same `feature → dev → main` flow as the other projects — merge to `main` to trigger a redeploy.

## Known limitations

- **No Spotify.** Full-track playback needs an OAuth Premium-login player that can't be embedded in a shared app — YouTube is the supported source.
- **No real waveform/audio analysis or independent pitch-shifting.** YouTube's iframe doesn't expose raw audio to the browser, so tempo changes pitch too, and BPM is manual tap-tempo rather than auto-detected.
- **No automated tests.** Basic smoke tests for the core crossfade/tempo logic would reduce regression risk as the app grows.

## Versioning

This repository uses a simple major/minor version format:

- the first number increases for major milestones or structural changes (e.g. new deck architecture, new track source, breaking layout changes)
- the second number increases for minor feature updates, workflow improvements, or documentation refinements

## Future Roadmap

**Near-term ideas**
- Auto-detect BPM from tap-tempo history instead of requiring a fresh manual tap per track
- Save/restore library and deck state across sessions (localStorage) so a reload doesn't lose the built playlist
- Keyboard shortcuts for play/pause, cue, and crossfade for a more hands-on-keyboard mixing flow
- Export/import the library as a shareable JSON/text file

**Medium-term ideas**
- Beat-grid markers (manual tap-set) so loops and drops can be lined up visually, not just by ear
- Per-deck EQ (low/mid/high) using the Web Audio API where available, layered on top of the YouTube iframe audio
- Mobile/touch-friendly layout for tablet DJing
- Basic smoke tests for core crossfade/tempo logic, plus a `CONTRIBUTING.md` now that the repo is public

**Longer-term ideas**
- Optional local-file/SoundCloud source support alongside YouTube, for when independent pitch-shifting or offline use matters
- Recording/export of a mixed session (e.g. via MediaRecorder against the tab's audio output)
- Preset/session management — save named sets of decks + crossfade timing for reuse

## Changelog

### v1.0.0 — Initial release
- Two-deck YouTube mixer with play/pause, cue, seek, per-deck volume, and tempo control
- Equal-power crossfader with master volume and timed auto-crossfade (optional auto-pause of outgoing deck)
- Library building from links/IDs/playlists, with shuffle-by-default order and search/filter
- Manual tap-BPM per deck

## License

Dual licensed:

- **Community Edition** — [GNU Affero General Public License v3 (AGPLv3)](LICENSE). Free to use, modify, and self-host. If you distribute a modified version or run it as a network service, you must make the corresponding source available.
- **Commercial License** — for organisations wanting to embed, modify, or distribute without AGPLv3's obligations. See [COMMERCIAL-LICENSE.md](COMMERCIAL-LICENSE.md).

---

<div align="center">
<sub>Built by <a href="https://github.com/TheBooleanJulian">@TheBooleanJulian</a></sub>
</div>
