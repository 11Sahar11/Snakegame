# SNAKE — Neon Arcade Game

## Project Overview
Single-file HTML5 Snake game with neon retro aesthetic, PWA support, and progressive gameplay modes.

## Files
| File | Purpose |
|------|---------|
| `index.html` | Main game (~2000+ lines, all HTML/CSS/JS embedded) |
| `manifest.json` | PWA manifest (icons, theme, standalone mode) |
| `sw.js` | Service worker — cache-first offline support |
| `icon.svg` | App icon 512×512 (snake S-curve, neon green) |
| `android/` | Android WebView APK project (built via GitHub Actions) |
| `.github/workflows/build-apk.yml` | CI: builds debug APK automatically on push |

## Current Game Modes
| Mode | Description | Status |
|------|-------------|--------|
| **STAGE** | 50 progressive levels, monsters, maps, win conditions | ✅ Active |
| **DAILY** | Seeded challenge — same map every day, save best score | ✅ Active |
| **STORE** | Unlock different snakes with special powers | 🔒 Planned |

## Implemented Features
- Neon "twilight" visual theme (Orbitron + Share Tech Mono fonts)
- Responsive canvas (auto-sizes to viewport)
- Particle bursts on food eat
- Floating score text (+N flies up)
- Speed trail (golden dots during speed boost)
- CRT scanlines + vignette overlay
- Combo system (x2/x3/x4 for consecutive fast eats)
- Bonus fruits: Speed ⚡, No-Walls 🧱, Ghost 👻, Shrink ✂️
- Golden food ⭐ (rare, 3s, worth 5× combo)
- Teleport portals 🌀 (level 4+ in classic mode)
- Moving obstacles (2-cell jumps every 4s from level 3)
- PWA install (Add to Home Screen on iPhone/Android)
- Android APK via GitHub Actions

## Stage Mode Architecture
- **50 stages** in `STAGES` array: `[foods, timeSec, speedIdx, mapType, staticObsCount, monsters[], isHard]`
- **6 map types**: open / cross / ring / islands / maze / corridor
- **3 monster types**: Roam (random), Chase (30% toward snake), Patrol (back-and-forth)
- **Win**: eat N foods within time limit
- **Stars**: 3★ fast, 2★ normal, 1★ barely made it
- **Progress**: saved in `localStorage['snakeStageProgress']`

## Architecture Notes
- Game loop: `setInterval(tick, speed)` — speed varies by level/bonus
- Rendering: Canvas 2D with `requestAnimationFrame` for effects only
- Audio: Web Audio API (square/sawtooth/triangle oscillators)
- Touch: `document.addEventListener('touchstart/end')` with `elementFromPoint` guard for buttons
- Daily seed: `year*10000 + month*100 + day` → seeded RNG (LCG xor hash)

## Roadmap
- [ ] STORE: buy skins with coins earned from stages
- [ ] Snake skins: Classic, Fire, Ice, Shadow, Rainbow — each with unique power
- [ ] Coin system: earn coins per stage star rating
- [ ] Achievements system
- [ ] Leaderboard (if backend added)
- [ ] Sound on/off toggle
- [ ] More stage maps

## Session Log
| Date | Changes |
|------|---------|
| Session 1 | Initial game, responsive canvas, audio, combo |
| Session 2 | PWA (manifest, sw.js, icon.svg), Apple meta tags |
| Session 3 | Ghost mode fix, moving obstacles, addictive gameplay ideas |
| Session 4 | Visual redesign (twilight theme, Orbitron font, dot-grid bg) |
| Session 5 | Particles, floating text, speed trail, start screen redesign |
| Session 6 | Android APK (GitHub Actions, WebView project) |
| Session 7 | Shrink bonus, golden food, portals, obstacles move 2 cells |
| Session 8 | Stage mode (50 levels), monsters, maps, store stub, README |
| Session 9 | Full Stage mode implementation: STAGES array (50 entries), MAP_TEMPLATES (6 types), monster logic (roam/chase/patrol), stage-select grid with star ratings, stage HUD (timer+food), stagewin screen, progress saved in localStorage |

## Running Locally
Open `index.html` in any modern browser. No build step needed.
For PWA features (service worker): must be served over HTTP/HTTPS, not `file://`.
Recommended: `npx serve .` or push to GitHub Pages.

## GitHub Pages Deploy
1. Push all files to a GitHub repo
2. Settings → Pages → Source: main branch / root
3. Access at `https://username.github.io/repo-name/`
4. On iPhone/Android Safari: Share → Add to Home Screen
