# SNAKE — Premium Arcade Game

A feature-rich HTML5 Snake game with 3D graphics, 9 snake types, 100 stages, daily challenges, and a coin economy.

---

## Features

### Game Modes
- **STAGE** — 100 stages across 10 worlds. Progressive difficulty with monsters, obstacles and maps. Unlocks 10 stages at a time per world.
- **DAILY** — Seed-based daily challenge with combo system, bonus fruits, golden food, and portals. Same map for all players each day.

### Snake Types (9)
| Snake | Power | Cooldown |
|-------|-------|----------|
| 🐍 Classic | None | — |
| ❄️ Frost | Freeze all enemies 3s | 15s |
| 🔄 Flip | Shield — absorbs 1 hit | 1/stage |
| 🔥 Fire | Shoot fireball at enemies | 10s |
| 👤 Shadow | Ghost mode 5s (pass through all) | 20s |
| ⚡ Thunder | Speed burst 3s | 12s |
| 🧲 Magnet | Pull food to head | 10s |
| 🐸 Venom | Drop poison zone, kills monsters | 12s |
| 🌪 Storm | Stun all monsters 3s | 18s |

**Double-tap** the game canvas on mobile to activate your snake's power.

### Coin Economy
- Earn **5 🪙 coins** per completed stage
- Spend **20 🪙 coins** to continue a failed stage
- Coin balance shown in HUD and store
- Store button shows current coin balance

### Daily Mode Extras (not in Stage)
- Combo multiplier (x1–x4, 3s window)
- Bonus fruits: Speed, No-Walls, Ghost, Shrink
- Golden food (3s timer, worth 5×)
- Portals (level 4+)
- Moving obstacles (level 3+)

### Audio
- Ambient chiptune background music (E minor pentatonic loop)
- SFX for eat, power, game over, level up
- 🔇 Mute button in score panel — persists across sessions

---

## Architecture

Single file: `index.html` (~3200 lines)
- All CSS and JS embedded
- Canvas 2D rendering with 3D glossy aesthetic
- Web Audio API for all sounds
- LocalStorage for: high score, daily scores, stage progress, coins, mute preference

### Key JS Structures
```js
STAGES[100]   // [foodsToWin, timeSec, speedIdx, mapType, obsCount, monsters[], stageType]
SNAKE_DEFS[9] // {id, icon, name, color, desc}
MAP_TEMPLATES // 'open'|'cross'|'ring'|'islands'|'maze'|'corridor'
monsters[]    // {x, y, type:'roam'|'chase'|'patrol', lastMove}
venomCells[]  // {x, y, expiresAt} — toxic zones from Venom snake
```

### State Flags
```js
isStageMode   // true = stage, false = free/daily
selectedSnake // 'classic'|'frost'|'flip'|'fire'|'shadow'|'thunder'|'magnet'|'venom'|'storm'
isMuted       // persisted in localStorage
coins         // persisted in localStorage
```

---

## PWA / APK

- `manifest.json` + `sw.js` + `icon.svg` → installable PWA
- GitHub Actions (`build-apk.yml`) builds Android WebView APK on push to main
- APK artifact available under Actions → Artifacts → `snake-game-debug`

---

## Stage Worlds

| World | Stages | Theme |
|-------|--------|-------|
| NOVICE | 1–10 | Open maps, no monsters |
| WARRIOR | 11–20 | Cross/Islands, roam monsters |
| VETERAN | 21–30 | Ring/Maze, chase monsters |
| ELITE | 31–40 | Hard stages, multiple monsters |
| MASTER | 41–50 | Corridor, boss stages |
| LEGEND | 51–60 | Mixed maps, fast speed |
| TITAN | 61–70 | Extreme difficulty |
| PHANTOM | 71–80 | Nightmare speed |
| NEMESIS | 81–90 | Max monsters |
| INFERNO | 91–100 | Final challenge |

Boss stages (every 10th): harder conditions, fanfare sound, ⚔ icon.
Stage select shows only the current world + unlocked previous worlds.

---

## Sessions Log
- Session 1: Initial game, WASD/swipe, daily seed, combo, bonus fruits
- Session 2: PWA setup (manifest, sw.js, icon.svg)
- Session 3: Ghost fix, moving obstacles, shrink potion, golden food, portals
- Session 4: Stage mode (50 stages), snake select (4 types), boss stages
- Session 5: 100 stages, Candy Crush stage select, GitHub Actions APK fix
- Session 6: 3D rendering, 5 new snakes, double-tap power, background music
- Session 7: Stage-only mode (no combos/bonuses), progressive unlock (10/world), obstacle color, mute button, coin economy
