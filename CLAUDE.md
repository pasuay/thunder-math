# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Thunder Math — an arcade math-practice game for kids. **The entire app is one static file, `index.html`** (~2,800 lines of HTML + CSS in `<style>` + JS in `<script>` blocks). No build step, no package manager, no dependencies to install, no tests, no lint config. three.js (r128) is the only external dependency, loaded from a CDN at runtime.

## Commands

Nothing to build or compile. To run:

```bash
python3 -m http.server 8000   # then open http://localhost:8000
```

Serving is preferred over opening the file directly so Web Audio unlocks cleanly. There is no test or lint tooling — verify changes by playing the game in a browser.

**Deploy:** GitHub Pages serves `main` at the repo root → https://pasuay.github.io/thunder-math/. Pushing to `main` auto-redeploys (allow ~1 min plus CDN cache).

## Reading `index.html` (important)

The file is ~1.24 MB, but **~1.1 MB of that is a single line**: the line beginning `window.SPR = {` (around line 230) is base64-encoded sprite art. **Do not read or print that line** — it will flood your context and choke tools. Locate regions by landmark (line numbers drift as the file is edited):

- **Top of file** — `<head>`, all CSS in `<style>`, and the `<body>` DOM (`#cv`, `#gl`, `#overlay`, `#controls`, answer buttons, etc.).
- **Just before the blob** — a `<script src=...three.js...>` CDN tag, then the one-line `window.SPR` sprite blob (skip it).
- **WebGL overlay `<script>`** — the block that defines `window.__thunderGL = { ready, strike, resize }` (the three.js thunder layer). It **degrades gracefully if three.js fails to load** (the 2D game still runs).
- **Main game `<script>`** — the last and largest block, an IIFE beginning `(function () { "use strict";`. All game logic lives here; sprite art is decoded from `window.SPR` into the `sprites` object near its top.

Use grep or targeted line ranges; never `cat` the whole file.

## Architecture: mode generators + a shared engine

The key structure is the split between **modes** (which only generate problems) and the **shared engine** (which runs everything else). This is why a gameplay change like a new boss-intro screen is one edit, not four.

`MODES = { addsub, mul, place, pattern }` — each is a self-contained problem generator implementing this contract:

- `usesCountAid` — flag for the "count it out" wrong-answer aid (multiply only).
- `DIFFS` — `{ easy, medium, hard }`, each `{ name, pace, boss, stages[] }`. A stage defines `kinds`/`steps`, `max`, `dots` (show the manipulative), `mix`, etc.
- `gen(bossPhase)` → a **problem object** for the current stage (or boss pool).
- `factKey(...)` → a stable dedup key for the adaptive-review queue (signature varies per mode).
- `factFits(f)` → whether a queued review fact suits the current stage.
- `problemFromFact(f)` → re-create a problem from a queued fact.
- `drawAid(t, by, hot)` → draw the mode's concrete manipulative above a monster (ten-frame, array, base-ten blocks, number line).

A **problem object** is `{ ans, disp, op, fa, fb, aidN, key }`: `disp` is the string drawn on the monster, `ans` the correct value, `key`/`op`/`fa`/`fb` feed the review queue, `aidN` drives `drawAid`.

The engine reaches the active mode via `M()` (keyed by the global `mode`). `setMode()` / `startGame()` repoint `DIFFS`, `STAGES`, `pace`, `bossPool`, `MAX_LEVEL`; `curStage()` returns the active stage spec. **To add or change gameplay for a mode, edit only its object in `MODES` — the engine is mode-agnostic.**

## Cross-cutting engine systems

- **Adaptive review** — missed facts go into a per-mode `reviewQueue` (capped at 12); ~1 in 3 problems resurface one via `makeProblem() → pickReviewFact() → M().problemFromFact()`. `recordAnswer()` masters a fact after 2 correct. **Invariant:** the `key` a mode emits must be identical for the same fact whether freshly generated or resurfaced, or mastery never registers. (Place mode encodes the question `kind` into both `key` and `op` so that e.g. "tens in 47" and "47 = 40 + ?" are tracked as different skills.)
- **Persistence** — `storeGet`/`storeSet` try, in order: an injected async `window.storage`, then `localStorage` (keys namespaced `tm_`), then an in-memory `mem` map. Per-mode keys come from `kBest()` / `kStars(d)` / `kReview()` (`"…_" + mode`); addsub also falls back to legacy un-namespaced keys. Loaded review arrays are schema-filtered through `validReviewFact()`.
- **Game state** — `state ∈ { start, playing, bossAlert, victory, over }`; both `update(dt)` and `render()` branch on it. Boss flow: `checkLevel() → beginBossAlert() →(player taps)→ confirmBossAlert() → startBoss() → beginFirePhase()/beginDirectPhase()`. Main loop is `requestAnimationFrame(loop)` → `update` + `render`.
- **Combo / THUNDERSTORM** — correct answers build `charge`; at `chargeMax` the power button enables a screen-clearing chain-lightning (`unleashPower`).
- **Audio** — fully synthesized Web Audio, no audio files: `tone()`, `noise()`, the `sfx*()` family, and a `music` step-sequencer with `setBoss()`.
- **Rendering** — `#cv` is the 2D game + HUD; `#gl` is the optional WebGL thunder overlay. Sprites are decoded from `window.SPR` into `sprites` at boot.
- **Reduced motion** — a `reduceMotion` flag (read from `prefers-reduced-motion`, kept live via a `change` listener) gates the full-screen `flash`, screen `shake`, and lightning; a CSS `@media (prefers-reduced-motion: reduce)` block stops the decorative animation loops. New visual effects should check it.

## Conventions

Plain ES5 inside one strict-mode IIFE: `var`, function declarations, no modules, no TypeScript, no framework. Match the surrounding terse style; existing comments explain *why*, not *what*.
