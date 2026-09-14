# Hyprland crash — toggling a pinned overlay to floating aborts the compositor

- **Date:** 2026-09-14
- **Modified:** 2026-09-14
- **Problem:** Whole wayland session died (display went off; only the laptop panel
  came back, external dock monitors stayed dark) around 20:38, while interacting
  with the YouTube Float floating-video overlay.
- **Cause:** `Hyprland 0.56.2-2` aborted (`SIGABRT`, internal assertion) inside
  `CDwindleAlgorithm::movedTarget` while **toggling a window to floating from a
  keybind**. The full chain, from the core dump:

  ```
  libinput key event → handleKeybinds → Lua (guardedPCall) → Actions::floatWindow
  → changeFloatingMode → setFloating → moveTarget → CDwindleAlgorithm::movedTarget → assert + abort
  ```

  The only float-toggle in the setup is the Omarchy default **`SUPER+T`**
  (`/usr/share/omarchy/default/hypr/bindings/tiling.lua` →
  `hl.dsp.window.float({action="toggle"})`). The target window was the pinned,
  floating mpv overlay from YouTube Float (class `YouTubeFloat`, `float`/`pin`
  rules from `~/.config/hypr/media-float.lua`, sitting on
  `special:floatoverlay`). Dwindle's `movedTarget`/`addTarget` reached a node-tree
  state it cannot represent and asserted — aborting the compositor, session, and
  thereby the display outputs.

  The simultaneous `SIGBUS` cores (orca-ide, chrome crashpad handler, nautilus)
  were collateral — how children die when the compositor is torn down. No OOM
  involved (zero OOM-kill records for that boot).
- **Fix / workaround:** none available yet in the package repo — the current
  build (`0.56.2-2`, 2026-09-01) predates the upstream fixes. Avoid triggering:
  - do **not** use `SUPER+T` while the mpv overlay is focused — instead use
    `SUPER+ALT+SHIFT+P` (hide/show) and `SUPER+ALT+CTRL+P` (close);
  - after a crash, the compositor restart may leave external (dock) monitors
    undetected at the kernel level; a full reboot restores them
    (`hyprctl monitors` re-lists DP-4/DP-5).
  - check for a newer `hyprland` after Dwindle `movedTarget` fixes ship in the
    repos, then `sudo pacman -Syu`. Upstream references: hyprwm/Hyprland
    discussion #15789 (stale `m_draggingTiled`), commit `338bdbb` (#15415,
    expired weakptr when toggling a tiled window to floating during a drag).
- **Files touched:** none — diagnosis only (`coredumpctl info 1463`, debuginfod
  backtrace). Core retained by systemd coredump for reference:
  `/var/lib/systemd/coredump/core.Hyprland.1000.*.1463.*.zst`.