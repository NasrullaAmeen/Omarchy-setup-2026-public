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
| Screens | `im0001gt.screens` | Monitor layout — drag-to-arrange, scale, HDR/VRR, saved profiles. Takes over `~/.config/hypr/monitors.lua` on first run (see [fix 001](001-multi-monitor-screens-plugin.md)). | [IM0001GT/omarchy-screens](https://github.com/IM0001GT/omarchy-screens) | `omarchy plugin add https://github.com/IM0001GT/omarchy-screens --enable --yes` |
| oShelf | `io.github.i12bp8.oshelf` | A temporary desktop-edge "shelf" — park files, images, links, and text, then pick them up in another app/window. Service kind, no bar widget. | [i12bp8/oShelf](https://github.com/i12bp8/oShelf) | `omarchy plugin add https://github.com/i12bp8/oShelf.git --enable --yes` |
| Bar Screens | `io.github.jondkinney.barscreens` | Clone of the stock bar with per-monitor show/hide toggles — runs Omarchy's own bar untouched and parks it off-screen on unticked monitors, so updates can't break it and the stock bar's fixes keep arriving. Replaces the bar when enabled (set in Settings › Bar). | [jondkinney/omarchy-barscreens](https://github.com/jondkinney/omarchy-barscreens) | `omarchy plugin add https://github.com/jondkinney/omarchy-barscreens.git --enable --yes` |
| OmaGlass | `io.github.cjohnson46.omaglass` | Live network traffic monitor — speed gauges, history graph (30s–30m windows), active connections with country flags + on-demand whois, per-app/host/type/country usage, LAN device discovery. Read-only (no firewall/blocking). Auto-placed in the bar's right section on enable. | [cjohnson46/omarchy-omaglass](https://github.com/cjohnson46/omarchy-omaglass) | `omarchy plugin add https://github.com/cjohnson46/omarchy-omaglass.git --enable --yes` |
| Hardware Monitor | `io.github.grootaiinfinity.hwmon` | `CPU%` + package temp readout in the bar (right-click/scroll expands to MEM/GPU/BAT%); left-click opens a full system panel: CPU cores, memory/swap, thermals + fans, per-GPU meters, storage/disks, network, top processes, battery. Reads sysfs/procfs only — no privileges or network. Auto-placed in the bar's right section on enable. | [GrootAiInfinity/omarchy-hwmon](https://github.com/GrootAiInfinity/omarchy-hwmon) | `omarchy plugin add https://github.com/GrootAiInfinity/omarchy-hwmon.git --enable --yes` |
| Netspeed | `vm.netspeed` | Live download/upload speed in the bar (`↓ x ↑ y`), fixed-width from `/sys` interface counters so the bar never shifts. Hover → tooltip with interface + both rates, left/middle click → immediate refresh, right-click → toggle upload readout. Auto-placed in the bar's right section on enable; settings `refreshSeconds` (1–5, default 1) and `showUpload`. | [jhonoryza/omarchy-netspeed](https://github.com/jhonoryza/omarchy-netspeed) | `omarchy plugin add https://github.com/jhonoryza/omarchy-netspeed.git --enable --yes` |
| Wallpaper Align | `wallpaper-align` | Clone of stock `omarchy.background` (which is disabled) plus a bar widget: set image/color per screen or span the whole layout, fill/fit/stretch + edge align. **Patched here** for a `span` mode — see [Wallpaper Align + span mode](#wallpaper-align--span-mode-patched). Auto-placed in the bar's right section on enable. | [Primly/omarchy-wallpaper](https://github.com/Primly/omarchy-wallpaper) | `omarchy plugin add https://github.com/Primly/omarchy-wallpaper --enable --yes` then apply `config/patches/wallpaper-align.span.patch` (done by `config/restore.sh` step 7) |
| Days | `leonrlr4.days` | Per-day task list overlay: tasks belong to the day they're written, Markdown notes, paste screenshots (via `SUPER + V`), subtasks, and the day's calendar events beside them. Overlay kind, no bar widget; data is local JSON in `~/.local/share/leonrlr4.days/`. | [leonrlr4/days](https://github.com/leonrlr4/days) | `omarchy plugin add https://github.com/leonrlr4/days.git --enable --yes` then `~/.config/omarchy/plugins/leonrlr4.days/scripts/setup` (binds `SUPER + M`; idempotent, `--check` reports missing bits) |
| Omarchy Google Calendar and Clock | `omarchy-google-calendar-clock` | Bar clock + local-first calendar. Used here **only as the read-only event bridge for Days** (Days calls `scripts/calendar-events <from> <to>`). Runs the [Caldir](https://github.com/t4t5/caldir) runtime over local `.ics` folders; Google sync is optional and is **not** configured on this machine. | [NachoRodriguezM/omarchy-google-calendar-clock](https://github.com/NachoRodriguezM/omarchy-google-calendar-clock) | `omarchy plugin add https://github.com/NachoRodriguezM/omarchy-google-calendar-clock --enable --yes` then `scripts/setup --binaries-only` (installs Caldir without Google OAuth) |
| OmiHaze | `nasrullaameen.omihaze` | Dims inactive windows (macOS HazeOver-style) so the focused window stays visually dominant — auto-follows focus, live intensity slider, presets, scope, per-app exclusions (reads `hyprctl -j clients`; e.g. spares scratchpad if `excludeSpecialWorkspace`). Bar-widget kind. Auto-placed in the bar's **center** section on enable. | [NasrullaAmeen/omihaze](https://github.com/NasrullaAmeen/omihaze) | `omarchy plugin add https://github.com/NasrullaAmeen/omihaze.git --enable --yes` |

## Bar: position and arrangement (this machine)

Current state of `~/.config/omarchy/shell.json` (top bar, non-transparent,
`centerAnchor: "omarchy-google-calendar-clock"`, whole bar replaced by the
**Bar Screens** plugin: top-level `"id": "io.github.jondkinney.barscreens"`).

Left → right order within the bar's three sections:

**left** (in order):
1. `omarchy.menu`
2. `omarchy.workspaces` — per-monitor workspace groups (eDP-2 → IDs 6–8,
   DP-4 → 0,9, DP-5 → 1–5; eDP-2 group uses `bright_green` colorKey)

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
2. `wallpaper-align` (Wallpaper Align — image → bar widget)
3. `vm.netspeed` (Netspeed)
4. `io.github.grootaiinfinity.hwmon` (Hardware Monitor)
5. `io.github.cjohnson46.omaglass` (OmaGlass)
6. `im0001gt.screens` (Screens)
7. `omarchy.agents`
8. `omarchy.bluetooth`
9. `omarchy.network`
10. `omarchy.audio`
11. `omarchy.monitor`
12. `omarchy.power`
13. `io.github.jondkinney.barscreens` (Bar Screens' own widget — the
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
  `YYYY-MM-DDTHHMM__<slug>.ics`.
- Bridge check:
  `~/.config/omarchy/plugins/omarchy-google-calendar-clock/scripts/calendar-events 2026-09-14 2026-09-20`
  prints `{"ok":true,"events":[...]}`.
- Days caches one file per day in `~/.local/share/leonrlr4.days/events/YYYY-MM-DD.json`
  and refetches a couple of seconds after an overlay opens, so a new `.ics` may
  take a moment to appear.
- Google sync is intentionally off (`setup --binaries-only`), so the clock
  widget stays a plain clock — fine, Days is the only consumer.

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