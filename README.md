# Omarchy setup 2026

Personal Omarchy (Arch Linux + Hyprland) setup, config, and a log of fixes and
enhancements made along the way.

## Default Apps

- **Terminal:** [foot](https://codeberg.org/dnkl/foot)
- **Main Browser:** [Zen Browser](https://zen-browser.app/)
- **Second Browser:** [Brave](https://brave.com/)
- **Main Editor:** [Orca](https://www.onorca.dev/)
- **Second Editor:** [Neovim](https://neovim.io/)

## Omarchy Plugins

Installed third-party plugins (from the
[Omarchy plugin marketplace](https://plugins.omarchy.org/)):

- Screens
- oShelf
- OmaGlass
- Hardware Monitor
- Netspeed
- Wallpaper Align (patched with span mode; toggle in Style menu)
- Days
- Omarchy Google Calendar and Clock (local calendar via Caldir; bridge for Days)
- OmiHaze (dims inactive windows)
- Paper Mode (paper/e-ink screen shaders from the bar; patched so its widget
   reaches its service over IPC under the Bar Screens replacement bar)
- Keystroke (Raycast-style command palette; it replaces the Omarchy menu —
  `Super+Space` now opens it)
- Onote (sticky notes as ordinary tiled Hyprland windows; `Super+N` for
  notes & stack, `Super+Alt+N` new note, `Super+Alt+H` stack all)
- Mouseless (keyboard-driven pointer — `Super+Alt+M` opens a lettered hint
  grid to warp and click without the mouse)
- YouTube Float (browse YouTube Watch Later/subscriptions/search from a
  picker and play in a floating pinned mpv window; `Super+Alt+Y` and overlay
  keys)
- FossFetch (one-click package search + install across Pacman, AUR and
  Flathub from the bar; natural-language category browsing)
- Hyprpin (compositor-level picture-in-picture — pin windows across
  workspaces to a corner, edge, display, or scratchpad)
- OmaIce (Ice-style hidden bar section — a chevron that replaces the stock
  tray and tucks bar widgets + tray icons away)
- Oma Aux (PipeWire patchbay — drag-to-connect routing graph for apps,
  mics, sinks, and filters, with per-route balance/EQ and per-app volume/mute)
- App Workspaces (workspace pills with real app icons instead of bare
  numbers, ordered like the windows; replaced the stock workspaces widget)
- Garden (private, local-only coding-activity heatmap — half-hour squares
  from watched folders, not GitHub or a token meter)
- King (fullscreen searchable keybindings overlay; `Super+Shift+K`)
- NetNeighbors (Wi-Fi radar for the bar — every device on the network with
  vendor/hostname/MAC, rootless, no daemon)
- Music Dock (Spotify "minimize": tray-style bar icon + `Super+Shift+M`
  drop-down workspace, works with the Brave-hosted web app on this machine)
- OmaListen (lecture/audiobook player — resume, speed, sleep timer,
  playlists, bookmarks; `Super+Shift+L`)
- Pomodoro (Braun-style focus/rest timer for the bar)
- Jot (minimalist sticky notes deck at the screen edge — a second,
  different-shaped note tool alongside Onote)
- WaveBar (now-playing bar widget with a live PipeWire waveform + MPRIS
  transport controls — prev/play-pause/next, ±10s skip, shuffle, repeat
  all/one, volume; patched — both the bar icon and its click-to-expand
  panel reach their service over IPC under a replacement bar, see below)
- SIA (local, git-versioned machine memory — journal/git/package-install
  evidence into a knowledge graph with local embeddings via Ollama; resident
  `sia-brainstem.service`; `sia ask "…"` / `sia status`)
- NVIDIA Hybrid (bar telemetry — util/mem/temp/clock — for a hybrid
  Intel+NVIDIA laptop, plus a GPU picker for new apps)
- USBGuard (kernel-level USB device authorization / BadUSB protection;
  installed widget-only — the whitelist-by-default daemon setup was
  deliberately not run, see below)
- DevTrack (LeetCode/Codeforces/GitHub coding-streak tracker + heatmap for
  the bar)
- Wallpaper Engine (plays Steam Wallpaper Engine Workshop items via
  `linux-wallpaperengine`; no Steam on this machine, so wired up a real
  local MP4 as a hand-built Workshop item instead — see below)
- Restic Monitor (bar widget watching `restic-backup*.service` systemd
  units — no backups configured on this machine yet)

Details, install commands, and management — including **auto-reminders for
timed calendar notes** (a note with a time triggers an Omarchy reminder at its
start, with a notification sound), the **Paper Mode** and **WaveBar** patches,
and a few plugins tried and removed (YouTube Music — Brave/Bar-Screens
patches worked, but blocked by a regional restriction; Airwaves — needs a
media server this machine doesn't have installed; OmaSpotify — Premium-only
for local playback, and a Brave-MPRIS patch only ever matched a subset of
what WaveBar + Music Dock already cover for the free account; RollingShot —
full-page browser screenshots, but its "page" mode needs Chromium
specifically, not installed here, rebound its keybind to Chromium-free
scroll-stitch mode first, then removed later at request; `cinco/omarchy-5bars`
— per-monitor bar layouts, looked broken [visually corrupted] but that was
actually an unrelated, recurring `bar.transparent` bug, not 5bars itself —
removed anyway in favor of the stock bar). Also tried and reverted: a
second, bottom-anchored bar — a floating dock plugin (styled to match,
still not what was wanted), then duplicating Omarchy's own bar inside its
shell process (broke the live shell — reverted immediately), then a
genuinely separate `quickshell -c bottom-bar` process hosting the real
WaveBar/OmaGlass/Hardware-Monitor/OmaListen plugin files with live data
(worked, but its window sat one compositor layer below Omarchy's own
always-on notification overlay, which silently absorbed every click meant
for it — abandoned before confirming the fix). Also: Bar Screens itself was
later removed in favor of the stock bar. Full story, including the
technique for loading real Omarchy bar-widget plugins in an independent
Quickshell process (in case it's ever revisited), the SIA "first light"
setup, and how a plain local MP4 got wired up as a Wallpaper Engine item
with no Steam involved:
[`fixes/009-omarchy-plugins.md`](fixes/009-omarchy-plugins.md).

The wallpaper-span, Paper Mode, and WaveBar add-ons are fully restorable:
their plugin patches live in
[`config/patches/wallpaper-align.span.patch`](config/patches/wallpaper-align.span.patch),
`config/patches/paper-mode.hostipc.patch`, and
`config/patches/wavebar.hostipc-and-frames.patch`, and the framing config,
toggle script, and Style-menu entry are mirrored in
[`config/omarchy/`](config/omarchy/) — `bash config/restore.sh` (steps 7, 7c,
10) re-applies all of it on a fresh install or after `omarchy plugin update`
reverts the patches.

## Customization of Apps

## Contents

- [`fixes/`](fixes/INDEX.md) — one note per fix/enhancement, indexed in
  [`fixes/INDEX.md`](fixes/INDEX.md). Copy [`fixes/_template.md`](fixes/_template.md)
  for a new entry.
- [`config/`](config/README.md) — the actual custom config files from
  [`fixes/008`](fixes/008-reapply-after-reinstall.md), restorable with
  `bash config/restore.sh` (re-applies everything after `omarchy update` or a
  reinstall).

### Fixes

| Date       | Title                                           | File                                                                             |
| ---------- | ----------------------------------------------- | -------------------------------------------------------------------------------- |
| 2026-09-01 | Multi-monitor detection and arrangement         | [001-multi-monitor-screens-plugin.md](fixes/001-multi-monitor-screens-plugin.md) |
| 2026-09-02 | Removing unwanted default apps — full inventory | [003-app-cleanup.md](fixes/003-app-cleanup.md)                                   |
| 2026-09-02 | Num Lock not on by default (Hyprland + SDDM)    | [004-numlock.md](fixes/004-numlock.md)                                           |
| 2026-09-02 | Lock screensaver to the matrix effect only      | [005-screensaver-matrix-effect.md](fixes/005-screensaver-matrix-effect.md)       |
| 2026-09-02 | Replace default Omarchy branding with custom logo | [006-custom-branding.md](fixes/006-custom-branding.md)                         |
| 2026-09-02 | Replace Plymouth boot splash and SDDM login logo | [007-plymouth-sddm-branding.md](fixes/007-plymouth-sddm-branding.md)             |
| 2026-09-14 | Re-applied all fixes on a fresh install, with `config/` backups | [008-reapply-after-reinstall.md](fixes/008-reapply-after-reinstall.md) |
| 2026-09-14 | Omarchy plugins from the official marketplace | [009-omarchy-plugins.md](fixes/009-omarchy-plugins.md)                    |
| 2026-09-14 | Hyprland crash — `SUPER+T` on a pinned floating overlay | [010-hyprland-dwindle-crash.md](fixes/010-hyprland-dwindle-crash.md)      |

## Public mirror

This repo is private. A sanitized subset is mirrored to a separate public repo
with no shared git history, so nothing ever leaks through "removed" commits.

- Allowlist: [`PUBLIC_MANIFEST.txt`](PUBLIC_MANIFEST.txt) — only paths listed here
  are published.
- Sync: `scripts/sync-to-public.sh` (add `--push` to also push).
