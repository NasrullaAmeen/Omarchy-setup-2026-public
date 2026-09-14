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
- Bar Screens
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
- OmaSpotify (full Spotify client in Quickshell; under Bar Screens its bar
   widget is an icon that opens the full player; local playback needs a
   Spotify Premium account)

Details, install commands, and management — including **auto-reminders for
timed calendar notes** (a note with a time triggers an Omarchy reminder at its
start, with a notification sound) and the **Paper Mode host-IPC patch**:
[`fixes/009-omarchy-plugins.md`](fixes/009-omarchy-plugins.md).

The wallpaper-span and Paper Mode add-ons are fully restorable: their plugin
patches live in
[`config/patches/wallpaper-align.span.patch`](config/patches/wallpaper-align.span.patch)
and `config/patches/paper-mode.hostipc.patch`, and the framing config, toggle
script, and Style-menu entry are mirrored in
[`config/omarchy/`](config/omarchy/) — `bash config/restore.sh` (steps 7, 7c)
re-applies all of it on a fresh install or after `omarchy plugin update`
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
