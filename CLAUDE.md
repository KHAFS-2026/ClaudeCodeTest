# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the games

No build step. Open any HTML file directly in a browser:

```
start shooter.html
start tictactoe.html
```

Or double-click the file in Explorer. There are no dependencies, servers, or install steps.

## Git & GitHub workflow

Remote: https://github.com/KHAFS-2026/ClaudeCodeTest.git

**Commit and push frequently throughout all work** — after each meaningful change, not just at the end. This ensures no progress is lost and the remote always reflects the current state. Never leave a completed step uncommitted.

```
git add <file>
git commit -m "description of what changed and why"
git push
```

Good commit checkpoints include: after adding a feature, fixing a bug, refactoring a section, or completing any discrete unit of work. Prefer small, focused commits over one large commit at the end.

Use `gh` CLI (GitHub CLI) for repo operations when available.

## Architecture

Each game is a **single self-contained HTML file** — all HTML, CSS, and JavaScript live together with no external dependencies.

### shooter.html

Canvas-based top-down space shooter. Key sections inside the `<script>` block (marked with `// === SECTION ===` banners):

- **STATE** — `gameState` string (`'start' | 'playing' | 'gameover'`), wave number, score, all entity arrays
- **ENTITIES** — `player` object; `playerBullets[]`, `enemyBullets[]`, `enemies[]`, `particles[]` arrays
- **SPAWNING** — `buildWave(n)` creates the enemy formation; boss spawns every 5th wave
- **COLLISION** — AABB via `overlaps(a,b)` (center-based); three passes per frame: player bullets vs enemies, enemy bullets vs player, enemy contact vs player
- **UPDATE / RENDER** — `update(dt)` mutates state; `render()` paints the frame in painter's-algorithm order
- **LOOP** — `requestAnimationFrame` with delta-time (`dt` capped at 50 ms)

Enemy movement is Space Invaders-style: a shared `waveVx` flips sign when the leading enemy hits a wall, dropping all enemies 20 px. High score persists via `localStorage`.

### tictactoe.html

DOM-based, no canvas. State is a flat 9-element `board` array. `init()` resets state and rebuilds cells; `handleClick(i)` applies a move, calls `checkWin()`, and updates scores. Scores survive across games in the `scores` object but are not persisted to storage.

## Design system

Both files share the same palette — use these values for any new UI:

| Role | Hex |
|---|---|
| Page background | `#1a1a2e` |
| Panel / canvas background | `#16213e` |
| Border accent | `#0f3460` |
| Primary highlight (red) | `#e94560` |
| Secondary highlight (cyan) | `#a8dadc` |
| Tertiary / warning (orange) | `#f4a261` |
| Body text | `#eeeeee` |
| Muted text | `#4a4a6a` |

Font: `'Segoe UI'`, uppercase letter-spacing for headings, `font-weight: 600–700` for emphasis.
