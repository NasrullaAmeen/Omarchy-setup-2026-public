# Omarchy plugins from the official marketplace

- **Date:** 2026-09-14
- **Modified:** 2026-09-14
- **Problem:** Want to install third-party Omarchy plugins (bar widgets,
  services, panels) from the official plugin marketplace and track them here.
  The old `omarchy plugin install <id>` command is gone — plugin discovery and
  installation changed.
- **Fix:**
  - Marketplace: [plugins.omarchy.org](https://plugins.omarchy.org/) (official)
    plus the community directory [omarchyplugins.com](https://omarchyplugins.com).
    Browse, inspect the source, and copy each install command.
  - Install: plugins are distributed as **git repos** with a `manifest.json` at
    the root. Installing clones them into `~/.config/omarchy/plugins/`:
    ```bash
    omarchy plugin add <git-url> --enable --yes
    ```
    Drop `--enable` to install but leave it off; drop `--yes` to get a
    confirmation prompt (the tool refuses without `--yes` otherwise).
  - Manage with `omarchy plugin list | disable | enable | update | remove`;
    the Setup → Plugins UI offers the same.
  - After `add --enable`, restart the shell to load the widget into the bar:
    `omarchy restart shell`.

## Installed plugins

| Plugins | ID | Purpose | Source | Install |
| ------ | --- | ------- | ------ | ------- |
| Keystroke | `evindor.keystroke` | Raycast-style command palette that **replaces the Omarchy menu** (`omarchy.clonedFrom: "omarchy.menu"`): type or speak apps, any Omarchy menu command, hotkeys, math, conversions, emoji, clipboard history, files, Codex hand-off; local smart-match embedding model. `Super+Space`, every `omarchy-menu` binding, `omarchy menu …` and the menu pickers all route to it. Menu + bar-widget kinds; its bar button replaces the stock menu button (first on the left). Disabling/removing (`omarchy plugin disable/remove evindor.keystroke`) restores the stock menu. | [evindor/keystroke](https://github.com/evindor/keystroke) | `omarchy plugin add https://github.com/evindor/keystroke.git --enable --yes` |
| Onote | `io.github.lolu13.onote` | Sticky notes as ordinary tiled Hyprland windows, drawn by `omarchy-shell` itself (no browser engine/separate app): SQLite store owned by a small local Rust helper (`~/.local/bin/onote-helper`, built from source), notes tile/float/resize like normal windows, closing puts a note in the stack (nothing deleted), tabs, pinned notes, full-text search, optional one-way Markdown mirror (Obsidian). Service + overlay + bar-widget kinds; bar button in the right section. Adds five `Super` bindings (see the Onote section) via `~/.config/hypr/onote.lua` + one `dofile` in `bindings.lua`. | [lolu13/onote](https://github.com/lolu13/onote) | `omarchy plugin add https://github.com/lolu13/onote.git --enable --yes` then `cargo build --release --manifest-path helper/Cargo.toml` and `python3 scripts/install.py` (needs a Rust toolchain; see the Onote section) |
| Mouseless | `wkuehler.mouseless` | Keyboard-driven pointer (warpd-style): press a modifier, the screen fills with a lettered hint grid, type three letters to warp the pointer and click; supports right/middle/double click, move-only, and scroll mode. Overlay kind (no bar widget — summoned by a keybind). Added `SUPER+ALT+M` binding (SUPER+M is taken by Days). | [wkuehler/mouseless](https://github.com/wkuehler/mouseless) | `omarchy plugin add https://github.com/wkuehler/mouseless.git --enable --yes` then add the trigger binding (see the Mouseless section) |
| YouTube Float | `io.github.jcputney.media-float-youtube` | Pick up where you left off on YouTube: Watch Later, history, subscriptions, playlists, channels, and search — then play the result in a small **floating, pinned mpv window** that follows you across workspaces. Overlay kind (no bar widget); `youtube-float` CLI + a launcher entry ("YouTube Float"); optional cookies via `youtube-float auth` for subscriptions/history (search works signed out). Requires mpv + yt-dlp (already present). Added four `SUPER+ALT` bindings (see the YouTube Float section); floats/window rules in `~/.config/hypr/media-float.lua`. | [jcputney/omarchy-media-float-youtube](https://github.com/jcputney/omarchy-media-float-youtube) | `omarchy plugin add https://github.com/jcputney/omarchy-media-float-youtube.git --enable --yes` then `<plugin-dir>/setup` and `omarchy restart shell` |
| FossFetch | `davedes.fossfetch` | Package search + one-click install across three ecosystems in one bar panel: **Pacman** (`pacman -Ss`), **AUR** (RPC search), and **Flatpak** (Flathub AppStream catalog) — all live, no stale curated lists. Natural-language category browsing ("video editing", "browser", "chat") by matching AppStream categories. Bar-widget kind; auto-placed in the right section. Configurable panel width and search debounce (see its settings in the bar widget settings). | [Davedes83/fossfetch](https://github.com/Davedes83/fossfetch) | `omarchy plugin add https://github.com/Davedes83/fossfetch.git --enable --yes` |

| Plugins | ID | Purpose | Source | Install |
| ------ | --- | ------- | ------ | ------- |
| Screens | `im0001gt.screens` | Monitor layout — drag-to-arrange, scale, HDR/VRR, saved profiles. Takes over `~/.config/hypr/monitors.lua` on first run (see [fix 001](001-multi-monitor-screens-plugin.md)). | [IM0001GT/omarchy-screens](https://github.com/IM0001GT/omarchy-screens) | `omarchy plugin add https://github.com/IM0001GT/omarchy-screens --enable --yes` |
| oShelf | `io.github.i12bp8.oshelf` | A temporary desktop-edge "shelf" — park files, images, links, and text, then pick them up in another app/window. Service kind, no bar widget. | [i12bp8/oShelf](https://github.com/i12bp8/oShelf) | `omarchy plugin add https://github.com/i12bp8/oShelf.git --enable --yes` |
| Bar Screens | `io.github.jondkinney.barscreens` | Clone of the stock bar with per-monitor show/hide toggles — runs Omarchy's own bar untouched and parks it off-screen on unticked monitors, so updates can't break it and the stock bar's fixes keep arriving. Replaces the bar when enabled (set in Settings › Bar). | [jondkinney/omarchy-barscreens](https://github.com/jondkinney/omarchy-barscreens) | `omarchy plugin add https://github.com/jondkinney/omarchy-barscreens.git --enable --yes` |
| OmaGlass | `io.github.cjohnson46.omaglass` | Live network traffic monitor — speed gauges, history graph (30s–30m windows), active connections with country flags + on-demand whois, per-app/host/type/country usage, LAN device discovery. Read-only (no firewall/blocking). Auto-placed in the bar's right section on enable. | [cjohnson46/omarchy-omaglass](https://github.com/cjohnson46/omarchy-omaglass) | `omarchy plugin add https://github.com/cjohnson46/omarchy-omaglass.git --enable --yes` |
| Hardware Monitor | `io.github.grootaiinfinity.hwmon` | `CPU%` + package temp readout in the bar (right-click/scroll expands to MEM/GPU/BAT%); left-click opens a full system panel: CPU cores, memory/swap, thermals + fans, per-GPU meters, storage/disks, network, top processes, battery. Reads sysfs/procfs only — no privileges or network. Auto-placed in the bar's right section on enable. | [GrootAiInfinity/omarchy-hwmon](https://github.com/GrootAiInfinity/omarchy-hwmon) | `omarchy plugin add https://github.com/GrootAiInfinity/omarchy-hwmon.git --enable --yes` |
| Netspeed | `vm.netspeed` | Live download/upload speed in the bar (`↓ x ↑ y`), fixed-width from `/sys` interface counters so the bar never shifts. Hover → tooltip with interface + both rates, left/middle click → immediate refresh, right-click → toggle upload readout. Auto-placed in the bar's right section on enable; settings `refreshSeconds` (1–5, default 1) and `showUpload`. | [jhonoryza/omarchy-netspeed](https://github.com/jhonoryza/omarchy-netspeed) | `omarchy plugin add https://github.com/jhonoryza/omarchy-netspeed.git --enable --yes` |
| Wallpaper Align | `wallpaper-align` | Clone of stock `omarchy.background` (which is disabled) plus a bar widget: set image/color per screen or span the whole layout, fill/fit/stretch + edge align. **Patched here** for a `span` mode — see [Wallpaper Align + span mode](#wallpaper-align--span-mode-patched). Auto-placed in the bar's right section on enable. | [Primly/omarchy-wallpaper](https://github.com/Primly/omarchy-wallpaper) | `omarchy plugin add https://github.com/Primly/omarchy-wallpaper --enable --yes` then apply `config/patches/wallpaper-align.span.patch` (done by `config/restore.sh` step 7) |
| Days | `leonrlr4.days` | Per-day task list overlay: tasks belong to the day they're written, Markdown notes, paste screenshots (via `SUPER + V`), subtasks, and the day's calendar events beside them. Overlay kind, no bar widget; data is local JSON in `~/.local/share/leonrlr4.days/`. | [leonrlr4/days](https://github.com/leonrlr4/days) | `omarchy plugin add https://github.com/leonrlr4/days.git --enable --yes` then `~/.config/omarchy/plugins/leonrlr4.days/scripts/setup` (binds `SUPER + M`; idempotent, `--check` reports missing bits) |
| Omarchy Google Calendar and Clock | `omarchy-google-calendar-clock` | Bar clock + local-first calendar. Used here **only as the read-only event bridge for Days** (Days calls `scripts/calendar-events <from> <to>`). Runs the [Caldir](https://github.com/t4t5/caldir) runtime over local `.ics` folders; Google sync is optional and is **not** configured on this machine. **Patched here** to add local todo/reminder/note buttons and a quieter month grid — see [Local todo/reminder/note patch](#omarchy-google-calendar-and-clock--local-todoremindernote-patch). | [NachoRodriguezM/omarchy-google-calendar-clock](https://github.com/NachoRodriguezM/omarchy-google-calendar-clock) | `omarchy plugin add https://github.com/NachoRodriguezM/omarchy-google-calendar-clock --enable --yes` then `scripts/setup --binaries-only` (installs Caldir without Google OAuth), then apply `config/patches/omarchy-google-calendar-clock.local-items.patch` (done by `config/restore.sh` step 9) |
| OmiHaze | `nasrullaameen.omihaze` | Dims inactive windows (macOS HazeOver-style) so the focused window stays visually dominant — auto-follows focus, live intensity slider, presets, scope, per-app exclusions (reads `hyprctl -j clients`; e.g. spares scratchpad if `excludeSpecialWorkspace`). Bar-widget kind. Auto-placed in the bar's **center** section on enable. | [NasrullaAmeen/omihaze](https://github.com/NasrullaAmeen/omihaze) | `omarchy plugin add https://github.com/NasrullaAmeen/omihaze.git --enable --yes` |
| Paper Mode | `io.github.prathamesh913.paper-mode` | Screen-wide paper/e-ink display modes via Hyprland's native `screen_shader` — grayscale, warm "paper", high-contrast "e-ink", one-click toggle in the bar (left-click toggles, right-click picks a mode). Service + bar-widget kinds. **Patched here** so the bar widget reaches its service over IPC when hosted by a replacement bar — see [Paper Mode (patched for replacement bars)](#paper-mode-patched-for-replacement-bars). | [Prathamesh913/paper-mode](https://github.com/Prathamesh913/paper-mode) | `omarchy plugin add https://github.com/Prathamesh913/paper-mode.git --enable --yes` then apply `config/patches/paper-mode.hostipc.patch` (done by `config/restore.sh` step 7c) |

| OmaSpotify | `io.github.jeremylanger.omaspotify` | Spotify-replacement in Quickshell — full client (search, browse, library, playlists, queue, stats, equalizer, lyrics), local receiver daemon for audio (**Spotify Premium required for playback**; free accounts can still browse/manage their library), about 60 MB RAM instead of ~950 MB. The receiver is a local librespot-based Rust backend (`scripts/setup.sh` builds it if no verified release is available). Service + bar-widget + panel kinds. Under the **Bar Screens** replacement bar its bar widget runs **icon-only** (no track text / in-bar controls — same `serviceFor` limitation as Paper Mode); the full player opens from the icon and is unaffected. Auto-placed in the bar's left section on enable. | [jeremylanger/omaspotify](https://github.com/jeremylanger/omaspotify) | `omarchy plugin add https://github.com/jeremylanger/omaspotify.git --enable --yes` then `scripts/setup.sh` (builds backend via Rust — see the OmaSpotify section) |

## Bar: position and arrangement (this machine)

Current state of `~/.config/omarchy/shell.json` (top bar, non-transparent,
`centerAnchor: "omarchy-google-calendar-clock"`, whole bar replaced by the
**Bar Screens** plugin: top-level `"id": "io.github.jondkinney.barscreens"`).

Left → right order within the bar's three sections:

**left** (in order):
1. `evindor.keystroke` — Keystroke command palette (took the stock
   `omarchy.menu`'s place: it routes every `omarchy.menu` / menu-bar call)
2. `omarchy.workspaces` — per-monitor workspace groups (eDP-2 → IDs 6–8,
   DP-4 → 0,9, DP-5 → 1–5; eDP-2 group uses `bright_green` colorKey)
3. `io.github.jeremylanger.omaspotify` — OmaSpotify (icon-only under Bar Screens;
   click opens the full player)

**center** (the `centerAnchor` drives which widget hugs dead-center):
1. `omarchy.indicators`
2. `omarchy.keyboard-layout`
3. `omarchy.weather`
4. `nasrullaameen.omihaze` (OmiHaze — dims inactive windows)
5. `omarchy-google-calendar-clock` — the clock (center anchor; stock
   `omarchy.clock` removed)
6. `omarchy.system-update`

**right** (in order):
1. `omarchy.tray`
2. `davedes.fossfetch` (FossFetch — package search; auto-placed here on enable)
3. `io.github.lolu13.onote` (Onote — sticky notes; middle-click = new note,
   right-click = stack all)
4. `io.github.prathamesh913.paper-mode` (Paper Mode — screen shader toggle;
   auto-placed here on enable)
5. `wallpaper-align` (Wallpaper Align — image → bar widget)
6. `vm.netspeed` (Netspeed)
7. `io.github.grootaiinfinity.hwmon` (Hardware Monitor)
8. `io.github.cjohnson46.omaglass` (OmaGlass)
9. `im0001gt.screens` (Screens)
10. `omarchy.agents`
9. `omarchy.bluetooth`
10. `omarchy.network`
11. `omarchy.audio`
12. `omarchy.monitor`
13. `omarchy.power`
14. `io.github.jondkinney.barscreens` (Bar Screens' own widget — the
    right-edge peek/toggle)

**Not on the bar** (enabled as service/overlay; no bar slot):
- `io.github.i12bp8.oshelf` → `plugins` array in `shell.json`
- `leonrlr4.days` → `plugins` array; opened via `SUPER + M`, not a widget

"position and arrangement" = `omarchy bar put <id> <left|center|right>` moves
a widget between section, and reordering is done by editing the array order in
`shell.json` (then `omarchy restart shell`).

## Wallpaper Align + span mode (patched)

[Wallpaper Align](https://github.com/Primly/omarchy-wallpaper) (`wallpaper-align`)
replaces stock `omarchy.background` and adds a bar widget. Stock behaviour is
**per-screen** framing (fill/fit/stretch + edge align). To get **one wallpaper
spanning across all monitors** (a single canvas over the whole arrangement), the
plugin was patched — both files, re-applied after any `omarchy plugin update`:

- `Background.qml` — `monitorLayout()` bounding-box helper plus per-screen
  slice rendering: `mode: "span"` maps one image onto the union layout and each
  output draws the region matching its position/size (clipped, logical coords,
  mixed-DPR safe). New col in the bar widget's **Scale** row: **Span** (clears
  per-screen overrides, `monitors: {}`).
- `Framing.qml` — accepts/serves the `span` mode, adds the **Span** option to
  the Scale `ButtonGroup`.

Persistent patch: `config/patches/wallpaper-align.span.patch`.

- Config file: `~/.config/omarchy/background-framing.json`
  (`{"mode":"span","halign":"center","valign":"center","monitors":{}}`;
  `halign`/`valign` pick which part of the image survives when its aspect
  doesn't match the canvas).
- Toggle from the root menu **Style › Wallpaper: span all screens**:
  - script `~/.local/bin/omarchy-wallpaper-span-toggle` (`--check`/`--status`;
    off↔on keeps a `.prev.json` sidecar to restore the prior arrangement)
  - menu entry `style.wallpaper-span` in
    `~/.config/omarchy/extensions/omarchy-menu.jsonc` with a `checked` guard
    (`✓` shown while span is active). Hot-reloaded, no shell restart needed.

Monitor layout spanned (logical): DP-5 ultrawide 0,0 3440×1440 · eDP-2
1016,1440 1600×1000 (@1.6) · DP-4 3440,437 2560×1440 → canvas 6000×2440.
Span is **confirmed working live** on all three outputs (single image, no seam,
per-screen slice matches layout position). For a canvas this big (6000×2440) a
stock wallpaper is too low-res, so the source was **AI-upscaled with
[Upscayl](https://upscayl.org/)**: 1920×1080 → **9600×5400** (Ultrasharp
model) → current file `wallhaven-4xr2l3_upscayl_5x_ultrasharp-4x.png` (crisp
even on the per-screen crop of the 6000×2440 canvas).

### Restore on a fresh install (script + files)

`config/restore.sh` **step 7** re-creates all of the above. Manually, the steps
are:

```bash
# 1. Plugin + span patch (plugin update reverts the patch — redo this after any
#    `omarchy plugin update`)
omarchy plugin add https://github.com/Primly/omarchy-wallpaper --enable --yes
git -C ~/.config/omarchy/plugins/wallpaper-align \
    apply /home/darko/Projects/Omarchy-setup-2026/config/patches/wallpaper-align.span.patch
omarchy restart shell

# 2. Toggle script (on PATH, used by the menu entry)
install -m 755 ~/Projects/Omarchy-setup-2026/config/omarchy/omarchy-wallpaper-span-toggle \
    ~/.local/bin/omarchy-wallpaper-span-toggle

# 3. Framing config (span active by default) + menu entry (only first time)
cp ~/Projects/Omarchy-setup-2026/config/omarchy/background-framing.json \
   ~/.config/omarchy/background-framing.json
cp ~/Projects/Omarchy-setup-2026/config/omarchy/extensions/omarchy-menu.jsonc \
   ~/.config/omarchy/extensions/omarchy-menu.jsonc
```

The toggle script (`config/omarchy/omarchy-wallpaper-span-toggle`, byte-for-byte
what lives at `~/.local/bin/`):

```bash
#!/usr/bin/env bash
set -euo pipefail

CFG="$HOME/.config/omarchy/background-framing.json"
PREV="$HOME/.local/state/omarchy/background-framing.prev.json"

is_span() {
  [[ -f "$CFG" ]] && jq -e '.mode == "span"' "$CFG" >/dev/null 2>&1
}

case "${1:-}" in
  --check)
    is_span
    ;;
  --status)
    is_span && printf 'span\n' || printf 'other\n'
    ;;
  *)
    if is_span; then
      if [[ -f "$PREV" ]]; then
        cp "$PREV" "$CFG"
        rm -f "$PREV"
      else
        printf '{"mode":"fill","halign":"center","valign":"center","monitors":{}}\n' > "$CFG"
      fi
    else
      cp "$CFG" "$PREV"
      printf '{"mode":"span","halign":"center","valign":"center","monitors":{}}\n' > "$CFG"
    fi
    ;;
esac
```

Framing config (`~/.config/omarchy/background-framing.json`):

```json
{ "mode": "span", "halign": "center", "valign": "center", "monitors": {} }
```

Menu entry (`~/.config/omarchy/extensions/omarchy-menu.jsonc` — dotted id
`style.wallpaper-span` nests it under **Style**; hot-reloaded, no restart):

```jsonc
"style.wallpaper-span": {
  "icon": "󰖟",
  "label": "Wallpaper: span all screens",
  "description": "Draw one wallpaper across the whole monitor arrangement",
  "action": "omarchy-wallpaper-span-toggle",
  "checked": "omarchy-wallpaper-span-toggle --check"
}
```

Regenerate the patch after editing the plugin:

```bash
git -C ~/.config/omarchy/plugins/wallpaper-align diff \
    > ~/Projects/Omarchy-setup-2026/config/patches/wallpaper-align.span.patch
```

## Paper Mode (patched for replacement bars)

[Paper Mode](https://github.com/Prathamesh913/paper-mode)
(`io.github.prathamesh913.paper-mode`) applies screen-wide paper/e-ink display
modes with Hyprland's native `decoration:screen_shader` — **Grayscale**,
**Paper** (warm cream tint), **E-Ink** (high-contrast dithered), or **Normal**,
toggled from a bar widget (left-click toggles the last mode, right-click opens
a preset menu). Service + bar-widget kinds; `omarchy plugin add
https://github.com/Prathamesh913/paper-mode.git --enable --yes` auto-places the
widget in the bar's right section. IPC: `omarchy-shell paper-mode
{status,toggle,enablePreset,disable,togglePreset}` (see below for why
`setPreset` is not part of it).

**The problem:** the stock widget reads its sibling service directly through the
injected shell — `bar.shell.serviceFor("io.github.prathamesh913.paper-mode")`.
Only the first-party omarchy bar mints a service-capable widget shell. Under
the **Bar Screens** replacement bar the widget gets a *service-less* entry
facade, so `serviceFor` always returns `null` and the icon sits in a
"Service unavailable" state — even though the service itself is up (its IPC
target responds). This box hits any widget that depends on a sibling service
while hosted by a replacement bar.

**The fix:** `PaperModeWidget.qml` now falls back to the plugin's own IPC when
the injected service is absent. When a service shell *is* provided (stock bar),
the original live `serviceFor` path is still used, so upstream behaviour is
untouched where it worked. In IPC mode the widget drives the exact same
target/`omarchy-shell` calls (`status`, `toggle`, `disable`, `setPreset`) the
service exposes, so GUI and CLI state always agree.

Persistent patch: `config/patches/paper-mode.hostipc.patch`
(it applies to the plugin's git checkout; a `omarchy plugin update` reverts it —
re-apply with `git -C ~/.config/omarchy/plugins/io.github.prathamesh913.paper-mode
apply .../paper-mode.hostipc.patch`, done by `config/restore.sh` step 7c).

The stock menu sends `setPreset <key>`, but the plugin's `IpcHandler` only
exposes `setPreset` as an in-process method — `omarchy-shell paper-mode
setPreset grayscale` answers **"Function not found"**. So presets were only
reachable via left-click `toggle` (which flips to `lastPreset`, defaulting to
eink); the menu's Grayscale/Paper/E-Ink rows did nothing. The patch sends
`enablePreset <key>` instead, which is the exposed equivalent (`{"enabled":…,
"preset":…}`), so every preset is selectable from the menu.

Regenerate the patch after editing the plugin:

```bash
git -C ~/.config/omarchy/plugins/io.github.prathamesh913.paper-mode diff \
    > ~/Projects/Omarchy-setup-2026/config/patches/paper-mode.hostipc.patch
```

Verified live: IPC `enablePreset eink` sets `decoration:screen_shader` to the
plugin's `eink.glsl` and `status` agrees; `disable` clears it back to empty.

## OmaSpotify (replacement bar: bar widget is icon-only)

[OmaSpotify](https://github.com/jeremylanger/omaspotify)
(`io.github.jeremylanger.omaspotify`) is a full Spotify client built in
Quickshell (Omarchy's own UI toolkit): search, browse, library, playlists,
queue, stats, equalizer, and lyrics, plus a local receiver that plays audio on
this computer — about 60 MB of RAM versus ~950 MB for the official client.
Service + bar-widget + panel kinds; `omarchy plugin add
https://github.com/jeremylanger/omaspotify.git --enable --yes` auto-places the
widget in the bar's left section.

**Spotify Premium required.** Local playback is served by librespot, which
refuses free-tier accounts (`librespot does not support "free" accounts`).
Browsing, search, and library management work without Premium; only streaming
audio on this computer needs it. This exits with status 1 and the
`omaspotify.service` unit goes `start-limit-hit` — see the unit journal to
confirm.

**How the pieces connect:** the bar widget is a thin status strip that binds
30+ live properties of the sibling service (`title`, `artist`, `playing`,
`hasMedia`, `daemon.running`, `accountConnected`, …). It reads the service the
same way Paper Mode's widget did — `bar.shell.serviceFor(
"io.github.jeremylanger.omaspotify")` (BarWidget.qml:15) — so under the **Bar
Screens** replacement bar the injected facade is service-less and `spotify` is
`null`. The widget then degrades to **icon-only** (`iconOnly` is true when
`!spotify`): no track title/artist, no in-bar controls.

**Why no patch here:** unlike Paper Mode (a single shader method), this service
exposes ~100 IPC procedures and the widget live-binds dozens of properties; a
`config/patches`-style IPC fallback would be a large, lossy rewrite. Left-click
on the icon summons the **full player** panel, which is served by the shell
itself (not the bar facade), so the complete client works normally. The only
loss is the compact track text in the bar.

**First-time setup** (from the plugin's own README):
1. Click the Spotify icon in the bar → **Set up and continue** (or **Continue
   with Spotify** if playback is already installed).
2. Sign in on Spotify's own page in the browser and approve access.
3. Complete the separate playback authorization so audio plays on this computer;
   if prompted choose **Finish playback setup**.

**Playback backend** (local audio receiver — the separate step that actually
streams): the plugin tries a verified release download first; if provenance
cannot be verified, it falls back to a local Rust build. On this machine the
verified release was unavailable, so the build was done with a user-local
rustup install (`curl https://sh.rustup.rs -sSf | sh -s -- -y --no-modify-path
--profile minimal` — no sudo needed, installs to `~/.cargo` + `~/.rustup`).
`~/.cargo/bin/cargo` was then added to PATH so `scripts/setup.sh` can find it.
The first build takes ~1 minute; the resulting binary is at
`~/.local/lib/omaspotify/omaspotify-backend` with its unit
(`omaspotify.service`, static, started on demand — never enabled at login).

Optional (only if you want it): replace Omarchy's stock `SUPER + SHIFT + M ·
Music` binding by adding to `~/.config/hypr/bindings.lua`:

```lua
  hl.unbind("SUPER + SHIFT + M") -- previously: Music
  o.bind("SUPER + SHIFT + M", "OmaSpotify",
    "omarchy shell -q io.github.jeremylanger.omaspotify.player togglePlayer")
```

then `hyprctl reload` and check `hyprctl configerrors`. In the player's
Settings you can then pick whether that shortcut opens Omarchy's Music app, the
full player, or the mini-player; `omarchy shell -q
io.github.jeremylanger.omaspotify.player {volumeUp,volumeDown}` raises/lowers
Spotify's own volume 5% per call.

Verified live: plugin loads with zero QML errors; service IPC target
`io.github.jeremylanger.omaspotify.player` is up; the playback backend built
and its unit installed, and the Connect-device credentials were stored
(`~/.local/state/omaspotify/{oauth,zeroconf}/credentials.json`). The backend
authenticated to Spotify's AP for a logged-in account — then stopped for
"free" accounts (the Premium gate above), so playback is not tested end-to-end
on this machine.

## Keystroke (replaces the Omarchy menu)

[Keystroke](https://github.com/evindor/keystroke) (`evindor.keystroke`) is a
Raycast-style command palette that **replaces the Omarchy menu**. It is one
native `menu`-kind plugin (QML + JS) running in the existing `omarchy-shell`
process, themed by the active Omarchy theme, with a small local embedding
model for "Smart Match" fuzzy understanding.

**How it replaces the menu:** the manifest declares
`omarchy: { clonedFrom: "omarchy.menu" }`, so Omarchy's plugin registry routes
every `omarchy.menu` call to the enabled replacement — `Super+Space`, all
`omarchy-menu` bindings, `omarchy menu summon <route>`, pickers
(`omarchy-menu-select` / `omarchy-menu-input`), and the menu button in the bar —
and restores the stock menu when the plugin is disabled or removed. Its bar
button sits where the stock menu button was (first, left section).

**What it can do** (pulled from its own README): fuzzy app + command search,
the entire Omarchy menu as rows, every Omarchy hotkey (run or learn the keys),
math, unit/conversion, colors, emoji, clipboard history, files under `~`
(fuzzy‑abbreviate path fragments), and hand-offs to Claude/Codex/browser with
your prompt in the composer — typed or spoken. Voice dictation and the Codex
integration need their per-feature setup; community **extensions** live in
`extensions/` and are all **off until enabled** (an off extension is never
even compiled). State (frecency, preferences) is stored as hashed ids only in
`~/.local/state/keystroke/usage.json`.

Verified live: plugin enabled with zero QML errors; the stock `omarchy.menu`
button is replaced in the bar (left section, index 0); `Super+Space` (any
route that invokes `omarchy.menu`) now opens the Keystroke palette.

## Onote (sticky notes as Hyprland windows)

[Onote](https://github.com/lolu13/onote) (`io.github.lolu13.onote`, formerly the
Omarchy edition of DeskNotes): sticky notes that are **ordinary Hyprland
windows** — they tile, float, move and resize like any other window — but are
drawn by `omarchy-shell` itself (no browser engine, no app process). A small
Rust helper (`onote-helper`) owns the SQLite store and is the only extra
process. Service + overlay + bar-widget kinds; the bar button sits in the
right section (left-click notes & stack, middle-click new note, right-click
stack all).

**Setup on this machine** (the helper is built from source, nothing is
downloaded):

```bash
cd ~/.config/omarchy/plugins/io.github.lolu13.onote
cargo build --release --manifest-path helper/Cargo.toml   # ~24 s
python3 scripts/install.py
```

`scripts/install.py` checks `Super+N` / `Super+Alt+N` / `Super+Alt+H` are free
(they were — only `Super+M` is taken by Days), writes
`~/.config/hypr/onote.lua`, adds one `dofile` line at the end of
`~/.config/hypr/bindings.lua`, copies the built helper to
`~/.local/bin/onote-helper`, installs a `.desktop` launcher + icon, and
restarts the shell. `hyprctl configerrors` is clean after install. (Rust was
already present from the OmaSpotify backend build.)

**Bindings added** (all in `onote.lua`, verified conflict-free):

| Keys | Action |
| --- | --- |
| `Super+N` | Notes & Stack — full-text search, restore, confirmed delete |
| `Super+Alt+N` | New note |
| `Super+Alt+V` | New note from clipboard text |
| `Super+Alt+H` | Stack all open notes |
| `Super+Alt+P` | Pin a note on every workspace |

Also one window rule: notes match `class = ^org.quickshell$` + the
` — Onote [dn:…]` title suffix and get `tile = true` (never match the class
alone — other shell windows share it).

Verified live: helper built and running (child of the shell); bar widget on the
bar's right section; `hyprctl configerrors` clean; install backups in
`~/.local/state/onote/install-backups/`.

## Mouseless (keyboard-driven pointer)

[Mouseless](https://github.com/wkuehler/mouseless) (`wkuehler.mouseless`) is a
warpd-style pointer that never needs the mouse: press the trigger, the screen
fills with a lettered hint grid, and three keystrokes put the pointer on a
15×14 px target and click. Two letters pick a cell from a 26×26 grid, the
third picks an exact spot from a keyboard-shaped block. Arrow keys
move a highlight; action keys: `↵`/final letter = click, `Space` = move only,
`Tab` = move + scroll mode, `Shift` = double click, `Ctrl` = right click,
`Alt` = middle click. Overlay kind — no bar widget, summoned by keybind.

**Trigger on this machine:** `SUPER+M` is taken by Days (Daily tasks), so the
plugin's suggested default is not used. Added to `~/.config/hypr/bindings.lua`
(following the README's exact command):

```lua
o.bind("SUPER + ALT + M", "Mouse: hint grid", "omarchy-shell shell toggle wkuehler.mouseless")
```

`hyprctl reload` + `hyprctl configerrors` are clean. It can also be reached any
time through the Keystroke palette (type "mouse"). Counted with the OmiHaze
style: it replaces reaching for the mouse entirely.

## YouTube Float (floating mpv player)

[YouTube Float](https://github.com/jcputney/omarchy-media-float-youtube)
(`io.github.jcputney.media-float-youtube`) picks up where you left off on
YouTube: Watch Later and in-progress videos first, then subscriptions,
playlists, channels, and search. Choosing a result plays it in a small
**floating, pinned mpv window** (≈ quarter-monitor, keeps the video's real
aspect ratio) that follows you across workspaces and dims nothing. Overlay
kind (`Picker.qml`), no bar widget.

**Two half installers** (this machine, both done):
1. `omarchy plugin add https://github.com/jcputney/omarchy-media-float-youtube.git --enable --yes`
   — hands the shell the picker overlay (registers in `shell.json`).
2. `<plugin-dir>/setup` — installs the `youtube-float` CLI to
   `~/.local/bin/`, the launcher entry ("YouTube Float"), and the shared
   floating window rules in `~/.config/hypr/media-float.lua`
   (`require("hypr.media-float")` from `hyprland.lua`); verifies `mpv` and
   `yt-dlp` (both were already installed).
3. `omarchy restart shell` — the shell caches plugin QML once loaded; the
   restart is what makes a freshly installed picker appear.

`setup --check` reports what's installed; `setup --uninstall` removes
everything it wrote.

**Signed out vs signed in:** search and channel browsing work signed out;
subscriptions, Watch Later, history, and playlists need cookies — run
`youtube-float auth` once (opens a tool-only browser profile; closing the
window exports the cookies itself — no extension, no file to place).

**Keybindings added** (all free on this machine; setup leaves them to you):

| Keys | Action |
| --- | --- |
| `SUPER+ALT+Y` | YouTube: browse and play (`youtube-float browse`) |
| `SUPER+ALT+SHIFT+P` | Overlay: hide/show (`float-overlay toggle`) |
| `SUPER+ALT+CTRL+P` | Overlay: close (`float-overlay quit`) |
| `SUPER+ALT+O` | Overlay: cycle size (`float-overlay size cycle`) |

Verified live: picker registered, `youtube-float` on PATH, launcher entry
present, window rule loaded via `hyprland.lua`, `hyprctl reload` +
`configerrors` clean, shell restarted with zero QML errors.

## Days + local calendar (Caldir)

Days shows the day's calendar events above the task list **only** if the
calendar plugin above is installed. Instead of Google, this setup uses a plain
local folder of `.ics` files:

```bash
# 1. Caldir config -> local folder (file created at ~/.config/caldir/config.toml)
mkdir -p ~/.config/caldir
printf 'calendar_dir = "~/Calendar"\ntime_format = "24h"\ndefault_calendar = "personal"\n' > ~/.config/caldir/config.toml

# 2. One subfolder per calendar, each with a per-calendar config + .ics files
mkdir -p ~/Calendar/personal/.caldir
printf 'color = "#7aa2f7"\nread_only = false\n' > ~/Calendar/personal/.caldir/config.toml

# 3. Drop .ics files in ~/Calendar/personal/, or create events with caldir:
~/.config/omarchy/plugins/omarchy-google-calendar-clock/scripts/calendar-caldir \
  new "Standup" -s "tomorrow 9:30" -d 30m --reminder 10m
```

- Layout: `~/Calendar/<slug>/` is a calendar; `~/.config/caldir/config.toml`
  points `calendar_dir` at it and sets `default_calendar`. `color` is
  `#RRGGBB` (shown in Days), `read_only` allows local edits.
- Events are plain iCalendar; files created by `caldir new` are named
  `YYYY-MM-DDTHHMM__<slug>.ics`:
  - **All-day notes** (no time) use `DTSTART;VALUE=DATE` and are named
    `YYYY-MM-DD__<slug>.ics`.
  - **Notes with a time** use `DTSTART;TZID=<zone>` (here
    `Indian/Maldives`) — e.g. today's `2026-09-14__test` → `test` at
    11:00–12:00 (`all_day:false` in the bridge).
  - **Recurring** via `RRULE:FREQ=YEARLY` (my birthday Aug 31, anniversary
    May 28) — the bridge resolves each occurrence to the right day
    (`recurring:true`, `recurrence_id` set) so Days shows them on the right
    dates.
- Bridge check:
  `~/.config/omarchy/plugins/omarchy-google-calendar-clock/scripts/calendar-events 2026-09-14 2026-09-20`
  prints `{"ok":true,"events":[...]}`.
- Days caches one file per day in `~/.local/share/leonrlr4.days/events/YYYY-MM-DD.json`
  and refetches a couple of seconds after an overlay opens, so a new `.ics` may
  take a moment to appear.
- **Google sync is intentionally off** (`setup --binaries-only`), so the clock
  widget stays a plain clock — fine, Days is the only consumer.
- **Calendar auto-reminders:** reminders are scheduled at note-add time by the
  path watcher (and re-checked by the 5-min timer). A reminder set for a note
  that's then deleted keeps firing — cancel it with `omarchy-reminder clear`.
  Editing a note reschedules cleanly (the stale systemd timer is stopped).

### Auto-reminders for timed notes

A timed note (an event **with a time**, e.g. `calendar-caldir new "Standup" -s
"11:40" -d 30m`) now **automatically becomes an Omarchy reminder** that fires
at the event's start — adding a timed note is enough, no separate reminder
needed. All-day notes (no time) are skipped.

- **Trigger:** `omarchy-calendar-remind.path` (systemd user unit) watches
  `~/Calendar` and each calendar subfolder, so creating a timed `.ics` runs
  `omarchy-calendar-remind.service` instantly. `omarchy-calendar-remind.timer`
  re-scans every 5 min (and right after boot) as a safety net for edits,
  events added while the machine was off, or brand-new subfolders.
- **Script:** `~/.local/bin/omarchy-calendar-remind` (mirrored at
  `config/omarchy/omarchy-calendar-remind`). It parses each `.ics`, computes
  minutes until `DTSTART` (honoring `TZID`), and schedules the reminder.
- **Same mechanism as `omarchy-reminder`:** a `systemd-run --user` transient
  timer `omarchy-reminder-<N>m-<epoch>` with the messages in
  `${XDG_RUNTIME_DIR}/omarchy-reminders/`, so these reminders appear in
  `omarchy-reminder show` and are cleared by `omarchy-reminder clear`. The
  unit name is derived deterministically from the event's start time.
- **Idempotent:** each event is fingerprinted by its `UID` in
  `~/.local/state/omarchy-calendar-remind/` — re-runs and the 5-min timer are
  no-ops. Editing a note's time/title cancels the stale reminder and schedules
  the new one; deleting the note leaves any already-set reminder intact.
- **Env overrides:** `OMARCHY_CAL_REMIND_HORIZON` caps how many minutes ahead
  to schedule (default `0` = any future event), `CALENDAR_DIR`, `STATE_DIR`.
- **Restore on a fresh install:** `config/restore.sh` **step 8** re-creates
  the script + units (regenerating a `PathChanged=` line per calendar
  subfolder, since `PathChanged=` does **not** support globs) and enables
  both units. If you later add a new calendar folder, re-run restore.sh (or
  add one `PathChanged=` line) to watch it.
- **Sound on fire:** a timed-note reminder that fires plays the freedesktop
  `complete` chime. Instead of patching the first-party `omarchy-reminder`,
  `~/.local/bin/omarchy-reminder-sound` (a systemd user service) watches
  `${XDG_RUNTIME_DIR}/omarchy-reminders/` for a `.message` file being deleted
  — that deletion *is* the reminder firing — so **every** reminder makes the
  sound, stock `omarchy reminder` and calendar auto-reminders alike. One chime
  per 2 s (bursts from `omarchy-reminder clear` can't stack). Overrides:
  `OMARCHY_REMINDER_SOUND`, `OMARCHY_REMINDER_PLAYER` (default `pw-play`),
  `OMARCHY_REMINDER_GAP_S`, `OMARCHY_REMINDER_DIR`. No first-party binary is
  edited, so `omarchy update` can't silently undo the sound. Installed by
  `config/restore.sh` **step 8b**.

## Omarchy Google Calendar and Clock — local todo/reminder/note patch

Caldir (and therefore Google Calendar, via this plugin) only knows about
**events** — no bare todo, standalone reminder, or note object exists in
either. The stock calendar popup's day toolbar was just a single `+` (add
event) plus sync icons. Patched `Panel.qml` and added `LocalItems.js` to add
three more buttons next to it:

- **`E`** — unchanged, opens the existing event form (still goes through
  Caldir/Google like before).
- **`T` / `R` / `N`** — Todo / Reminder / Note. Each opens a small inline
  field (Reminder also gets a `HH:MM` field) under the selected day's events.
  None of these touch Caldir or get pulled/pushed — they live entirely in
  their own file, `~/.local/share/omarchy-google-calendar-clock/local-items.json`
  (JSON array, one entry per item: `type`, `date`, `text`, `time` for
  reminders, `done` for todos).
- Shown under the day's Google events in `TODO` / `REMINDERS` / `NOTES`
  sections, each row with a delete button (todos also get a done-toggle).
- A reminder rides the **same** desktop-notification pipeline the plugin
  already uses for timed events (5 minutes before + at the time), reusing the
  `calendarNotified` de-dupe map so it can't double-fire across a shell
  restart.

Also restyled the month grid to match: today/selected is a border only (no
filled background except on hover), and a day's events show as up to three
small accent dots or a `+N` count badge past that, instead of one dot per
Google calendar color.

Persistent patch:
[`config/patches/omarchy-google-calendar-clock.local-items.patch`](../config/patches/omarchy-google-calendar-clock.local-items.patch)
— re-apply after any `omarchy plugin update` (done by `config/restore.sh`
**step 9**). Regenerate after editing the plugin further:

```bash
git -C ~/.config/omarchy/plugins/omarchy-google-calendar-clock diff HEAD~1 HEAD \
    > ~/Projects/Omarchy-setup-2026/config/patches/omarchy-google-calendar-clock.local-items.patch
```

## Files touched

- `~/.config/omarchy/plugins/<id>/` — git checkout of each plugin (this is the
  whole distribution mechanism; `omarchy plugin update` fast-forwards the repo)
- `~/.config/hypr/monitors.lua` — becomes plugin-managed by Screens (fix 001)
- `~/.local/state/im0001gt.screens/` — Screens backups/profiles
- `~/.local/share/caldir` / `~/.config/caldir/config.toml` — Caldir runtime
  dir + local calendar config (`calendar_dir = "~/Calendar"`)
- `~/Calendar/<slug>/` — local `.ics` calendar store (one subfolder per
  calendar, `.caldir/config.toml` carries `color`/`read_only`)
- `~/.local/share/leonrlr4.days/` — Days data: `days/YYYY-MM-DD.json`,
  `blobs/` (deduplicated screenshots), `events/YYYY-MM-DD.json` (calendar cache)
- `~/.config/hypr/bindings.lua` — Days' `SUPER + M` binding (added by its setup
  script)
- `~/.config/omarchy/shell.json` — bar layout; enabling a widget that replaces
  a built-in (e.g. workspaces) disables the built-in here
- `~/.config/omarchy/plugins/wallpaper-align/` — wallpaper plugin checkout,
  patched with span mode
- `~/.config/omarchy/plugins/nasrullaameen.omihaze/` — OmiHaze checkout
  (dims inactive windows; bar widget in the center section)
- `~/.config/omarchy/background-framing.json` — wallpaper framing config
  (currently `{"mode":"span",...}`; the span toggle's target file)
- `~/.local/state/omarchy/background-framing.prev.json` — sidecar the toggle
  script writes when switching to span, so turning it off restores the exact
  prior framing
- `~/.local/bin/omarchy-wallpaper-span-toggle` — span toggle (mirrored in
  `config/omarchy/omarchy-wallpaper-span-toggle`)
- `~/.config/omarchy/extensions/omarchy-menu.jsonc` — Style menu entry
  `style.wallpaper-span` (mirrored in
  `config/omarchy/extensions/omarchy-menu.jsonc`) — restoring these three is
  `config/restore.sh` step 7
- `~/.local/bin/omarchy-calendar-remind` — timed-note → reminder watcher
  (mirrored in `config/omarchy/omarchy-calendar-remind`) — installed by
  `config/restore.sh` step 8
- `~/.config/systemd/user/omarchy-calendar-remind.{path,service,timer}` —
  systemd user units: `.path` watches `~/Calendar` + subfolders for instant
  trigger, `.timer` re-scans every 5 min and on boot (mirrored in
  `config/systemd/user/`)
- `~/.local/state/omarchy-calendar-remind/` — per-UID fingerprint markers
  (dedupe) plus the `flock` lock
- `~/.local/bin/omarchy-reminder-sound` — chime when any reminder fires
  (mirrored in `config/omarchy/omarchy-reminder-sound`) — installed by
  `config/restore.sh` **step 8b**; requires `inotifywait` + `pw-play`
- `~/.config/systemd/user/omarchy-reminder-sound.service` — systemd user
  service running the sound watcher (mirrored in `config/systemd/user/`)
- `~/.config/omarchy/plugins/io.github.prathamesh913.paper-mode/` — Paper Mode
  plugin checkout; `PaperModeWidget.qml` **patched** with an IPC fallback for
  replacement-bar hosts (patch: `config/patches/paper-mode.hostipc.patch`,
  re-applied by `config/restore.sh` step 7c; a `omarchy plugin update` reverts
  it)
- `~/.config/omarchy/plugins/io.github.jeremylanger.omaspotify/` — OmaSpotify
  plugin checkout (full Spotify client); no patches — its bar widget is
  icon-only under Bar Screens, which is an upstream limitation, not something
  patched here. Its bar entry lives in `~/.config/omarchy/shell.json` (left
  section)
- `~/.local/lib/omaspotify/` — built `omaspotify-backend` + its source/hash
  provenance files (built locally — see the OmaSpotify section; no verified
  release was available on this machine)
- `~/.config/systemd/user/omaspotify.service` — static, on-demand playback
  backend unit (never enabled; rendered from the plugin's `systemd/` template)
- `~/.config/omaspotify/playback.conf` — playback config (device name,
  `backend = "pulseaudio"`, bitrate 320)
- `~/.local/state/omaspotify/` — account + playback sessions:
  `session.json`, `{oauth,zeroconf}/credentials.json` (playback Connect creds),
  plus `library.json`/`plays.json`/`queries.json` caches
- `~/.rustup/` + `~/.cargo/` — user-local Rust toolchain (no sudo) used to
  build the backend; rust-toolchain pin lives in the plugin checkout
- `~/.local/state/keystroke/` — Keystroke state: `usage.json` (frecency /
  preferences, hashed ids only, never query text); extensions are off until
  enabled in Settings → Extensions
- `~/.local/bin/onote-helper` — Onote's Rust helper (built from
  `~/.config/omarchy/plugins/io.github.lolu13.onote/helper/` via `cargo build
  --release`; must stay here because `omarchy plugin update` replaces the
  plugin directory)
- `~/.config/hypr/onote.lua` — Onote window rule + five bindings, sourced via
  one `dofile(...)` line at the end of `~/.config/hypr/bindings.lua`
  (validated by `hyprctl configerrors` on install; backups in
  `~/.local/state/onote/install-backups/`)
- `~/.local/share/applications/onote.desktop` + icon under
  `~/.local/share/icons/hicolor/128x128/apps/` — launcher entry installed by
  `scripts/install.py`
- `~/.local/share/com.desknotes.omarchy/desknotes.db` — notes SQLite store
  (owned by `onote-helper`; shared with earlier DeskNotes installs if any)
- `~/.config/hypr/bindings.lua` — Mouseless trigger
  `o.bind("SUPER + ALT + M", "Mouse: hint grid", "omarchy-shell shell toggle
  wkuehler.mouseless")` (added above the Onote `dofile` line; SUPER+M stayed
  with Days)
- `~/.local/bin/youtube-float` — YouTube Float CLI (browse/play/quality/resume/
  auth/toggle/quit/size); installed by the plugin's `setup`
- `~/.config/hypr/media-float.lua` — shared floating-overlay window rules for
  the media-float family (plex/twitch/youtube), `require`d from
  `~/.config/hypr/hyprland.lua`
- `~/.config/hypr/bindings.lua` — YouTube Float keys: `SUPER+ALT+Y` (browse/
  play), `SUPER+ALT+SHIFT+P` (hide/show), `SUPER+ALT+CTRL+P` (close),
  `SUPER+ALT+O` (cycle size) — see the YouTube Float section
- `~/.local/share/applications/io.github.jcputney.media-float-youtube.desktop`
  — "YouTube Float" launcher entry
- `~/.cache/youtube-float/` — cookies and watch-history cache (created by
  `youtube-float auth`/usage)
- `~/.config/omarchy/plugins/davedes.fossfetch/` — FossFetch plugin checkout
  (bar-widget-only; no patches, no external setup, no extra files)

## Caveats

- Plugins run as arbitrary, **unsandboxed** code inside the long-lived
  `omarchy-shell` process. Only install repos you trust and review the code
  first — the marketplace validates listings, not security.
- Enabling a plugin that replaces a built-in widget disables the built-in
  (observed with `air.workspaces`, since removed — removing it auto-restored
  `omarchy.workspaces`). Re-enable the built-in later with
  `omarchy plugin enable omarchy.workspaces`.
- **Bar Screens gotcha:** it doubles as the bar *option* and its widget, so the
  host gives it no per-widget shell facade — `omarchy bar put
  io.github.jondkinney.barscreens` reports "is on the bar" but does **not**
  insert the widget into `shell.json`. The icon must be added via **Settings ›
  Bar › Add widget**, or by adding `{"id": "io.github.jondkinney.barscreens"}`
  to the desired `layout` section of `~/.config/omarchy/shell.json` directly
  (done this way here — right section — then `omarchy restart shell`).
- **Wallpaper Align gotchas:** (1) the span patch lives in a cloned repo — an
  `omarchy plugin update` fast-forwards it and reverts the change; re-apply
  `config/patches/wallpaper-align.span.patch` (or just re-run `config/restore.sh`).
  (2) Clicking **Fill/Fit/Stretch** in the plugin's own panel exits span mode
  (saved as a normal per-screen mode) — turn it back on with the **Span**
  button or the **Style › Wallpaper: span all screens** menu toggle. (3) The
  menu toggle needs `jq` (Arch: `pacman -S jq`).
- **oShelf gotcha:** `omarchy plugin add` clones the source but does **not**
  build the required native Qt drag component — run
  `make -C ~/.config/omarchy/plugins/io.github.i12bp8.oshelf` once after
  install (and again after Qt/plugin updates), then restart the shell.
- **Hardware Monitor gotcha (this machine):** GPU visibility depends on data
  sources that are currently missing here.
  - The Intel iGPU **is** detected (`Intel Graphics`) but reports `util: null` /
    `temp: null` — this kernel's i915 fdinfo exports no `drm-cycles-*` counters
    (the plugin's Intel/AMD-via-DRM source) and i915 exposes no hwmon temp node.
  - The RTX 5060 is invisible to `nvidia-smi` ("No devices were found", even as
    root) despite `/proc/driver/nvidia/gpus/...` showing it initialized — the
    leftover state from the Sep 13 iGPU-only test (PCI hot-unplug + rescan
    without a reboot). A reboot re-initializes NVML; afterwards the panel
    (which queries `nvidia-smi` in `--full` mode while open) lists the NVIDIA
    GPU.