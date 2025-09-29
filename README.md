# Scoundrel (Web)

A polished, accessible, mobile-first web version of the solo card game Scoundrel. Vanilla HTML/CSS/JS. No build step required.

## Play

Just open `index.html` in a modern browser. The game auto-saves to LocalStorage after each action and resumes automatically when you come back.

## Features

- Full rules implementation with clear UI feedback and logs
- 3D card flip animation with proper backs (honors `prefers-reduced-motion`)
- Killer card shown on Game Over (mini card preview), with centered score line
- Mobile-friendly layout:
  - HUD health meter on its own row; four panels on one row
  - Action buttons (“Face Selected”, “Avoid Room”) sized to card columns
  - Compact header with a menu toggle
- Accessible: keyboard support, focus styles, ARIA live log
- Persistent saves: picks up where you left off

## Rules Summary

- Health: start at 20 (max 20). Clear the dungeon to win; if health ≤ 0, you lose.
- Deck setup (44 cards total):
  - Monsters: all clubs/spades (♣/♠): 2–10, J=11, Q=12, K=13, A=14 (26)
  - Weapons: diamonds (♦) 2–10 (9)
  - Potions: hearts (♥) 2–10 (9)
  - Removed: red face cards (J/Q/K ♥♦) and red Aces (A♥, A♦)
- Room: draw up to 4 cards visible.
  - Avoid Room: put all 4 on bottom (cannot avoid twice in a row)
  - Face: choose 3 to resolve in any order; the 4th carries to next room
- Resolving:
  - Weapon (♦): equip; discard old weapon and its stacked monsters; resets last‑defeated gate
  - Potion (♥): heal by its value (cap 20). Only one potion per room heals; extras are discarded
  - Monster (♣/♠):
    - Bare-handed: take full value as damage
    - With weapon: damage = max(0, monster − weapon). If 0, monster is defeated and stacked
    - Non‑increasing gate: you can only use the weapon on values ≤ the last defeated monster (or any value until your first defeat with that weapon)
- End & Score:
  - Win: score = remaining health
  - Lose: score = negative sum of remaining monster values in deck/room/carry; Game Over modal shows the killer card

## Files

- `index.html` — App structure: HUD, room grid, log, modals (How to Play, Game Over)
- `css/styles.css` — Theme, layout, responsive/mobile tweaks, animations
- `js/utils.js` — PRNG, shuffle, DOM helpers, storage helpers
- `js/game.js` — Core rules and game state
- `js/ui.js` — Rendering and interactions
- `js/main.js` — Bootstraps UI

## Accessibility

- Keyboard: tab to cards, Enter/Space to select; buttons are reachable and labeled
- Visible focus styles; live region for log updates
- Respects `prefers-reduced-motion` (reduces flip animations)

## Implementation Notes

- Avoid restriction enforced (cannot avoid twice)
- Potion limit per room enforced (extras are auto-discarded)
- Non-increasing weapon rule enforced; blocked hits deal full damage
- Carry is implicit (the unchosen 4th card); HUD carry panel removed to reduce clutter
- Killer card recorded and displayed on death
- No “Restart” button — the game auto-resumes; use “New Game” to start fresh

## Development

No build step. Edit files and reload.

## Assets

- Card backs: `assets/deck.jpg`
- Hearts: `assets/heart.jpg`
- Clubs/Spades: tiered art by value (e.g., `assets/club-1.jpg`, `club-2.jpg`, `club-3.jpg`)
- Diamonds: tiered art by ranges (e.g., `assets/diamond-1.jpg`, `-2.jpg`, `-3.jpg`)

## License

This project recreates rules described openly and contains only original code and assets. No external libraries used.
