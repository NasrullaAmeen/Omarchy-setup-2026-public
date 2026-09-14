# Feature request: `span` mode — one wallpaper across the whole monitor arrangement

**Plugin:** Wallpaper Align (`omarchy-wallpaper`, bar widget `wallpaper-align`)
**Date:** 2026-09-14
**Omarchy / Quickshell:** standard marketplace build, Linux (Hyprland)

## Before opening this — I checked

- Searched the repo (Issues + PRs, open and closed) for `span`, `multi-monitor`,
  `stretch across`, `array` — **nothing found**; this repo currently has 0
  issues and 0 PRs.
- Read `README.md`, `LICENSE` (MIT), `manifest.json`. No `CONTRIBUTING` /
  `CODE_OF_CONDUCT` files — nothing imposes a template.
- Read `scripts/framing-io.py`: it is **mode-agnostic** (pure path/ownership/
  symlink/atomicity safety; no mode whitelist), so a new `"span"` mode flows
  through it unchanged — **no I/O layer changes required**.
- The mode *normalization* the README mentions (unknown → default) happens in
  `normalizedFraming` inside `Background.qml`/`Framing.qml` — the exact spot
  this patch extends.

Note on process: *Issue creation reportedly displays as restricted for this
repo.* If that's the case, view this as the PR description instead — the full
patch is included below and I'm happy to submit a clean PR from a fork.

## Summary

Add a **`span`** framing mode that draws a single wallpaper image continuously
across the entire monitor arrangement — one canvas over the union bounding box
of all outputs, each screen showing the slice that matches its position in the
layout. This is the classic GNOME/KDE "span" behaviour;

```
┌─────────────┬──────────────────────────┬──────────┐
│             │                          │          │
│   DP-5      │          DP-4            │   eDP-2  │
│ 3440×1440   │        2560×1440         │ 1600×1000│
│  @ (0,0)    │     @ (3440,437)         │@(1016,1440)
│             │                          │          │
└─────────────┴──────────────────────────┴──────────┘
                  canvas ≈ 6000 × 2440
```

## Current behaviour

`fill` / `fit` / `stretch` + edge alignment are all **per-screen**: every output
repeats the chosen image on its own. There is no way to draw one image across
multiple outputs, so an ultrawide setup never gets a "panorama" wallpaper.

## Proposed behaviour

- New mode `"span"` (accepted aliases: `"across"`, `"span-all"`), fourth option
  in the panel's **Scale** row (`Fill | Fit | Stretch | Span`).
- `monitorLayout()` computes the union bounding box of `Quickshell.screens`
  (logical coordinates, so mixed-DPR layouts line up).
- The image is fitted to that bounding box with **cover** semantics; `halign` /
  `valign` decide which part of the image survives the crop when its aspect
  ratio doesn't match the canvas (same left/center/right + top/center/bottom
  controls already exposed).
- Each output renders only its own slice: the shared image is positioned with
  x/y/width/height computed in the screen's local coordinate space and drawn
  `Image.Stretch` under a `clip: true` layer, so nothing bleeds across
  outputs.
- **Span is a whole-layout mode.** Selecting it in the panel clears any
  per-screen override and resets `monitors: {}` so every output inherits it.
  It is stored in the existing framing config as
  `{"mode":"span","halign":...,"valign":...,"monitors":{}}` — no schema
  change, no new files, fully backward-compatible.

## Patch (works out of the box against the current plugin)

Applied on top of the current clone; both `Background.qml` (rendering) and
`Framing.qml` (panel UI) are touched.

```diff
diff --git i/Background.qml w/Background.qml
index 6509e14..fc62fe9 100644
--- i/Background.qml
+++ w/Background.qml
@@ -80,6 +80,7 @@ Item {
     if (["fill", "cover", "crop"].indexOf(mode) !== -1) mode = "fill"
     else if (mode === "fit" || mode === "contain") mode = "fit"
     else if (mode === "stretch" || mode === "fill-stretch") mode = "stretch"
+    else if (["span", "across", "span-all"].indexOf(mode) !== -1) mode = "span"
     else mode = "fill"
     if (["left", "center", "right"].indexOf(h) === -1) h = "center"
     if (["top", "center", "bottom"].indexOf(v) === -1) v = "center"
@@ -97,6 +98,26 @@ Item {
     return out
   }

+  // Bounding box of all connected outputs in global (logical) coordinates.
+  // Used by "span" mode to render one wallpaper continuously across screens.
+  function monitorLayout() {
+    var list = Quickshell.screens || []
+    if (list.length === 0) return null
+    var minX = Infinity, minY = Infinity, maxX = -Infinity, maxY = -Infinity
+    for (var i = 0; i < list.length; i++) {
+      var s = list[i]
+      var w = Number(s.width || 0), h = Number(s.height || 0)
+      if (w <= 0 || h <= 0) continue
+      var x = Number(s.x || 0), y = Number(s.y || 0)
+      minX = Math.min(minX, x)
+      minY = Math.min(minY, y)
+      maxX = Math.max(maxX, x + w)
+      maxY = Math.max(maxY, y + h)
+    }
+    if (maxX <= minX || maxY <= minY) return null
+    return { minX: minX, minY: minY, W: maxX - minX, H: maxY - minY }
+  }
+
   function applyFramingConfig(text) {
     var parsed = null
     try { parsed = JSON.parse(String(text || "")) } catch (e) { parsed = null }
@@ -451,6 +472,38 @@ Item {
       readonly property int frameFillMode: root.framingFillModeFor(frameCfg)
       readonly property int frameHAlign: root.framingHAlignQtFor(frameCfg)
       readonly property int frameVAlign: root.framingVAlignQtFor(frameCfg)
+      readonly property bool spanMode: frameCfg.mode === "span"
+
+      // Geometry (as Qt.rect in this screen's local coordinate space) for a
+      // wallpaper Image drawn across the whole monitor layout, so that each
+      // output shows the region of the image matching its position in the
+      // arrangement. Returns null until the image has loaded.
+      function spanRectFor(img) {
+        if (!img || !img.sourceSize || !img.sourceSize.width || !img.sourceSize.height) return null
+        var layout = root.monitorLayout()
+        if (!layout) return null
+        var iw = img.sourceSize.width, ih = img.sourceSize.height
+        var k = Math.max(layout.W / iw, layout.H / ih)
+        var dispW = iw * k, dispH = ih * k
+        var cfg = frameCfg
+        var hfrac = cfg.halign === "left" ? 0 : (cfg.halign === "right" ? 1 : 0.5)
+        var vfrac = cfg.valign === "top" ? 0 : (cfg.valign === "bottom" ? 1 : 0.5)
+        var baseX = layout.minX + (layout.W - dispW) * hfrac
+        var baseY = layout.minY + (layout.H - dispH) * vfrac
+        var s = modelData
+        var sx = s ? Number(s.x || 0) : 0
+        var sy = s ? Number(s.y || 0) : 0
+        return Qt.rect(baseX - sx, baseY - sy, dispW, dispH)
+      }
+
+      // Safe span geometry for Image bindings; falls back to covering this
+      // screen until the image is ready (match behaviour of plain fill).
+      function spanGeom(img) {
+        if (!spanMode) return null
+        var r = spanRectFor(img)
+        if (r) return r
+        return Qt.rect(0, 0, layers.width, layers.height)
+      }

       function maybeStartReveal() {
         if (!root.incomingBackground || root.revealProgress !== 0 || maskReady) return
@@ -467,13 +520,21 @@ Item {
       WlrLayershell.keyboardFocus: WlrKeyboardFocus.None
       exclusionMode: ExclusionMode.Ignore

+      Item {
+        id: layers
+        anchors.fill: parent
+        clip: true
+
       Image {
         id: base
-        anchors.fill: parent
+        x: panel.spanMode ? panel.spanGeom(base).x : 0
+        y: panel.spanMode ? panel.spanGeom(base).y : 0
+        width: panel.spanMode ? panel.spanGeom(base).width : parent.width
+        height: panel.spanMode ? panel.spanGeom(base).height : parent.height
         source: root.imageUrl(root.displayedBackground)
-        fillMode: panel.frameFillMode
-        horizontalAlignment: panel.frameHAlign
-        verticalAlignment: panel.frameVAlign
+        fillMode: panel.spanMode ? Image.Stretch : panel.frameFillMode
+        horizontalAlignment: panel.spanMode ? Image.AlignLeft : panel.frameHAlign
+        verticalAlignment: panel.spanMode ? Image.AlignTop : panel.frameVAlign
         asynchronous: true
         cache: true
         smooth: true
@@ -489,11 +550,14 @@ Item {

       Image {
         id: oldFrame
-        anchors.fill: parent
+        x: panel.spanMode ? panel.spanGeom(oldFrame).x : 0
+        y: panel.spanMode ? panel.spanGeom(oldFrame).y : 0
+        width: panel.spanMode ? panel.spanGeom(oldFrame).width : parent.width
+        height: panel.spanMode ? panel.spanGeom(oldFrame).height : parent.height
         source: root.imageUrl(root.oldBackground)
-        fillMode: panel.frameFillMode
-        horizontalAlignment: panel.frameHAlign
-        verticalAlignment: panel.frameVAlign
+        fillMode: panel.spanMode ? Image.Stretch : panel.frameFillMode
+        horizontalAlignment: panel.spanMode ? Image.AlignLeft : panel.frameHAlign
+        verticalAlignment: panel.spanMode ? Image.AlignTop : panel.frameVAlign
         asynchronous: true
         cache: false
         smooth: true
@@ -517,11 +581,14 @@ Item {

         Image {
           id: incomingFrame
-          anchors.fill: parent
+          x: panel.spanMode ? panel.spanGeom(incomingFrame).x : 0
+          y: panel.spanMode ? panel.spanGeom(incomingFrame).y : 0
+          width: panel.spanMode ? panel.spanGeom(incomingFrame).width : parent.width
+          height: panel.spanMode ? panel.spanGeom(incomingFrame).height : parent.height
           source: root.imageUrl(root.incomingBackground)
-          fillMode: panel.frameFillMode
-          horizontalAlignment: panel.frameHAlign
-          verticalAlignment: panel.frameVAlign
+          fillMode: panel.spanMode ? Image.Stretch : panel.frameFillMode
+          horizontalAlignment: panel.spanMode ? Image.AlignLeft : panel.frameHAlign
+          verticalAlignment: panel.spanMode ? Image.AlignTop : panel.frameVAlign
           asynchronous: true
           cache: false
           smooth: true
@@ -566,6 +633,8 @@ Item {
         }
       }

+      }
+
       MouseArea {
         anchors.fill: parent
         acceptedButtons: Qt.LeftButton | Qt.RightButton
diff --git i/Framing.qml w/Framing.qml
index a2b1b5f..f399d78 100644
--- i/Framing.qml
+++ w/Framing.qml
@@ -41,6 +41,7 @@ Panel {
     if (["fill", "cover", "crop"].indexOf(mode) !== -1) mode = "fill"
     else if (mode === "fit" || mode === "contain") mode = "fit"
     else if (mode === "stretch") mode = "stretch"
+    else if (["span", "across", "span-all"].indexOf(mode) !== -1) mode = "span"
     else mode = "fill"
     if (["left", "center", "right"].indexOf(h) === -1) h = "center"
     if (["top", "center", "bottom"].indexOf(v) === -1) v = "center"
@@ -316,10 +317,21 @@ Panel {
           options: [
             { value: "fill", label: "Fill", tooltip: "Cover screen, crop overflow" },
             { value: "fit", label: "Fit", tooltip: "Whole image, letterbox bars" },
-            { value: "stretch", label: "Stretch", tooltip: "Ignore aspect ratio" }
+            { value: "stretch", label: "Stretch", tooltip: "Ignore aspect ratio" },
+            { value: "span", label: "Span", tooltip: "One image across all screens" }
           ]
           value: root.effFraming.mode
-          onChanged: function(v) { root.saveFraming(v, root.effFraming.halign, root.effFraming.valign) }
+          onChanged: function(v) {
+            if (v === "span") {
+              // Span is a whole-layout mode: drop the screen override and
+              // clear per-screen modes so every output inherits it.
+              root.framingTarget = ""
+              var copy = JSON.parse(JSON.stringify(root.framingMonitors))
+              for (var key in copy) delete copy[key]
+              root.framingMonitors = copy
+            }
+            root.saveFraming(v, root.framingHAlign, root.framingVAlign)
+          }
         }

         Text {
@@ -327,7 +339,9 @@ Panel {
             ? "Fill covers the screen; edges crop off."
             : root.effFraming.mode === "fit"
               ? "Fit shows the whole image; bars are black."
-              : "Stretch fills exactly; shapes may distort."
+              : root.effFraming.mode === "span"
+                ? "Span draws one wallpaper across the whole monitor arrangement."
+                : "Stretch fills exactly; shapes may distort."
           color: Qt.darker(Color.popups.text, 1.5)
           font.family: Style.font.family
           font.pixelSize: Style.font.caption
```

Apply with:

```bash
omarchy plugin add https://github.com/Primly/omarchy-wallpaper --enable --yes
git -C ~/.config/omarchy/plugins/wallpaper-align apply <patch-file>
omarchy restart shell
```

## Framing config

`~/.config/omarchy/background-framing.json` (span default used for the toggle
below):

```json
{ "mode": "span", "halign": "center", "valign": "center", "monitors": {} }
```

`halign`/`valign` mirror the existing edge-alignment controls and choose which
part of the image survives when its aspect doesn't match the canvas (cover).

## Supporting toggle script

A user-side convenience that flips the config file exactly as your README's
"Configure without the panel" section does (edit the file, it hot-reloads).
`--check` reports whether span is the active mode (usable for a "✓" condition
in the Omarchy style menu, which accepts `checked:` shell commands). Note it
writes the file directly for simplicity — if you adopt it, an upstream version
should go through `scripts/framing-io.py` to keep the atomic-write guarantee:

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

Optional Omarchy style-menu entry (nested under `style` via the dotted id in
`~/.config/omarchy/extensions/omarchy-menu.jsonc` — hot-reloaded, no restart):

```jsonc
"style.wallpaper-span": {
  "icon": "󰖟",
  "label": "Wallpaper: span all screens",
  "description": "Draw one wallpaper across the whole monitor arrangement",
  "action": "omarchy-wallpaper-span-toggle",
  "checked": "omarchy-wallpaper-span-toggle --check"
}
```

## Verification

**Confirmed working on this setup** (3 outputs: laptop panel eDP-2 at 1.6
scale + two DPI desktop displays, layout above — the wallpaper is drawn once
across all three and each screen shows its aligned slice with no visible
seams or re-scaling). For a total canvas this large (6000×2440) I AI-upscaled
the source from 1920×1080 to **9600×5400** (Upscayl, Ultrasharp model) so the
per-screen crop stays crisp.

- Multi-DPI, mixed-scale layout (laptop at 1.6 + two DPI displays): slices line
  up pixel-perfect because the plan works entirely in `Quickshell.screens`
  logical coordinates.
- Wallpaper transitions (`oldFrame`/`incomingFrame`) and the reveal animation
  behave identically in span mode (all three `Image`s use the span geometry).
- No regression in `fill`/`fit`/`stretch`: those paths are untouched
  (`anchors.fill` equivalent via per-mode x/y/width/height).
- Span is skipped gracefully when a screen has no geometry (`monitorLayout()`
  returns null) or the image hasn't loaded (falls back to covering the screen).

## One design question for you

When `span` is chosen in the panel I clear per-monitor `monitors: {}` overrides,
since span is inherently a whole-layout mode. If you'd rather keep spans
per-screen-assignable (a "span" cell in the per-screen matrix), that's a
different UI — happy to rework the panel side to match your intended model.

## Keeping the clone in sync

Your README's Compatibility note says the plugin tracks the stock background
renderer (`Background.qml` diffed against
`/usr/share/omarchy/shell/plugins/background/Background.qml` after each
`omarchy update`). The span changes are additive and localised
(`monitorLayout()` + the three `Image`s + the panel option), so a stock-side
change should never collapse into them — but if you accept this upstream, the
README tables (Scale row, "Configure without the panel") and the `Framing.qml`
default `changeText` would want a `span` row/sentence.

Thanks for maintaining the plugin. Happy to iterate here and, if you'd like it
upstream, open a clean PR from a fork.