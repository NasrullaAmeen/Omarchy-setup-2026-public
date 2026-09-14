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

Details, install commands, and management: [`fixes/009-omarchy-plugins.md`](fixes/009-omarchy-plugins.md).

The wallpaper-span add-on is fully restorable: the plugin patch lives in
[`config/patches/wallpaper-align.span.patch`](config/patches/wallpaper-align.span.patch)
and the framing config, toggle script, and Style-menu entry are mirrored in
[`config/omarchy/`](config/omarchy/) — `bash config/restore.sh` (step 7)
re-applies all of it on a fresh install or after `omarchy plugin update`
reverts the patch.

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

## Public mirror

This repo is private. A sanitized subset is mirrored to a separate public repo
with no shared git history, so nothing ever leaks through "removed" commits.

- Allowlist: [`PUBLIC_MANIFEST.txt`](PUBLIC_MANIFEST.txt) — only paths listed here
  are published.
- Sync: `scripts/sync-to-public.sh` (add `--push` to also push).
