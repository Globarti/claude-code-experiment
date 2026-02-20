# Claude Code Rules — claude-code-experiment

This repo is a playground for building browser games (starting with Snake).
Follow these rules to keep quality high and builds deployable.

---

## Game Architecture

### Game Loop
- Always use `requestAnimationFrame`. Never `setInterval` or `setTimeout` for the loop.
- Compute `deltaTime` from the `timestamp` argument — all movement must be dt-based so the game runs the same at any frame rate.
- Structure: `update(dt)` → `draw()` → `requestAnimationFrame(loop)`. Never mix logic and rendering.

### State Machine
- Maintain a single `gameState` enum: `MENU | PLAYING | PAUSED | GAMEOVER`.
- Branch on `gameState` at the top of `update()` and `draw()`. No scattered `if (isGameOver)` checks.

### Input
- Register `keydown`/`keyup` once at startup. Store state in a `keys` object. **Poll from `update()`, never act inside the event handler.**
- Prevent 180° reversal — buffer the next direction and validate before applying.
- Add touch/swipe support for mobile (map swipes to arrow directions).

---

## Code Quality

- Keep `update()` and `draw()` as pure as possible — no side effects that don't belong.
- No magic numbers: define constants at the top (`GRID_SIZE`, `TICK_MS`, `COLS`, `ROWS`, etc.).
- Single file is fine up to ~300 lines. Beyond that, split into ES modules (`src/snake.js`, `src/input.js`, etc.) and use `<script type="module">`.
- Use `roundRect` or smooth rendering — no naked pixel-perfect squares unless it's intentional art style.

---

## Visual Standards

- Snake head glows (use `ctx.shadowBlur`). Body uses gradient from head color to a darker tail color.
- Food pulses or has a subtle glow. Not a plain square.
- Background: dark (`#111` or darker). Grid dots optional but clean.
- Score and high score always visible in a minimal HUD.
- All canvas work in `#111` background with `#3BD5AE` as primary accent color.

---

## Features Every Snake Must Have

- [ ] Score + persistent high score (`localStorage`)
- [ ] Start screen (press arrow to begin)
- [ ] Game over screen with score + restart prompt
- [ ] Prevent 180° turn
- [ ] Wall collision kills snake
- [ ] Self collision kills snake
- [ ] Mobile swipe support

---

## Deployment

### GitHub Pages (primary)
Enable in repo Settings → Pages → source: `main` branch, `/ (root)`.
`index.html` at root = instant playable URL at `https://globarti.github.io/claude-code-experiment/`.

### Netlify Drop (instant share)
Drag the project folder to `app.netlify.com/drop` for a shareable link with no setup.

### itch.io (optional, for discoverability)
Zip the project, upload as HTML game. Set iframe size to match canvas dimensions.

---

## Local Dev

```bash
npx serve .          # zero-config local server at localhost:3000
# or
npx http-server .    # alternative
```

Never open `index.html` directly from the filesystem — `file://` breaks ES module imports.

---

## What to Build Next

Ideas in order of fun/effort:

1. **Better Snake** — speed increases over time, walls wrap (toggle), power-ups (speed boost, score x2), particle explosion on death
2. **Snake with levels** — obstacles per level, increasing speed
3. **Two-player Snake** — WASD vs arrows, same canvas
4. **Breakout / Arkanoid** — ball, paddle, bricks
5. **Asteroids** — rotation + thrust, bullets

---

## Commit Rules

- Commit after every working feature (not half-finished states).
- Message format: `Add <feature>` / `Fix <bug>` / `Improve <thing>`
- Always push after committing.
