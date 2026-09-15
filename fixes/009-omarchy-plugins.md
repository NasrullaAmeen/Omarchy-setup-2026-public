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


| Plugins       | ID                                       | Purpose                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Source                                                                                          | Install                                                                                                                                                                                                                 |
| ------------- | ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Keystroke     | `evindor.keystroke`                      | Raycast-style command palette that **replaces the Omarchy menu** (`omarchy.clonedFrom: "omarchy.menu"`): type or speak apps, any Omarchy menu command, hotkeys, math, conversions, emoji, clipboard history, files, Codex hand-off; local smart-match embedding model. `Super+Space`, every `omarchy-menu` binding, `omarchy menu …` and the menu pickers all route to it. Menu + bar-widget kinds; its bar button replaces the stock menu button (first on the left). Disabling/removing (`omarchy plugin disable/remove evindor.keystroke`) restores the stock menu.                              | [evindor/keystroke](https://github.com/evindor/keystroke)                                       | `omarchy plugin add https://github.com/evindor/keystroke.git --enable --yes`                                                                                                                                            |
| Onote         | `io.github.lolu13.onote`                 | Sticky notes as ordinary tiled Hyprland windows, drawn by `omarchy-shell` itself (no browser engine/separate app): SQLite store owned by a small local Rust helper (`~/.local/bin/onote-helper`, built from source), notes tile/float/resize like normal windows, closing puts a note in the stack (nothing deleted), tabs, pinned notes, full-text search, optional one-way Markdown mirror (Obsidian). Service + overlay + bar-widget kinds; bar button in the right section. Adds five `Super` bindings (see the Onote section) via `~/.config/hypr/onote.lua` + one `dofile` in `bindings.lua`. | [lolu13/onote](https://github.com/lolu13/onote)                                                 | `omarchy plugin add https://github.com/lolu13/onote.git --enable --yes` then `cargo build --release --manifest-path helper/Cargo.toml` and `python3 scripts/install.py` (needs a Rust toolchain; see the Onote section) |
| Mouseless     | `wkuehler.mouseless`                     | Keyboard-driven pointer (warpd-style): press a modifier, the screen fills with a lettered hint grid, type three letters to warp the pointer and click; supports right/middle/double click, move-only, and scroll mode. Overlay kind (no bar widget — summoned by a keybind). Added `SUPER+ALT+M` binding (SUPER+M is taken by Days).                                                                                                                                                                                                                                                                | [wkuehler/mouseless](https://github.com/wkuehler/mouseless)                                     | `omarchy plugin add https://github.com/wkuehler/mouseless.git --enable --yes` then add the trigger binding (see the Mouseless section)                                                                                  |
| YouTube Float | `io.github.jcputney.media-float-youtube` | Pick up where you left off on YouTube: Watch Later, history, subscriptions, playlists, channels, and search — then play the result in a small **floating, pinned mpv window** that follows you across workspaces. Overlay kind (no bar widget); `youtube-float` CLI + a launcher entry ("YouTube Float"); optional cookies via `youtube-float auth` for subscriptions/history (search works signed out). Requires mpv + yt-dlp (already present). Added four `SUPER+ALT` bindings (see the YouTube Float section); floats/window rules in `~/.config/hypr/media-float.lua`.                         | [jcputney/omarchy-media-float-youtube](https://github.com/jcputney/omarchy-media-float-youtube) | `omarchy plugin add https://github.com/jcputney/omarchy-media-float-youtube.git --enable --yes` then `<plugin-dir>/setup` and `omarchy restart shell`                                                                   |
| OmaIce        | `io.github.terrifiedbug.omaice`          | Ice-style hidden section for the bar: a chevron that replaces the stock tray widget and hides every bar widget + tray icons placed to its left in the section. Right-click the chevron for the "Bar widgets" drag-reorder list, plus tray-icon pin/hide per icon, hover vs click toggle, and row vs inline reveal mode. Requires `omarchy.tray` disabled (both `--enable` + `disable` are part of the install). Configurable `rehideSeconds`, `revealOnHover`, `revealMode`.                                                                                                                        | [TerrifiedBug/omaice](https://github.com/TerrifiedBug/omaice)                                   | `omarchy plugin add https://github.com/TerrifiedBug/omaice.git --enable --yes` then `omarchy plugin disable omarchy.tray` (done; see the OmaIce section)                                                                |
| FossFetch     | `davedes.fossfetch`                      | Package search + one-click install across three ecosystems in one bar panel: **Pacman** (`pacman -Ss`), **AUR** (RPC search), and **Flatpak** (Flathub AppStream catalog) — all live, no stale curated lists. Natural-language category browsing ("video editing", "browser", "chat") by matching AppStream categories. Bar-widget kind; auto-placed in the right section. Configurable panel width and search debounce (see its settings in the bar widget settings).                                                                                                                              | [Davedes83/fossfetch](https://github.com/Davedes83/fossfetch)                                   | `omarchy plugin add https://github.com/Davedes83/fossfetch.git --enable --yes`                                                                                                                                          |
| Hyprpin       | `io.github.jondkinney.hyprpin`           | Compositor-level picture-in-picture: keep chosen windows visible across workspaces by pinning them to a corner pop-out, a tiled edge, a dedicated display, or the scratchpad. Service + bar-widget kinds; bar icon in the right section with on/off switch (right-click toggles without opening). Stock `SUPER+O`/`SUPER+T` fall through unchanged while Hyprpin is off; optional `SUPER+P` and `SUPER+ALT+S` overrides available (see the Hyprpin section).                                                                                                                                        | [jondkinney/hyprpin](https://github.com/jondkinney/hyprpin)                                     | `omarchy plugin add https://github.com/jondkinney/hyprpin.git --enable --yes`                                                                                                                                           |



| Plugins                           | ID                                   | Purpose                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Source                                                                                                            | Install                                                                                                                                                                                                                                                                                                                                                                                                       |
| --------------------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Screens                           | `im0001gt.screens`                   | Monitor layout — drag-to-arrange, scale, HDR/VRR, saved profiles. Takes over `~/.config/hypr/monitors.lua` on first run (see [fix 001](001-multi-monitor-screens-plugin.md)).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | [IM0001GT/omarchy-screens](https://github.com/IM0001GT/omarchy-screens)                                           | `omarchy plugin add https://github.com/IM0001GT/omarchy-screens --enable --yes`                                                                                                                                                                                                                                                                                                                               |
| oShelf                            | `io.github.i12bp8.oshelf`            | A temporary desktop-edge "shelf" — park files, images, links, and text, then pick them up in another app/window. Service kind, no bar widget.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | [i12bp8/oShelf](https://github.com/i12bp8/oShelf)                                                                 | `omarchy plugin add https://github.com/i12bp8/oShelf.git --enable --yes`                                                                                                                                                                                                                                                                                                                                      |
| OmaGlass                          | `io.github.cjohnson46.omaglass`      | Live network traffic monitor — speed gauges, history graph (30s–30m windows), active connections with country flags + on-demand whois, per-app/host/type/country usage, LAN device discovery. Read-only (no firewall/blocking). Auto-placed in the bar's right section on enable.                                                                                                                                                                                                                                                                                                                                                                                                                                                      | [cjohnson46/omarchy-omaglass](https://github.com/cjohnson46/omarchy-omaglass)                                     | `omarchy plugin add https://github.com/cjohnson46/omarchy-omaglass.git --enable --yes`                                                                                                                                                                                                                                                                                                                        |
| Hardware Monitor                  | `io.github.grootaiinfinity.hwmon`    | `CPU%` + package temp readout in the bar (right-click/scroll expands to MEM/GPU/BAT%); left-click opens a full system panel: CPU cores, memory/swap, thermals + fans, per-GPU meters, storage/disks, network, top processes, battery. Reads sysfs/procfs only — no privileges or network. Auto-placed in the bar's right section on enable.                                                                                                                                                                                                                                                                                                                                                                                            | [GrootAiInfinity/omarchy-hwmon](https://github.com/GrootAiInfinity/omarchy-hwmon)                                 | `omarchy plugin add https://github.com/GrootAiInfinity/omarchy-hwmon.git --enable --yes`                                                                                                                                                                                                                                                                                                                      |
| Netspeed                          | `vm.netspeed`                        | Live download/upload speed in the bar (`↓ x ↑ y`), fixed-width from `/sys` interface counters so the bar never shifts. Hover → tooltip with interface + both rates, left/middle click → immediate refresh, right-click → toggle upload readout. Auto-placed in the bar's right section on enable; settings `refreshSeconds` (1–5, default 1) and `showUpload`.                                                                                                                                                                                                                                                                                                                                                                         | [jhonoryza/omarchy-netspeed](https://github.com/jhonoryza/omarchy-netspeed)                                       | `omarchy plugin add https://github.com/jhonoryza/omarchy-netspeed.git --enable --yes`                                                                                                                                                                                                                                                                                                                         |
| Wallpaper Align                   | `wallpaper-align`                    | Clone of stock `omarchy.background` (which is disabled) plus a bar widget: set image/color per screen or span the whole layout, fill/fit/stretch + edge align. **Patched here** for a `span` mode — see [Wallpaper Align + span mode](#wallpaper-align--span-mode-patched). Auto-placed in the bar's right section on enable.                                                                                                                                                                                                                                                                                                                                                                                                          | [Primly/omarchy-wallpaper](https://github.com/Primly/omarchy-wallpaper)                                           | `omarchy plugin add https://github.com/Primly/omarchy-wallpaper --enable --yes` then apply `config/patches/wallpaper-align.span.patch` (done by `config/restore.sh` step 7)                                                                                                                                                                                                                                   |
| Days                              | `leonrlr4.days`                      | Per-day task list overlay: tasks belong to the day they're written, Markdown notes, paste screenshots (via `SUPER + V`), subtasks, and the day's calendar events beside them. Overlay kind, no bar widget; data is local JSON in `~/.local/share/leonrlr4.days/`.                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | [leonrlr4/days](https://github.com/leonrlr4/days)                                                                 | `omarchy plugin add https://github.com/leonrlr4/days.git --enable --yes` then `~/.config/omarchy/plugins/leonrlr4.days/scripts/setup` (binds `SUPER + M`; idempotent, `--check` reports missing bits)                                                                                                                                                                                                         |
| Omarchy Google Calendar and Clock | `omarchy-google-calendar-clock`      | Bar clock + local-first calendar. Used here **only as the read-only event bridge for Days** (Days calls `scripts/calendar-events <from> <to>`). Runs the [Caldir](https://github.com/t4t5/caldir) runtime over local `.ics` folders; Google sync is optional and is **not** configured on this machine. **Patched here** to add local todo/reminder/note buttons and a quieter month grid — see [Local todo/reminder/note patch](#omarchy-google-calendar-and-clock--local-todoremindernote-patch).                                                                                                                                                                                                                                    | [NachoRodriguezM/omarchy-google-calendar-clock](https://github.com/NachoRodriguezM/omarchy-google-calendar-clock) | `omarchy plugin add https://github.com/NachoRodriguezM/omarchy-google-calendar-clock --enable --yes` then `scripts/setup --binaries-only` (installs Caldir without Google OAuth), then apply `config/patches/omarchy-google-calendar-clock.local-items.patch` (done by `config/restore.sh` step 9)                                                                                                            |
| OmiHaze                           | `nasrullaameen.omihaze`              | Dims inactive windows (macOS HazeOver-style) so the focused window stays visually dominant — auto-follows focus, live intensity slider, presets, scope, per-app exclusions (reads `hyprctl -j clients`; e.g. spares scratchpad if `excludeSpecialWorkspace`). Bar-widget kind. Auto-placed in the bar's **center** section on enable.                                                                                                                                                                                                                                                                                                                                                                                                  | [NasrullaAmeen/omihaze](https://github.com/NasrullaAmeen/omihaze)                                                 | `omarchy plugin add https://github.com/NasrullaAmeen/omihaze.git --enable --yes`                                                                                                                                                                                                                                                                                                                              |
| Paper Mode                        | `io.github.prathamesh913.paper-mode` | Screen-wide paper/e-ink display modes via Hyprland's native `screen_shader` — grayscale, warm "paper", high-contrast "e-ink", one-click toggle in the bar (left-click toggles, right-click picks a mode). Service + bar-widget kinds. **Patched here** so the bar widget reaches its service over IPC when hosted by a replacement bar — see [Paper Mode (patched for replacement bars)](#paper-mode-patched-for-replacement-bars).                                                                                                                                                                                                                                                                                                    | [Prathamesh913/paper-mode](https://github.com/Prathamesh913/paper-mode)                                           | `omarchy plugin add https://github.com/Prathamesh913/paper-mode.git --enable --yes` then apply `config/patches/paper-mode.hostipc.patch` (done by `config/restore.sh` step 7c)                                                                                                                                                                                                                                |
| Oma Aux                           | `sam0110.oma-aux`                    | PipeWire patchbay for the bar: a visual routing graph of application playback, microphones, sink monitors, recording apps, hardware outputs, and filter nodes. Drag (or select + click) to connect; matches channel names (`FL`/`FR`) with mono/unnamed fallbacks. Per-route stereo processor (balance + 5-band EQ, ±12 dB). Per-application volume/mute grouped by identity (`application.id` → binary → name), persisted in `routes.json` and reapplied across pause/resume. Live stereo peak meters while the panel is open. Bar-widget kind; auto-appended to the right section on enable.                                                                                                                                         | [sam0110/oma-aux](https://github.com/sam0110/oma-aux)                                                             | `omarchy plugin add https://github.com/sam0110/oma-aux.git --enable --yes`                                                                                                                                                                                                                                                                                                                                    |
| App Workspaces                    | `io.github.zucram.app-workspaces`    | Workspace pills with **real app icons** (from installed desktop entries/icon themes) instead of bare numbers, ordered like the windows themselves (left-to-right, top-to-bottom). Separate scratchpad pills, floating-window corner markers, empty numbered workspaces hidden unless active. Click a number to switch, click an icon to focus that window, scroll to cycle, right-click for live settings (icon size/spacing, grouping, per-monitor filter, floating/scratchpad visibility). MIT fork of [Decent Workspaces](https://github.com/TheTrueFerret/omarchy-decent-workspaces). Bar-widget kind; **replaced `omarchy.workspaces` here** (now disabled) — see [App Workspaces](#app-workspaces-real-app-icons-per-workspace). | [zucram/omarchy-app-workspaces](https://github.com/zucram/omarchy-app-workspaces)                                 | `omarchy plugin add https://github.com/zucram/omarchy-app-workspaces.git --enable --yes` then `omarchy plugin disable omarchy.workspaces`                                                                                                                                                                                                                                                                     |
| Garden                            | `bjcatar.garden`                     | Private, **local-only** year-long coding-activity heatmap — a square is a half-hour with a file change in a folder you chose, or a git commit you authored (pale tile if git-only). Not GitHub.com, not a token meter. Click a day for a 48-slot strip of which repos lit each half-hour. Bar-widget kind; sprout icon in the right section.                                                                                                                                                                                                                                                                                                                                                                                           | [bjcatar/garden](https://github.com/bjcatar/garden)                                                               | `omarchy plugin add https://github.com/bjcatar/garden.git --enable`. Installed plain here (no `garden-scan --install-timer`), so it only scans on click/Refresh, not every 15 min in the background — run `python3 ~/.config/omarchy/plugins/bjcatar.garden/bin/garden-scan --install-timer` to add that.                                                                                                     |
| King                              | `keybindings.king`                   | Fullscreen, searchable, customizable overlay of your keybindings — group into categories, hide ones you'll never use, type to search or select-and-execute. Overlay kind (no bar icon).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | [danthemadnorweign/repo-keybindings-king](https://github.com/danthemadnorweign/repo-keybindings-king)             | `omarchy plugin add https://github.com/danthemadnorweign/repo-keybindings-king.git --enable --yes`. **The README's claimed `SUPER+SHIFT+K` binding is not installed by the plugin** — added manually to `~/.config/hypr/bindings.lua`: `o.bind("SUPER + SHIFT + K", "Keybindings King", "omarchy-shell shell toggle keybindings.king")` (the generic overlay-toggle IPC every `overlay`-kind plugin exposes). |
| NetNeighbors                      | `io.github.i12bp8.netneighbors`      | Fing-style Wi-Fi radar for the bar: one click lists every device on the local network — vendor, hostname, MAC — and flags new ones as they appear. Rootless: reads the kernel neighbor table after a gentle probe sweep, no daemon, no privileges. Bar-widget kind; `refreshSeconds` setting (default 30).                                                                                                                                                                                                                                                                                                                                                                                                                             | [i12bp8/omarchy-netneighbours](https://github.com/i12bp8/omarchy-netneighbours)                                   | `omarchy plugin add https://github.com/i12bp8/omarchy-netneighbours.git --enable --yes`                                                                                                                                                                                                                                                                                                                       |
| Music Dock                        | `io.github.dataknox.music-dock`      | Tray-style icon that stands in for the Spotify tray icon neither the native client nor the web app provides: appears whenever a Spotify window exists, lights up while playing, shows the track as a tooltip. Click (or `Super+Shift+M`) shows/hides Spotify in a drop-down `special:music` workspace — Hyprland's nearest thing to minimize — so playback continues hidden; middle-click play/pause, scroll to skip. Works with the native client **and** the open.spotify.com web app in Chromium-app-mode (what this machine uses, via Brave). Bar-widget kind.                                                                                                                                                                     | [DataKnox/omarchy-music-dock](https://github.com/DataKnox/omarchy-music-dock)                                     | `omarchy plugin add https://github.com/DataKnox/omarchy-music-dock.git --enable --yes` then copy `hypr/music-dock.lua` into `~/.config/hypr/`, `require("hypr.music-dock")` from `hyprland.lua`, `hyprctl reload` (see [Music Dock](#music-dock))                                                                                                                                                             |
| OmaListen                         | `io.github.hank-dev.listen`          | Lecture- and audiobook-first player: resume position, playback speed, sleep timer, playlists, bookmarks. Service + panel + bar-widget kinds; compact launcher icon in the left section. No default keybind — bound `Super+Shift+L` here (`Super+Shift+M` was already Music Dock's). First run needs **Choose music folder** pointed at a directory (defaults to `~/Music`); shows "No audio in the music folder yet" until then.                                                                                                                                                                                                                                                                                                       | [Hank-dev/omalisten](https://github.com/Hank-dev/omalisten)                                                       | `omarchy plugin add https://github.com/Hank-dev/omalisten.git --enable --yes` then add a binding, e.g. `o.bind("SUPER + SHIFT + L", "OmaListen", "omarchy-shell shell toggle io.github.hank-dev.listen '{}'")`                                                                                                                                                                                                |
| Pomodoro                          | `luisreche.pomodoro`                 | Braun-style pomodoro timer for the bar: focus → rest → long rest, a four-block set. Rests auto-start; the next focus block waits for you to commit. Bar-widget kind, no external dependencies.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | [rechedev9/omarchy-pomodoro](https://github.com/rechedev9/omarchy-pomodoro)                                       | `omarchy plugin add https://github.com/rechedev9/omarchy-pomodoro.git --enable --yes`                                                                                                                                                                                                                                                                                                                         |
| Jot                               | `jot`                                | Minimalist sticky notes living at the edge of the screen: hover the bar icon to fan the deck out, click to write, drag to reorder, instant search, archive manager. Panel + bar-widget kinds. A second, different-shaped note-taking tool alongside Onote (tiled Hyprland windows) — no conflict, just two approaches installed at once.                                                                                                                                                                                                                                                                                                                                                                                               | [JoeJoeflyn/jot](https://github.com/JoeJoeflyn/jot)                                                               | `omarchy plugin add https://github.com/JoeJoeflyn/jot.git --enable --yes`                                                                                                                                                                                                                                                                                                                                     |
| WaveBar                           | `io.github.erikburdett.wavebar`      | Now-playing bar widget with a live waveform (real PipeWire capture, not a canned animation) plus MPRIS transport controls (previous/play-pause/next), scrolling title, optional album cover. Service + bar-widget kinds. **Patched here** — see [WaveBar (patched)](#wavebar-patched).                                                                                                                                                                                                                                                                                                                                                                                                                                                 | [ErikBurdett/omarchy-wavebar](https://github.com/ErikBurdett/omarchy-wavebar)                                     | `omarchy plugin add https://github.com/ErikBurdett/omarchy-wavebar.git --enable --yes` then apply `config/patches/wavebar.hostipc-and-frames.patch` (done by `config/restore.sh` step 10)                                                                                                                                                                                                                       |
| SIA                                | `khephri.sia`                        | "The Omarchy Brain": a resident daemon (`sia-brainstem.service`) that ingests journal errors, package installs, and git commits into a local, git-versioned knowledge graph with embeddings (via **Ollama**, installed as part of setup), queryable via `sia ask "…"`/`sia status`/CLI. Bar-widget + overlay kinds; cockpit UI. **Setup is a separate, heavier step** — see [SIA setup](#sia-the-omarchy-brain-setup).                                                                                                                                                                                                                                                                                                              | [AnubisQuantumCipher/sia](https://github.com/AnubisQuantumCipher/sia)                                             | `omarchy plugin add https://github.com/AnubisQuantumCipher/sia.git --enable --yes` then `sudo pacman -S python-cryptography` (missing dep, no pip on this machine) then `./install.sh` from the plugin dir                                                                                                                                                                                                      |
| NVIDIA Hybrid                      | `nenadjokic.nvidia-hybrid`           | Read-only `nvidia-smi`-polled bar widget (util/mem/temp/clock, every 2s) for a hybrid Intel+NVIDIA laptop (this machine: RTX 5060 Max-Q + Intel Arrow Lake-S iGPU), plus a helper to pick which GPU new apps launch on. No root, no daemon. Worked immediately — drivers were already installed.                                                                                                                                                                                                                                                                                                                                                                                                                                       | [nenadjokic/omarchy-nvidia-hybrid](https://github.com/nenadjokic/omarchy-nvidia-hybrid)                           | `omarchy plugin add https://github.com/nenadjokic/omarchy-nvidia-hybrid.git --enable --yes` then `omarchy restart shell` (new bar widgets aren't picked up by hot-reload)                                                                                                                                                                                                                                       |
| USBGuard                           | `io.github.skymebr.usbguard`         | Kernel-level USB device authorization (BadUSB protection) via the `usbguard` daemon: **whitelist-based — new USB devices are blocked by default** until approved. Bar widget shows daemon/policy state. **Installed widget-only here — the setup wizard (`omarchy-setup-security-usbguard`) that actually installs the daemon and default-block policy was deliberately not run**, since misconfiguration can lock out keyboard/mouse (TTY/rescue-mode recovery). Currently shows an inert "no daemon" state.                                                                                                                                                                                                                      | [Skymebr/omarchy-usbguard](https://github.com/Skymebr/omarchy-usbguard)                                           | `omarchy plugin add https://github.com/Skymebr/omarchy-usbguard.git --enable --yes` — **do not** run `omarchy-setup-security-usbguard` without deliberately deciding to accept the lockout risk first                                                                                                                                                                                                          |
| DevTrack                           | `devtrack.streak`                    | Coding-streak tracker for the bar: LeetCode, Codeforces, and GitHub activity, a 14-week contribution heatmap in a popout panel, evening reminder notifications. Service + bar-widget kinds; polls every 15 min with local caching. Inert (shows `0d`) until usernames are set in `~/.config/omarchy/devtrack/config.json` or the in-panel settings.                                                                                                                                                                                                                                                                                                                                                                                    | [shreyasmene06/DevTrack](https://github.com/shreyasmene06/DevTrack)                                               | `omarchy plugin add https://github.com/shreyasmene06/DevTrack.git --enable --yes`                                                                                                                                                                                                                                                                                                                                |
| Wallpaper Engine for Omarchy       | `io.github.14brussell.wallpaper-engine` | Plays Steam Wallpaper Engine Workshop items (video/scene/web) via the `linux-wallpaperengine` backend, per-display, with a panel UI + `we` CLI + gum TUI. **No Steam/Workshop content on this machine** — got a real local video wallpaper working anyway by hand-building a Workshop item; see [Wallpaper Engine (patched local item)](#wallpaper-engine-for-omarchy-patched-local-item).                                                                                                                                                                                                                                                                                                                                             | [14brussell/Wallpaper-Engine-Omarchy](https://github.com/14brussell/Wallpaper-Engine-Omarchy)                     | `omarchy plugin add https://github.com/14brussell/Wallpaper-Engine-Omarchy.git --enable --yes` then `yay -S linux-wallpaperengine-git` (AUR, needs sudo) — see linked section for the rest                                                                                                                                                                                                                      |
| Restic Monitor                     | `wkuehler.restic-monitor`            | Bar widget that watches `restic-backup*.service`/`.timer` systemd units (state + journal only, never touches repo credentials) and shows backup freshness + job history. **Only monitors existing backups — doesn't create them.** Inert here: no restic backup units are configured on this machine yet.                                                                                                                                                                                                                                                                                                                                                                                                                             | [wkuehler/omarchy-restic-monitor](https://github.com/wkuehler/omarchy-restic-monitor)                             | `omarchy plugin add https://github.com/wkuehler/omarchy-restic-monitor.git --enable --yes`                                                                                                                                                                                                                                                                                                                       |


## App Workspaces (real app icons per workspace)

[App Workspaces](https://github.com/zucram/omarchy-app-workspaces)
(`io.github.zucram.app-workspaces`) replaces bare workspace numbers with the
actual icons of the windows on each workspace, ordered the way the windows are
laid out (left-to-right, then top-to-bottom) rather than by creation order.
Scratchpads get their own pills (`S` for the default one, named ones use their
name); floating windows get a small outlined corner marker; pinned windows
show once, in their reported workspace, not duplicated across pills.

**Install is two steps** — the plugin does not auto-disable whatever
workspace widget was there before, so both showed side by side until the stock
one was turned off:

```bash
omarchy plugin add https://github.com/zucram/omarchy-app-workspaces.git --enable --yes
omarchy plugin disable omarchy.workspaces
omarchy restart shell
```

It landed in the left section right after Keystroke (same slot
`omarchy.workspaces` held). Right-click any pill for **Appearance** (icon
size 12–28px, spacing, numbers on/off, icons-per-workspace cap) and
**Windows** (group same-app windows, show empty workspaces, per-monitor
filter, scratchpad/floating visibility) — settings save immediately to the
widget's entry in `~/.config/omarchy/shell.json`, no restart needed for those.

Verified live: plugin loads with zero QML errors; pills render in the left
section (e.g. workspace `3`/`0` shown); stock `omarchy.workspaces` disabled,
no duplicate indicator.

## Music Dock

[Music Dock](https://github.com/DataKnox/omarchy-music-dock)
(`io.github.dataknox.music-dock`) needed a Hyprland-side step beyond the
plugin install itself — the bar icon alone doesn't park Spotify anywhere:

```bash
cp ~/.config/omarchy/plugins/io.github.dataknox.music-dock/hypr/music-dock.lua ~/.config/hypr/
echo 'require("hypr.music-dock")' >> ~/.config/hypr/hyprland.lua
hyprctl reload
```

`music-dock.lua` declares the `special:music` drop-down workspace, a window
rule that parks Spotify there as it opens, and rebinds `Super + Shift + M`
— it calls `hl.unbind("SUPER + SHIFT + M")` first, so it cleanly replaces
Omarchy's stock **Music** binding (`{ omarchy = "spotify" }`) rather than
conflicting with it. `hyprctl configerrors` stayed clean.

**Brave quirk, self-healing:** the window rule's class regex is
`^([Ss]potify|chrome-open\.spotify\.com__-Default)$` — Chromium's app-mode
class prefix. This machine's Spotify is a Brave app-mode window
(`brave-open.spotify.com__-Default`), which that regex doesn't match, so a
freshly opened Spotify window won't auto-park on its **first** open. Not a
real gap: `bin/omarchy-music-dock`'s own class match
(`^spotify$|open\.spotify\.com`, unanchored) is broader and matches Brave
fine, and the script explicitly parks any matching window that isn't
already in the drop-down (`[[ $ws == "special:$WS" ]] || park "$addr"`) —
so the very next toggle moves it in regardless. Both the bar icon's own
detection (`isSpotify()` in `MusicDock.qml`, matching on `open.spotify.com`
as a substring) and the script's matching already handle Brave; only the
static auto-park-on-open rule is Chrome-specific.

Verified live: `bin/omarchy-music-dock toggle` twice — first call parked the
existing Brave Spotify window into `special:music` and opened the drop-down
(confirmed via `hyprctl monitors -j` → `specialWorkspace.name`), second call
retracted it. Bar icon loads with zero QML errors.

## WaveBar (patched)

[WaveBar](https://github.com/ErikBurdett/omarchy-wavebar)
(`io.github.erikburdett.wavebar`) shows a real, live waveform of whatever's
playing (captured from PipeWire via a `waveform.py` helper at 24fps) next to
MPRIS transport controls. The icon rendered, media was detected, everything
looked healthy — but the waveform itself stayed empty, and IPC `status`
showed `"lastError":"Waveform output exceeded the protocol limit"`,
`"helperRunning":false`, `"fatalHelperError":true`.

**Root cause — not the Python helper.** Running `waveform.py` directly
against the live Brave/Spotify audio for 40+ seconds produced clean,
well-formed output the whole time; the bug is in `Service.qml`'s own stdout
framing (`consumeChunk` → `MediaModel.frameChunk`). That function bounds how
much raw output it will parse from a single JS callback via
`MAX_FRAMES_PER_CHUNK = 32` (`MediaModel.js`) — if more than 32 waveform
lines (at 24fps, ~1.3s of backlog) arrive in one chunk, it rejects the
**entire stream** as a protocol violation, sets `fatalHelperError`, and
**never retries on its own** (only a track change calling
`restartVisualizer()`, or the IPC `restart` call, clears it — until it trips
again). With 20+ third-party plugins competing for the JS event loop in this
setup (Garden's watcher, NetNeighbors' probe sweep, Oma Aux's meters,
OmaSpotify's API/lyrics polling, …), stalling the event loop past 1.3s
between stdout reads is easy, so the flood guard was tripping on entirely
legitimate output.

**Fix:** raised `MAX_FRAMES_PER_CHUNK` from 32 to 256 (~10.7s of tolerable
backlog), comfortably inside the existing `MAX_RAW_CHUNK_CHARS = 4096` raw
safety cap (~680 frames at ~6 bytes each), so genuine batching from a busy
shell no longer trips it. (Folded into the same persistent patch as the two
bugs below — see the combined `config/patches/wavebar.hostipc-and-frames.patch`
mentioned there.)

Verified live: cleared the tripped state with `quickshell -p /usr/share/omarchy/shell ipc call io.github.erikburdett.wavebar restart`,
then polled `status` every 20s for 2 minutes of real playback —
`captureState` stayed `"live"` and `fatalHelperError` stayed `false`
throughout; the waveform rendered in the bar (confirmed in a screenshot).

**Second bug — bar icon invisible under Bar Screens.** After the frames fix,
the icon that should sit in the bar's left section simply never rendered (no
crash, no error — it was misidentified once as OmaGlass's unrelated network
sparkline before being properly isolated). Root cause: the same class of bug
already hit by [Paper Mode](#paper-mode-patched-for-replacement-bars) —
`BarWidget.qml` binds its entire display to `bar.shell.serviceFor(moduleName)`,
which returns `null` under the Bar Screens replacement bar (it has no
service-registry concept), so every property read off it (`waveformService.
hasMedia`, `.playing`, `.title`, …) silently evaluated against `null` and the
widget rendered nothing.

**Third bug — expanded panel stuck on "Nothing playing."** Fixing the bar
icon alone wasn't enough: clicking it opens `Panel.qml`, which receives the
same null `service` directly (`injectPanel()` sets `panelLoader.item.service
= root.waveformService`), so the panel independently showed "🎵 Nothing
playing" / "Media service is loading" with every transport button disabled,
confirmed by a live screenshot even after the bar icon was already working.

**Fix (both):** the same host-IPC-fallback pattern used for Paper Mode.
`Service.qml`'s `status()` IPC function was extended to expose everything the
UI needs (`samples`, `receivingFrames`, `identity`, `captureState`,
`lastError`, `inputRejected`, and the five MPRIS `can*` flags). `BarWidget.qml`
polls that IPC target on a 1s timer whenever `waveformService` is null and
exposes the results as `ipc*` cache properties, with every render-tree binding
and `actionEnabled()` check now reading `waveformService ? <live> : <ipc
fallback>`. `Panel.qml` — which never had its own polling — doesn't duplicate
that infrastructure; it reads straight through the already-polling bar widget
instance (`hostWidget`, always set by `injectPanel()`) via the same fallback
pattern for title/artist/identity/waveform samples/playing state and the
prev/play-pause/next buttons. Seeking, the volume slider, and the multi-source
picker were deliberately left `service`-only (matching the accepted
degradation already documented for OmaSpotify) — each already self-hides via
its own `visible: root.service && …` guard, so there was nothing extra to
patch there.

Persistent patch (now covers `BarWidget.qml`, `Panel.qml`, `Service.qml`, and
`MediaModel.js`): `config/patches/wavebar.hostipc-and-frames.patch`.

Verified live: `qmllint` clean on all four files; live Quickshell log clean
after a full `omarchy-restart-shell` (required — `keepLoaded: true` keeps the
old `Service` instance alive across a normal hot-reload); IPC `status` while
Brave was playing showed real data end-to-end
(`"hasMedia":true,"playing":true,"title":"Raining in Lofi City - lofi chill
night […]","artist":"Lofi in Cities","identity":"Brave",…,"samples":[…24
values…],"receivingFrames":true,"canTogglePlaying":true,"canPlay":true,
"canPause":true`), confirming the bar widget's IPC-fallback path is now
correctly wired all the way through to what the panel would render.

**Fourth addition — shuffle, repeat, ±10s skip, and volume, all IPC-aware.**
Requested after the panel fix above: shuffle/repeat buttons, skip-back/skip-
forward-10s buttons flanking play/pause, and making the volume slider work
under the same IPC fallback (previously it was one of the deliberately
service-only exceptions, alongside seek). `Service.qml` now reads Quickshell's
`MprisPlayer.shuffle`/`shuffleSupported`/`loopState`/`loopSupported` (already
available — `Quickshell.Services.Mpris` was already imported for the player
list) and exposes them plus `position`/`length`/`canSeek` through `status()`;
`runActionOn()` gained `"shuffle"` (toggles `player.shuffle`) and `"loop"`
(cycles `loopState` **Off → All → One → Off**, matching OmaSpotify's own
`cycleRepeat()` convention rather than the MPRIS enum's declared order of
None/Track/Playlist); a new `seekTo(position: real)` IPC function reuses the
existing `root.seekTo()`. `BarWidget.qml` and `Panel.qml` both gained the
matching `media*` fallback properties (`mediaShuffle(On|Supported)`,
`mediaLoopState`, `mediaLoopSupported`, `mediaCanSeek`, `mediaPosition`,
`mediaLength`, `mediaVolume(Supported)`), and the position/volume sliders,
the arrow-key seek handler, and the transport row were all rewired to go
through these instead of reading `player`/`service` directly — so the seek
bar and volume slider, previously accepted as service-only degradations, now
work under Bar Screens too. Icons (`󰒟` shuffle, `󰑖`/`󰑘` repeat all/one, `󰵛`/
`󰵜` skip back/forward 10s) were copied from OmaSpotify's own transport row —
same font, already confirmed to render correctly in this setup.

Whether shuffle/repeat actually do anything still depends on the underlying
MPRIS player: Brave's browser-media-session bridge reports
`shuffleSupported: false` / `loopSupported: false` (confirmed live via IPC
`status`), so those two buttons correctly show disabled for browser tabs —
this is standard MPRIS behavior, not a bug in the patch. A native player with
real shuffle/loop support (mpv with `mpv-mpris`, Spotify, VLC, …) will show
them enabled.

Verified live: `qmllint` clean on all four files; live log clean after
`omarchy-restart-shell`; IPC `status` while Brave was playing returned the
full new field set with correct semantics for a browser session
(`"shuffle":false,"shuffleSupported":false,"loopState":0,"loopSupported":false,
"canSeek":true,"position":1.326,"length":215.845,"volumeSupported":true`).
Clicking the actual buttons to see them rendered was not verified directly —
this machine has no synthetic-pointer tool (no `ydotool`/`uinput` access) to
simulate a click headlessly, so that final visual check is still pending a
manual click.

Regenerate the patch after editing the plugin:

```bash
git -C ~/.config/omarchy/plugins/io.github.erikburdett.wavebar diff \
    > ~/Projects/Omarchy-setup-2026/config/patches/wavebar.hostipc-and-frames.patch
```

## Second, independent bottom bar (tried, then reverted)

Wanted: a second bar strip at the bottom of the screen, alongside the
existing top bar. Omarchy's shell has no native support for this —
`shell.qml` holds exactly one `property var bar`, and `shell.json`'s `bar`
block is a single object (one `position`, one `layout`), not a list. Three
approaches were tried, in order of increasing safety:

**1. A floating dock plugin** (`randomchaos7800-hub/omarchy-dock`, id
`dino.dock`) — installed, then restyled from its default rounded floating
capsule into a flat, full-width, always-visible strip (patched
`Dock.qml`: `dockCard` anchored `left`/`right` instead of centered, radius
capped like the real bar's own `Math.min(Style.cornerRadius, height/2)`,
`color: Color.bar.background`, hover-magnification disabled). Worked, but
didn't match what was wanted (a real bar, not a dock) — removed
(`omarchy plugin remove dino.dock --yes`, plus its config files).

**2. Duplicating Omarchy's own `Bar.qml` a second time, inside the main
shell process.** `shell.qml`'s `configureBar()` is the only path that
creates a second full bar instance, and it unconditionally does
`shell.bar = target` — there is no way to load a second instance without
that assignment stealing the single `shell.bar` slot from the real bar.
Tried a workaround: a new local plugin (`local.bottom-bar`) listing `"bar"`
in `kinds` (to get a bar-capable scoped shell facade from
`pluginHasBarCapabilities()`) but with **no** `entryPoints.bar` (so it
shouldn't be selectable as the active bar) — instead using an
`entryPoints.panel` file that manually `Qt.createComponent()`s the real
upstream `Bar.qml`, the same technique `io.github.jondkinney.barscreens`
itself uses, but skipping its `shell.bar = created` handover.

This did not work: `omarchy plugin enable local.bottom-bar` itself treats
any manifest with `"bar"` in `kinds` as bar-selectable regardless of whether
`entryPoints.bar` exists, printed `"Now using local.bottom-bar as the bar"`,
and tore down the real bar (`hideTooltip`/`hookVariants is not a function`
cascading errors from the orphaned instance) — worse, the whole
`omarchy-shell` process exited outright rather than degrading. Recovered
immediately: `omarchy plugin enable io.github.jondkinney.barscreens` to
restore `bar.id`, `omarchy plugin remove local.bottom-bar --yes`,
`omarchy-restart-shell`, verified clean via the live log and a screenshot.
**Do not retry this without a different technique** — this is a harder wall
than a missing `entryPoints.bar` check.

**3. A second, fully independent Quickshell process** (`quickshell -c
bottom-bar`, config at `~/.config/quickshell/bottom-bar/`, autostarted via
`~/.config/hypr/autostart.lua`). This got the furthest, genuinely isolated
from the main shell (separate PID, separate log, killing it never touched
`omarchy-shell`) — and, after the user rejected a hand-reimplemented version
("this is not exact plugin"), ended up hosting the **real, unmodified**
plugin files (`BarWidget.qml`/`Panel.qml`/`Service.qml` etc., copied
verbatim from each plugin's own directory) for Hardware Monitor, OmaGlass,
OmaListen, and WaveBar, with live data and working transport, not a
reimplementation:

- `qs.Commons`/`qs.Ui` (everything these plugins import) are small,
  self-contained QML modules with their own `qmldir` declaring `module
  qs.Commons`/`module qs.Ui` — copying them into this process's own config
  root made its own `qs` namespace resolve them, independent of
  `/usr/share/omarchy/shell`.
- The `bar` property every widget/panel expects was a real
  `qs.Ui.PluginBarApi` instance — the same host-facade type Omarchy's own
  shell hands third-party widgets, already designed with safe no-op
  defaults for everything except `barForeground`/`fontFamily`/`barSize`/
  `position` and `shell.serviceFor(id)`, wired directly to
  locally-instantiated `Service.qml` copies (also portable verbatim — WaveBar
  and OmaListen's `Service.qml` files import no `qs.*` at all, only their UI
  wrapper files do).
- Hardware Monitor's entry point (`hwmon.qml`) is `Panel{}` directly rather
  than a separate BarWidget+Panel pair; loaded via `Loader { source: ... }`
  since a lowercase filename can't be a declarative QML type tag.

Three real bugs surfaced and were fixed along the way, all specific to this
synthetic-host setup, not the plugins themselves:

1. **Popups opened in the wrong place.** `PluginBarApi.position` defaults to
   `"top"`; every panel's anchor math (`KeyboardPanel.qml`'s `barPos`/
   `cardOrigin`) read that, so a popup anchored as if this bar's icons sat
   at the *top* of the screen. Fixed: `barApi.position = "bottom"`.
2. **Stuck full-screen overlay surfaces blocked all input.** Each opened
   panel spawns an `omarchy-keyboard-panel` overlay plus one
   `omarchy-keyboard-panel-dismiss` twin per *other* monitor (by design, so
   outside-click dismissal works from any screen). Opening several
   panels without each one cleanly closing stacked these up — confirmed via
   `hyprctl layers` — and since they sit on the Overlay layer, above
   everything, they silently ate every subsequent click screen-wide on every
   monitor. A clean process restart cleared it; the underlying dismiss path
   (`root.close()` in `KeyboardPanel.qml`) was separately confirmed working
   correctly once clicks were reaching the window at all.
3. **The bar icons themselves never received clicks, even on a clean
   restart.** Diagnostic timers proved the actual plugin logic was
   completely healthy — calling `runAction("playPause")` and `toggle()`
   *programmatically* (bypassing the mouse entirely) worked instantly,
   confirmed by the real Brave MPRIS session flipping to `Playing` — so the
   bug was pointer delivery, not plugin code. `hyprctl layers` showed why:
   Omarchy's own `omarchy-noty-deck`/`oshelf` overlays span the *entire*
   screen height and sit on the Overlay layer, one level above this bar's
   `WlrLayer.Top` — they have no awareness of a second, independent
   process's bar and don't route pointer input around it, so they silently
   absorbed every click meant for it. Fix: moved this bar's own
   `WlrLayershell.layer` to `WlrLayer.Overlay` too. This fix was applied but
   **not confirmed working** before the whole approach was abandoned (see
   below) — worth knowing if this is ever retried.

**Reverted.** After the layer fix, the user decided the whole approach
wasn't worth continuing and asked to remove it before confirming whether the
last fix actually resolved the click issue. Removed cleanly: killed the
`quickshell -c bottom-bar` process, removed the `o.launch_on_start(...)`
line from `~/.config/hypr/autostart.lua`, deleted
`~/.config/quickshell/bottom-bar/` entirely, and restored Hardware Monitor,
OmaGlass, OmaListen, and WaveBar to the top bar's left section (their
original position, same order) via `~/.config/omarchy/shell.json`. Verified
via a clean `omarchy-restart-shell` (no errors) and a screenshot showing all
four back on the top bar, functional (they're on the real shell again, so
none of the click issues above apply).

No files were left behind in this repo for this attempt — nothing to
restore, nothing wired into `config/restore.sh`.

## SIA ("the Omarchy Brain") setup

[SIA](https://github.com/AnubisQuantumCipher/sia) (`khephri.sia`) is a much
bigger install than a typical bar-widget plugin — the README's "downloads
toolchains and builds components" undersells it. Confirmed before running
anything: it installs **Ollama** (a full local LLM/embeddings runtime) as a
systemd service, plus **Bun** and **Restic**, builds its own memory engine
(`gbrain`, pinned to a specific upstream commit), and enables a persistent
`sia-brainstem.service` — all before it does anything with journal/git/
package-install evidence.

**Blocker:** `./install.sh` failed immediately with `python-cryptography
with Ed25519 support is required: No module named 'cryptography'`. No pip on
this machine either. Fixed with `sudo pacman -S python-cryptography`
(`extra/python-cryptography`), then re-ran the installer.

**What the installer actually did**, step by step (`1/9` through `9/9`):
downloaded `restic`+`bun`, built `gbrain` from
[garrytan/gbrain](https://github.com/garrytan/gbrain) via bun install,
downloaded Ollama (1.3 GB), replayed the machine's evidence tails into a
fresh corpus (839 events / 835 pages / 260 graph nodes / 505 graph edges,
integrity check `pass`), enabled the plugin, and enabled+started
`sia-brainstem.service`. **Declined by default** (env-var opt-in, not
enabled): the `Super+Shift+B` keybind, the Claude Code agent skill
(`~/.claude/skills/sia/SKILL.md`), and MCP registration for
claude/codex/grok — none of those were turned on here.

Verified live: `systemctl --user status sia-brainstem.service` →
`active (running)`, 47.5 MB RSS, already logged a consolidation dream cycle
(`1145 pages embedded`) within a minute of starting; bar icon updated from a
"SETUP" prompt to its live status readout.

Storage/indexing/embeddings are local; an optional operator-configured CLI
judge may send recalled context (left unconfigured — disabled by default).
The corpus lives at `~/.local/share/sia/corpus` — the README calls it out
explicitly as worth backing up.

## Wallpaper Engine for Omarchy (patched local item)

[Wallpaper Engine for Omarchy](https://github.com/14brussell/Wallpaper-Engine-Omarchy)
(`io.github.14brussell.wallpaper-engine`) plays Steam Wallpaper Engine
Workshop items via the [linux-wallpaperengine](https://github.com/Almamu/linux-wallpaperengine)
backend. This machine has no Steam, no Wallpaper Engine purchase, and no
Workshop content — installed anyway (inert) at first, then wired up a real
local MP4 (`~/Wallpapers/*.mp4`, a downloaded "live wallpaper" video) as a
genuine Workshop-shaped item, entirely without Steam.

**1. Built the engine:** `yay -S linux-wallpaperengine-git` (AUR, ~sudo,
compiles from source against mpv/glfw/glew/etc. — all resolved cleanly).

**2. Wrapped the MP4 as a Workshop item.** The plugin already scans
`~/Wallpapers` (its default extra dir); a video item just needs a folder
with a `project.json` next to the video. Two non-obvious requirements, found
by reading the plugin's own `we_list_wallpapers()` scanner
(`lib/common.sh`) and `linux-wallpaperengine`'s actual parser
(`src/WallpaperEngine/Data/Parsers/ProjectParser.cpp`,
`WallpaperParser.cpp` upstream) rather than guessing:

- **The folder name must be purely numeric** (`^[0-9]+$`) — it mimics a
  Steam Workshop item id; a descriptive name is silently skipped by the
  scanner. Used `900000001` (obviously fake, no collision risk).
- **The video must be a real file inside the folder, not a symlink to
  somewhere else** — `linux-wallpaperengine`'s virtual filesystem layer for
  reading project assets doesn't follow symlinks that escape the item's own
  directory (`filesystem error: Cannot find requested file … [/scene.mp4]`
  until switched from `ln -s ../video.mp4 scene.mp4` to a plain `cp`).

`project.json` (the whole thing — `type`/`title`/`file` are all
`ProjectParser.cpp` requires):

```json
{
  "title": "Cyberpunk 2077 - Night City Ride",
  "type": "video",
  "file": "scene.mp4",
  "workshopid": "-1",
  "general": { "properties": {} }
}
```

**3. The real blocker: assets.** `linux-wallpaperengine` looks for Wallpaper
Engine's own shader/texture assets (normally installed by the actual Steam
app) and refuses to start without a valid `--assets-dir`, even for a plain
video wallpaper that never touches them:
`Cannot find a valid assets folder, resolved to
"/opt/linux-wallpaperengine/assets"`. Tested directly: an **empty** local
directory satisfies the check and video playback works fine without real
Wallpaper Engine assets (confirmed by capturing an actual rendered frame via
`--screenshot`, not just a clean exit code). Persisted by setting
`"assets_dir"` in `~/.config/omarchy/wallpaper-engine/config.json` (already
a recognized key — `we` has no CLI flag for it, edited the JSON directly) to
`~/.local/share/linux-wallpaperengine-assets` (empty dir, created for this).

Verified live: `we apply` painted all three displays
(`LWE FBO painted` per monitor), `we status` → `active=true engine=running`
with one PID per display, confirmed visually via the plugin's own panel
(all three displays showing "Running", correct wallpaper selected) and a
captured frame showing the actual video content.

Not done: `we install-hooks` (boot-persistence + theme-sync hooks) — the
wallpaper does not currently survive a reboot or `omarchy plugin update`
without re-running `we apply` by hand. Revisit if this should survive
restarts.

## Bar: position and arrangement (this machine)

Current state of `~/.config/omarchy/shell.json` (top bar, non-transparent,
`centerAnchor: "omarchy-google-calendar-clock"`, bar engine is the **stock**
`omarchy.bar` — see Caveats for why: **Bar Screens** was removed, and its
would-be replacement, **5bars**, was tried and reverted the same session).

Left → right order within the bar's three sections:

**left** (in order):

1. `evindor.keystroke` — Keystroke command palette (took the stock
 `omarchy.menu`'s place: it routes every `omarchy.menu` / menu-bar call)
2. `io.github.zucram.app-workspaces` — App Workspaces (real app icons per
 window, ordered spatially; replaced `omarchy.workspaces`, now disabled —
 see [App Workspaces](#app-workspaces-real-app-icons-per-workspace))
3. `io.github.grootaiinfinity.hwmon` (Hardware Monitor — moved from right)
4. `io.github.cjohnson46.omaglass` (OmaGlass — moved from right)
5. `io.github.hank-dev.listen` (OmaListen — lecture/audiobook launcher;
 auto-placed here on enable)
6. `io.github.erikburdett.wavebar` — WaveBar (patched — see
 [WaveBar (patched)](#wavebar-patched))

`vm.netspeed` (Netspeed) was removed from this section — see Caveats.

**center** (the `centerAnchor` drives which widget hugs dead-center):

1. `omarchy.indicators`
2. `omarchy.keyboard-layout`
3. `omarchy.weather`
4. `nasrullaameen.omihaze` (OmiHaze — dims inactive windows)
5. `omarchy-google-calendar-clock` — the clock (center anchor; stock
 `omarchy.clock` removed)
6. `omarchy.system-update`

**right** (in order):

1. `io.github.jondkinney.hyprpin` (Hyprpin — compositor-level PIP)
2. `davedes.fossfetch` (FossFetch — package search)
3. `io.github.lolu13.onote` (Onote — sticky notes; middle-click = new note,
 right-click = stack all)
4. `io.github.prathamesh913.paper-mode` (Paper Mode — screen shader toggle)
5. `wallpaper-align` (Wallpaper Align — image → bar widget; got silently
 swapped out for stock `omarchy.background` during the bar-engine changes
 below — caught and restored, see Caveats)
6. `im0001gt.screens` (Screens)
7. `omarchy.monitor`
8. `io.github.terrifiedbug.omaice` (OmaIce — chevron + tray drawer; replaces
 `omarchy.tray`, which is now disabled; **everything above it, items 1–7, is
 the hidden set** behind the chevron)
9. `omarchy.agents`
10. `omarchy.bluetooth`
11. `omarchy.network`
12. `omarchy.audio`
13. `omarchy.power`
14. `sam0110.oma-aux` (Oma Aux — PipeWire patchbay; auto-appended on enable)
15. `bjcatar.garden` (Garden — local coding-activity heatmap; half-hour
  squares from watched folders, not GitHub or token usage)
16. `io.github.i12bp8.netneighbors` (NetNeighbors — Wi-Fi radar; rootless,
  reads the kernel neighbor table)
17. `io.github.dataknox.music-dock` — Music Dock (needs a Hyprland-side
  setup step beyond the plugin install — see [Music Dock](#music-dock))
18. `luisreche.pomodoro` (Pomodoro — Braun-style focus/rest timer;
  auto-placed here on enable)
19. `jot` (Jot — sticky-notes deck; auto-placed here on enable)
20. `khephri.sia` — SIA (auto-placed here on enable; see
  [SIA setup](#sia-the-omarchy-brain-setup))
21. `nenadjokic.nvidia-hybrid` — NVIDIA Hybrid (auto-placed here on enable)
22. `io.github.skymebr.usbguard` — USBGuard (auto-placed here on enable;
  widget-only, no daemon — see Caveats)
23. `devtrack.streak` — DevTrack (auto-placed here on enable)
24. `io.github.14brussell.wallpaper-engine` — Wallpaper Engine (auto-placed
  here on enable; see [Wallpaper Engine (patched local
  item)](#wallpaper-engine-for-omarchy-patched-local-item))
25. `wkuehler.restic-monitor` — Restic Monitor (auto-placed here on enable)

`io.github.jeremylanger.omaspotify` (OmaSpotify) was removed — see
[OmaSpotify (tried, patched, then removed)](#omaspotify-tried-patched-then-removed).
`io.github.jondkinney.barscreens` (Bar Screens) was also removed — its dead
layout entry (left behind after `plugin remove`, since a removed plugin's
bar slot isn't auto-cleaned) was found and stripped from `shell.json`
directly.

**Not on the bar** (enabled as service/overlay; no bar slot):

- `io.github.i12bp8.oshelf` → `plugins` array in `shell.json`
- `keybindings.king` — King (overlay kind, no bar icon; `SUPER+SHIFT+K`
toggles it via `omarchy-shell shell toggle keybindings.king`, bound in
`~/.config/hypr/bindings.lua` since the plugin doesn't install its own
binding despite its README implying it does)
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
  [[ORCA_RICH_MD:c8a2b598744a4be8f0d66e59f1b0a8b4:document-link:-f%20%22%24CFG%22]] && jq -e '.mode == "span"' "$CFG" >/dev/null 2>&1
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
      if [[ORCA_RICH_MD:c8a2b598744a4be8f0d66e59f1b0a8b4:document-link:-f%20%22%24PREV%22]]; then
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
a preset menu). Service + bar-widget kinds; `omarchy plugin add https://github.com/Prathamesh913/paper-mode.git --enable --yes` auto-places the
widget in the bar's right section. IPC: `omarchy-shell paper-mode {status,toggle,enablePreset,disable,togglePreset}` (see below for why
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
re-apply with `git -C ~/.config/omarchy/plugins/io.github.prathamesh913.paper-mode apply .../paper-mode.hostipc.patch`, done by `config/restore.sh` step 7c).

The stock menu sends `setPreset <key>`, but the plugin's `IpcHandler` only
exposes `setPreset` as an in-process method — `omarchy-shell paper-mode setPreset grayscale` answers **"Function not found"**. So presets were only
reachable via left-click `toggle` (which flips to `lastPreset`, defaulting to
eink); the menu's Grayscale/Paper/E-Ink rows did nothing. The patch sends
`enablePreset <key>` instead, which is the exposed equivalent (`{"enabled":…, "preset":…}`), so every preset is selectable from the menu.

Regenerate the patch after editing the plugin:

```bash
git -C ~/.config/omarchy/plugins/io.github.prathamesh913.paper-mode diff \
    > ~/Projects/Omarchy-setup-2026/config/patches/paper-mode.hostipc.patch
```

Verified live: IPC `enablePreset eink` sets `decoration:screen_shader` to the
plugin's `eink.glsl` and `status` agrees; `disable` clears it back to empty.

## OmaSpotify (tried, patched, then removed)

[OmaSpotify](https://github.com/jeremylanger/omaspotify)
(`io.github.jeremylanger.omaspotify`) is a full Spotify client built in
Quickshell (Omarchy's own UI toolkit): search, browse, library, playlists,
queue, stats, equalizer, and lyrics, plus a local receiver that plays audio on
this computer — about 60 MB of RAM versus ~950 MB for the official client.
Service + bar-widget + panel kinds; `omarchy plugin add https://github.com/jeremylanger/omaspotify.git --enable --yes` auto-places the
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
same way Paper Mode's widget did — `bar.shell.serviceFor( "io.github.jeremylanger.omaspotify")` (BarWidget.qml:15) — so under the **Bar
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
rustup install (`curl https://sh.rustup.rs -sSf | sh -s -- -y --no-modify-path --profile minimal` — no sudo needed, installs to `~/.cargo` + `~/.rustup`).
`~/.cargo/bin/cargo` was then added to PATH so `scripts/setup.sh` can find it.
The first build takes ~1 minute; the resulting binary is at
`~/.local/lib/omaspotify/omaspotify-backend` with its unit
(`omaspotify.service`, static, started on demand — never enabled at login).

Optional (only if you want it): replace Omarchy's stock `SUPER + SHIFT + M · Music` binding by adding to `~/.config/hypr/bindings.lua`:

```lua
  hl.unbind("SUPER + SHIFT + M") -- previously: Music
  o.bind("SUPER + SHIFT + M", "OmaSpotify",
    "omarchy shell -q io.github.jeremylanger.omaspotify.player togglePlayer")
```

then `hyprctl reload` and check `hyprctl configerrors`. In the player's
Settings you can then pick whether that shortcut opens Omarchy's Music app, the
full player, or the mini-player; `omarchy shell -q io.github.jeremylanger.omaspotify.player {volumeUp,volumeDown}` raises/lowers
Spotify's own volume 5% per call.

Verified live: plugin loads with zero QML errors; service IPC target
`io.github.jeremylanger.omaspotify.player` is up; the playback backend built
and its unit installed, and the Connect-device credentials were stored
(`~/.local/state/omaspotify/{oauth,zeroconf}/credentials.json`). The backend
authenticated to Spotify's AP for a logged-in account — then stopped for
"free" accounts (the Premium gate above), so playback is not tested end-to-end
on this machine.

**Free-account attempt: mirror Brave's MPRIS session, then removed anyway.**
Once [Music Dock](#music-dock) and [WaveBar](#wavebar-patched) were both
working for the free account (Music Dock shows/hides the actual Spotify web
player running in Brave; WaveBar reads/controls it over MPRIS), tried making
OmaSpotify piggyback on that same MPRIS session instead of the Premium-gated
Web API/librespot path. The plugin already had almost the entire mechanism
built in for its own librespot backend: `activePlayer`/`hasLocalPlayer`
(`Service.qml`) already prefer a local MPRIS player over remote Web API state
for title/artist/album/art/position/length/volume/shuffle/repeat, and every
transport function (`togglePlayback()`, `next()`, `previous()`, seek, volume,
shuffle, loop) already calls straight into that MPRIS player object when one
is present — it just gated which MPRIS session counted as "local" through one
function, `isLocalEngine()`, that only matched `librespot` in the player's
`dbusName`/`desktopEntry`/`identity`. Widening that one match to also accept
`brave` (Brave exposes exactly one MPRIS session, for whichever tab currently
owns media focus — the same session WaveBar/Music Dock already use) made the
entire existing mirror/control path light up for free: confirmed via a
standalone JS test of the widened match against the live D-Bus service name
(`org.mpris.MediaPlayer2.brave.instance…`, `Identity: "Brave"`) and a clean
`qmllint`/shell-restart with no errors.

That covers *resuming/pausing/skipping whatever's already loaded* — but it
turned out not to be the whole story. OmaSpotify's search/browse UI starts
**new** playback through a completely different function, `playItem()`
(`Service.qml`), which posts to `/me/player` with a target device id — MPRIS
has no command for "load and play this specific track," only
toggle/next/previous/seek on whatever a player already has loaded, so that
path can never route around the Web API no matter what's patched. Confirmed
live: clicking Play on a search result still logged
`PUT /me/player/play ... Player command failed: Premium required
(PREMIUM_REQUIRED)` even with the widened `isLocalEngine()` in place, tracing
straight back to `playItem()`.

At that point OmaSpotify would only ever offer transport control over media
someone had *already* started playing some other way (i.e., from Brave/Music
Dock directly) — strictly a subset of what WaveBar already does, for a
second, heavier bar icon. Removed rather than keep two overlapping widgets:
`omarchy plugin remove io.github.jeremylanger.omaspotify --yes` (needed a
second attempt — the first hit the usual "shell busy right after a plugin
action" hiccup and only got as far as disabling it), plus its leftovers the
remove step doesn't touch: `~/.cache/omaspotify/` (874 MB, mostly the Rust
`target/` build dir), `~/.local/lib/omaspotify/` (built backend binary),
`~/.config/omaspotify/` (`playback.conf`), `~/.local/state/omaspotify/`
(session/credentials/library caches), and
`~/.config/systemd/user/omaspotify.service` (a static unit that was sitting
`failed`/`start-limit-hit` from the Premium-gated backend's crash loop —
`systemctl --user stop/reset-failed` before deleting it, then
`daemon-reload`). `~/.rustup`/`~/.cargo` were **not** removed — Onote's helper
also builds with `cargo build --release` and still needs them. The bar
returned to a normal 20-widget right section with no manual `shell.json`
cleanup needed. The Brave-local-engine patch itself
(`config/patches/omaspotify.brave-local-engine.patch`, a single ~10-line
change to `isLocalEngine()`) was written and verified against a fresh
upstream clone, then deleted along with its `config/restore.sh` step once the
plugin was removed — nothing left to apply it to.

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
right section (left-click notes &amp; stack, middle-click new note, right-click
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


| Keys          | Action                                                          |
| ------------- | --------------------------------------------------------------- |
| `Super+N`     | Notes &amp; Stack — full-text search, restore, confirmed delete |
| `Super+Alt+N` | New note                                                        |
| `Super+Alt+V` | New note from clipboard text                                    |
| `Super+Alt+H` | Stack all open notes                                            |
| `Super+Alt+P` | Pin a note on every workspace                                   |


Also one window rule: notes match `class = ^org.quickshell$` + the
 `— Onote [dn:…]` title suffix and get `tile = true` (never match the class
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


| Keys                | Action                                            |
| ------------------- | ------------------------------------------------- |
| `SUPER+ALT+Y`       | YouTube: browse and play (`youtube-float browse`) |
| `SUPER+ALT+SHIFT+P` | Overlay: hide/show (`float-overlay toggle`)       |
| `SUPER+ALT+CTRL+P`  | Overlay: close (`float-overlay quit`)             |
| `SUPER+ALT+O`       | Overlay: cycle size (`float-overlay size cycle`)  |


Verified live: picker registered, `youtube-float` on PATH, launcher entry
present, window rule loaded via `hyprland.lua`, `hyprctl reload` +
`configerrors` clean, shell restarted with zero QML errors.

## Hyprpin (compositor-level picture-in-picture)

[Hyprpin](https://github.com/jondkinney/hyprpin) (`io.github.jondkinney.hyprpin`)
keeps chosen windows visible when you switch away from their workspace: the
compositor moves them onto a **corner pop-out**, a **tiled edge**, a
**dedicated display**, or the **scratchpad** — compositor-level
picture-in-picture for video calls, a side monitor, or anything else you want
always on top without it getting lost. Service + bar-widget kinds.

**Bar icon:** click opens the window picker (choose a display and placement);
right-click toggles the on/off switch without opening the panel. While off,
every pop-out returns to its original workspace, position, size, and tiled or
floating state — rules are kept, and on resumes them. The icon dims while off.

**Stock `SUPER+O`/`SUPER+T`:** these are stock Omarchy bindings (centered pop
and tile toggle). Hyprpin intercepts them only while active; when off they fall
through unchanged. No extra bindings are needed for basic use.

**Optional bindings** (from the plugin's README; add to
`~/.config/hypr/local.lua` or `bindings.lua` if you want them):

```lua
hl.unbind("SUPER + P")
o.bind("SUPER + P", "Cycle Hyprpin placement / pseudo window", function()
  omarchy-shell("hyprpin", "cyclePlacement", {})
end)

hl.unbind("SUPER + ALT + S")
o.bind("SUPER + ALT + S", "Move window to scratchpad", function()
  omarchy-shell("hyprpin", "moveToScratchpad", {})
end)
```

Verified live: bar icon in the right section (index 2 after tray); service
loaded with zero QML errors; stock `SUPER+O`/`SUPER+T` unaffected.

## OmaIce (Ice-style hidden bar section)

[OmaIce](https://github.com/TerrifiedBug/omaice) (`io.github.terrifiedbug.omaice`)
replaces the system-tray chevron with one that hides **every bar widget placed
to its left** in the section, tray icons included. The point: the tray chevron
that Omarchy ships only ever hides tray icons; bar widgets (Spotify, a VPN
indicator, netspeed, hwmon...) had nowhere to go.

**Install is a two-step:** the plugin only makes sense with the stock tray out
of the way (`--enable` drops it straight after `omarchy.tray`; then
`omarchy plugin disable omarchy.tray`). Both were done here:

```bash
omarchy plugin add https://github.com/TerrifiedBug/omaice.git --enable --yes
omarchy plugin disable omarchy.tray
```

The tray rendering (icons, menus, pin/hide per icon) is **vendored from
Omarchy's own tray widget**, so OmaIce behaves like the thing it replaces —
right-click the chevron for the tray-icon pin/hide list on top. Pinned icons
stay in the bar while the section is collapsed; hidden ones never appear.

**Hiding rule — everything to the left of the chevron is hidden.** The hidden
set is always a contiguous run at the start of the section. Move widgets across
the boundary with Omarchy's bar drag-reorder, or right-click the chevron and use
the **"Bar widgets"** list (one `omarchy bar move` per widget as you close it).
Widgets dragged behind the chevron gather in front of it; removing OmaIce leaves
them where you put them.

**Behaviour** (also reachable from the chevron's right-click menu; apply live):


| Setting         | Default  | Meaning                                                              |
| --------------- | -------- | -------------------------------------------------------------------- |
| `rehideSeconds` | `0`      | Extra timeout before a revealed section closes; `0` = never          |
| `revealOnHover` | `false`  | Reveal on hover instead of on click                                  |
| `revealMode`    | `inline` | `inline` slides out beside the chevron; `row` shows a strip under it |


**Scriptable** (`omarchy-shell io.github.terrifiedbug.omaice toggle|reveal|hide|opened`).

Caveats: the plugin walks the QML scene (no supported sibling API — upstream
omacom/omarchy#10937). If the bar's structure changes, it runs as a "tray
drawer" and logs it. During the same session, disabling the tray while the shell
was running produced transient `Bar.qml` "Cannot assign [undefined]" warnings —
harmless, cleared by the shell restart. Current layout: chevron at the **head**
of the right section (index 0), so nothing is hidden yet; right-click the
chevron to park widgets behind it. Omarchy 4.0.3+ required (installed 4.0.3-1).

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

A timed note (an event **with a time**, e.g. `calendar-caldir new "Standup" -s "11:40" -d 30m`) now **automatically becomes an Omarchy reminder** that fires
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

- `**E**` — unchanged, opens the existing event form (still goes through
Caldir/Google like before).
- `**T` / `R` / `N`** — Todo / Reminder / Note. Each opens a small inline
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

- `~/.config/omarchy/plugins/[[ORCA_RICH_MD:c8a2b598744a4be8f0d66e59f1b0a8b4:inline-html:%3Cid%3E]]/` — git checkout of each plugin (this is the
whole distribution mechanism; `omarchy plugin update` fast-forwards the repo)
- `~/.config/hypr/monitors.lua` — becomes plugin-managed by Screens (fix 001)
- `~/.local/state/im0001gt.screens/` — Screens backups/profiles
- `~/.local/share/caldir` / `~/.config/caldir/config.toml` — Caldir runtime
dir + local calendar config (`calendar_dir = "~/Calendar"`)
- `~/Calendar/[[ORCA_RICH_MD:c8a2b598744a4be8f0d66e59f1b0a8b4:inline-html:%3Cslug%3E]]/` — local `.ics` calendar store (one subfolder per
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
- `~/.config/omarchy/plugins/io.github.jeremylanger.omaspotify/`,
`~/.local/lib/omaspotify/`, `~/.config/systemd/user/omaspotify.service`,
`~/.config/omaspotify/`, `~/.local/state/omaspotify/`, and its 874 MB
`~/.cache/omaspotify/` (mostly the Rust `target/` build dir) — all removed;
see [OmaSpotify (tried, patched, then
removed)](#omaspotify-tried-patched-then-removed)
- `~/.rustup/` + `~/.cargo/` — user-local Rust toolchain (no sudo); kept after
OmaSpotify's removal because Onote's helper also builds with
`cargo build --release` and still needs it
- `~/.local/state/keystroke/` — Keystroke state: `usage.json` (frecency /
preferences, hashed ids only, never query text); extensions are off until
enabled in Settings → Extensions
- `~/.local/bin/onote-helper` — Onote's Rust helper (built from
`~/.config/omarchy/plugins/io.github.lolu13.onote/helper/` via `cargo build --release`; must stay here because `omarchy plugin update` replaces the
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
`o.bind("SUPER + ALT + M", "Mouse: hint grid", "omarchy-shell shell toggle wkuehler.mouseless")` (added above the Onote `dofile` line; SUPER+M stayed
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
- `~/.config/omarchy/plugins/io.github.jondkinney.hyprpin/` — Hyprpin plugin
checkout (service + bar-widget; no patches, no external setup; rules are
created dynamically by the service via `hyprctl` when you pin a window)
- `~/.config/omarchy/plugins/io.github.terrifiedbug.omaice/` — OmaIce plugin
checkout (bar-widget-only; vendored tray rendering from stock `omarchy.tray`)
- `~/.config/omarchy/shell.json` → `/bar/layout/right` — `omarchy.tray` removed
(disabled) and `io.github.terrifiedbug.omaice` placed at the head of the
right section; `disabledPlugins` includes `omarchy.tray`
- `~/.config/omarchy/plugins/io.github.i12bp8.netneighbors/` — NetNeighbors
plugin checkout (bar-widget-only, rootless; no patches, no external setup)
- `~/.config/omarchy/plugins/io.github.dataknox.music-dock/` — Music Dock
plugin checkout (no patches, but see [Music Dock](#music-dock) for its
Hyprland-side setup step)
- `~/.config/hypr/music-dock.lua` — copied verbatim from the plugin
(`special:music` drop-down workspace, Spotify window-park rule,
`Super+Shift+M` rebind); not mirrored into this repo's `config/` — a
reinstall means re-copying it from the plugin checkout
- `~/.config/hypr/hyprland.lua` — one `require("hypr.music-dock")` line,
added after the `media-float` require block
- `~/.config/omarchy/plugins/khephri.sia/` — SIA plugin checkout;
`~/.local/share/sia/` (installed runtime, corpus, `gbrain`), `~/.local/state/sia/`,
`~/.config/systemd/user/sia-brainstem.service` (enabled) — see [SIA
setup](#sia-the-omarchy-brain-setup)
- `~/.config/omarchy/plugins/nenadjokic.nvidia-hybrid/` — NVIDIA Hybrid
plugin checkout (bar-widget-only; no patches, no external setup)
- `~/.config/omarchy/plugins/io.github.skymebr.usbguard/` — USBGuard plugin
checkout (bar-widget-only here; no daemon/policy installed — see Caveats)
- `~/.config/omarchy/plugins/devtrack.streak/` — DevTrack plugin checkout;
`~/.config/omarchy/devtrack/config.json` (usernames/reminder settings, not
yet configured)
- `~/.config/omarchy/plugins/io.github.14brussell.wallpaper-engine/` —
Wallpaper Engine plugin checkout; `~/.config/omarchy/wallpaper-engine/config.json`
(displays, `assets_dir`); `/home/darko/Wallpapers/900000001/` — hand-built
local Workshop item (`project.json` + `scene.mp4`, a copy of
`~/Wallpapers/349a738f11_…mp4`); `~/.local/share/linux-wallpaperengine-assets/`
— empty dir, satisfies the engine's assets-folder check — see [Wallpaper
Engine (patched local item)](#wallpaper-engine-for-omarchy-patched-local-item)
- `~/.config/omarchy/plugins/wkuehler.restic-monitor/` — Restic Monitor
plugin checkout (bar-widget-only; no restic backup units configured yet)

## Caveats

- **Bar engine switched: Bar Screens → stock `omarchy.bar`.** Removed Bar
Screens outright (no longer wanted). Switched to `omarchy.bar` *first*
(`omarchy plugin enable omarchy.bar`), confirmed it rendered, *then* removed
Bar Screens — doing it in that order avoids the known "removing the active
`kind: bar` plugin leaves `bar.id` null, no bar at all" gotcha documented
earlier in this file. Removing Bar Screens still left one dead entry behind
in `shell.json`'s right section (its own widget's layout entry — a removed
plugin's bar slot isn't auto-cleaned), found and stripped manually.
- **Side effect of the bar-engine switch: Wallpaper Align got silently
disabled.** Somewhere across `omarchy plugin enable omarchy.bar` / removing
Bar Screens / the 5bars detour below, `wallpaper-align` (the patched,
span-mode plugin) flipped to disabled and stock `omarchy.background`
flipped to enabled in its place — not a deliberate action, just discovered
while re-auditing the live bar layout for this doc update. Fixed:
`omarchy plugin enable wallpaper-align` + `omarchy plugin disable
omarchy.background`; `~/.config/omarchy/background-framing.json` still had
`"mode": "span"`, so nothing was actually lost. **Worth spot-checking after
any bar-engine change** — these first-party/third-party pairs (workspaces,
background, tray, menu) seem to get silently re-toggled by some of the
plugin-enable code paths.
- **Tried and removed:** `ErikBurdett/omarchy-rollingshot` — full-page
browser screenshot tool. Installed cleanly (service+overlay kinds, no bar
slot), keybind installer bound `Super+Shift+PrtSc`. Its default "auto" mode
shells out to a `chromium`/`chromium-browser` binary specifically for
browser-page capture — not installed here (only Brave/Zen) — so every
capture with a browser focused failed with `chromium is not installed`.
Rebound the key to the plugin's own `captureScroll` IPC method (grab-scroll-
grab-stitch, works with any window, no Chromium needed) plus a second
`Super+Shift+Alt+PrtSc` binding for `cancel`, verified both work end-to-end
(a fixed-region test capture, then a mid-capture cancel that actually killed
the helper process). Removed later at the user's request — plugin, both
keybind lines in `~/.config/hypr/bindings.lua`, and the installer's own
backup file, all cleaned up; `hyprctl configerrors` confirmed clean after.
- **Tried, misdiagnosed, then removed:** `cinco/omarchy-5bars` — per-monitor
bar layouts (each screen gets its own left/center/right, not just Bar
Screens' show/hide). Installed and switched to cleanly (`omarchy plugin
enable cinco.5bars` directly sets it active, like any `kind: bar` plugin).
The bar then rendered with the desktop wallpaper visibly bleeding through
large parts of it — looked exactly like a background-transparency rendering
bug in 5bars' patched `Bar.qml`, and it was removed on that assumption
(reverted to stock `omarchy.bar`). **That diagnosis was wrong**: the actual
cause, found only after removing 5bars, was `bar.transparent` having
drifted to `true` again (see the next entry below) — the exact same
pre-existing, unrelated bug already documented in this file, coincidentally
surfacing right when 5bars went in. Fixing transparency alone (`omarchy bar
transparent false`) fully resolved the corrupted look. 5bars was not
reinstalled to confirm — the user chose to stay on the stock bar rather than
re-test — so **5bars itself may well have been working correctly the whole
time**; don't rule it out on the strength of this entry alone if revisiting.
- **Bar transparency drifted again.** Same issue as the entry further down
this file from earlier in the session (`bar.transparent` flipping to `true`
with no clear trigger, no history in this repo for `shell.json`) —
recurred a second time, this time surfacing as apparent visual corruption
right after installing 5bars (see above), which is what caused it to be
misdiagnosed as a 5bars bug rather than caught directly. Reset again with
`omarchy bar transparent false`. Given it's now recurred twice with no
identified cause, worth treating "does the bar look wrong" as a
transparency check first, before assuming a plugin bug.
- **Widget-only installs left deliberately or currently inert:**
`khephri.sia` (SIA) was fully set up (see [SIA
setup](#sia-the-omarchy-brain-setup)) and `io.github.14brussell.wallpaper-engine`
was fixed and is actually playing a video wallpaper (see its own section)
— everything else installed this batch is inert, either on purpose or for
lack of setup: `io.github.skymebr.usbguard` (no `usbguard` daemon/policy —
the setup wizard that creates it was deliberately not run, see the plugin
table), `wkuehler.restic-monitor` (no `restic-backup*.service` units
configured), and `devtrack.streak` (no LeetCode/Codeforces/GitHub usernames
configured yet).
- Plugins run as arbitrary, **unsandboxed** code inside the long-lived
`omarchy-shell` process. Only install repos you trust and review the code
first — the marketplace validates listings, not security.
- Enabling a plugin that replaces a built-in widget disables the built-in
(observed with `air.workspaces`, since removed — removing it auto-restored
`omarchy.workspaces`). Re-enable the built-in later with
`omarchy plugin enable omarchy.workspaces`.
- **Tried and removed:** `omapalette` (`janooh37-hue/omapalette`) — palette
circles in the bar, matching/generating themes from the wallpaper
collection or a clicked color. Installed, enabled in the right section
(loaded with zero QML errors), then removed the same session along with
its cache (`~/.cache/omapalette/map.json`). `omarchy plugin remove` alone
unloads it from the bar cleanly; no shell.json cleanup needed.
- **Tried and removed:** `wolften.youtube-music` (YouTube Music) —
music.youtube.com as a Chromium-app-mode bar dropdown. Needed two patches
to work at all: `browser_executable()` only checked `chromium`/
`google-chrome*` (this machine has neither, only **Brave** — added as a
fallback), and the bar icon's `bar.shell.serviceFor(...)` is `null` under
**Bar Screens** (same class of bug as [Paper
Mode](#paper-mode-patched-for-replacement-bars) — patched with an IPC
fallback). Both verified working live (real Brave window, actual audio
playing) — then YouTube Music itself reported *"not available in your
area"*, a Google-side regional restriction unrelated to either patch and
not fixable from here. Removed, including the leftover profile dir
(`~/.local/share/omarchy-youtube-music/`) and a still-running transient
systemd unit from testing. The patch (still useful reference for the Bar
Screens `serviceFor` pattern) is kept at
`config/patches/youtube-music.brave-hostipc.patch` but is **not** wired
into `config/restore.sh` any more.
- **Tried and removed:** `io.github.sergebelov.airwaves` (Airwaves) —
OwnTone (AirPlay 2 media server) control from the bar: now playing,
transport, speaker routing, radio stations. Installed and enabled with
zero QML errors, but it's inert without a running OwnTone instance
(`http://localhost:3689/api`) plus `mpd-mpris` for transport/now-playing —
neither is installed here. OwnTone is AUR-only (`owntone-server`, no
official-repo package); `mpd-mpris` is in `extra`. Both installs need
interactive `sudo`/AUR-build confirmation this session couldn't supply
(no cached credentials, no TTY for a password prompt), so removed rather
than leave a half-wired media server. Revisit with `yay -S owntone-server`
  - `sudo pacman -S mpd-mpris` run manually first, then
  `omarchy plugin add https://github.com/sergebelov/omarchy-airwaves.git --enable --yes`.
- **Tried, patched, then removed:** `io.github.jeremylanger.omaspotify`
(OmaSpotify) — full Spotify client, but local playback needs Premium (this
account is free). Patched it to mirror/control Music Dock's Brave-hosted
Spotify web player over MPRIS instead, which worked for transport on
already-loaded media but not for starting new tracks from its own
search/browse UI (no MPRIS equivalent exists for that). Removed as a
redundant, heavier duplicate of what WaveBar already does for this account —
full story, and the leftover files the removal doesn't clean up on its own,
in [OmaSpotify (tried, patched, then
removed)](#omaspotify-tried-patched-then-removed).
- **Removed:** `vm.netspeed` (Netspeed) — no issue with it, just removed by
preference. `omarchy plugin remove` needed a second attempt (the first hit
the usual "shell busy right after a plugin action" hiccup and only got as
far as disabling it); the retry completed the removal.
- **Tried and reverted:** `sanjyay.peekbar` (PeekBar) — a full `kind: bar`
replacement (edge-triggered peek reveal over fullscreen apps, otherwise a
100%-compatible pinned bar). Installed and switched cleanly, all widgets
survived the swap — but `omarchy plugin remove` on a `bar`-kind plugin does
**not** restore the previous bar (`bar.id` was left `null`, no bar
rendering at all); had to `omarchy plugin enable io.github.jondkinney.barscreens` manually to bring Bar Screens back. Also
saw one live-bar-switch artifact both ways (bar rendered nothing until an
`omarchy restart shell`) — not a plugin bug, just something to expect when
switching the active bar mid-session.
- **Bar transparency drifted from the documented default.** Found
`~/.config/omarchy/shell.json`'s `bar.transparent` set to `true` with no
record of who/what changed it (not tracked by this repo, no history for
that file) — this doc had said "non-transparent" since it was first
written. Reset with `omarchy bar transparent false`. If you ever want it
transparent on purpose, that's the same command with `true` (or `toggle`).
- **Bar Screens gotcha:** it doubles as the bar *option* and its widget, so the
host gives it no per-widget shell facade — `omarchy bar put io.github.jondkinney.barscreens` reports "is on the bar" but does **not**
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

