# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file, zero-dependency browser game (`Hive.html`). Open directly in a browser — no build step, no server, no dependencies.

## Architecture

Everything lives in `Hive.html`. Key sections:

- **CSS** — Industrial dark aesthetic using CSS variables (`--bg`, `--surface`, `--accent` amber `#f59e0b`). Fonts: Bebas Neue (display), IBM Plex Mono (mono).
- **Hex math** — Axial coordinate system. `R=44` is the hex radius. `hexXY(q,r)` converts axial coords to SVG pixel positions. `DIRS` is the 6-neighbor offset array.
- **Piece movement** — One function per piece type: `queenMoves`, `beetleMoves`, `hopperMoves`, `spiderMoves`, `antMoves`. All respect the one-hive rule via `isConnected(board, excl)`.
- **State** — `board` (Map of `"q,r"` → piece stack array), `hands` (counts per player), `turn`, `selected`, `validMoves`, `history` (undo stack). `applyState()` is the single state-transition function.
- **Rendering** — `render()` → `renderSVG()` (board), `renderHand()` (both panels), `renderMovePreview()` (bottom-left info panel). SVG is fully rebuilt on each render.
- **Piece icons** — `PIECE_ICONS` object maps type char (`Q B G S A`) to a function `(color, size) => SVG string`.
- **Block reasons** — `getBlockReason(q,r)` returns `{title, body}` or null; used for hover tooltips and the blocked-piece selection state.
- **Tweaks panel** — Hidden unless activated via `postMessage` from a parent frame (design tool integration). Toggles: hints, grid, coords.
