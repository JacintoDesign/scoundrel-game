# Scoundrel (Web)

A polished, accessible, mobile-first web version of the solo card game Scoundrel. Runs fully in the browser with no build step.

## Play

Open `index.html` in any modern browser.

## Rules Summary

- Health: start at 20 (max 20). Clear the dungeon to win; if health ≤ 0, you lose.
- Deck setup (44 cards total):
  - Monsters: all clubs/spades (♣/♠): 2–10, J=11, Q=12, K=13, A=14 (26 cards)
  - Weapons: diamonds (♦) 2–10 (9 cards)
  - Potions: hearts (♥) 2–10 (9 cards)
  - Remove red face cards (J/Q/K ♥♦) and red Aces (A♥, A♦) from a standard deck
- Room: draw until 4 cards are visible.
  - Avoid: put all 4 on bottom (cannot avoid twice in a row).
  - Face: choose 3 to resolve in any order; the 4th carries to next room.
- Resolving:
  - Weapon (♦): equip; discard old weapon and its stack. Resets last-defeated gate.
  - Potion (♥): heal by its value (cap 20). Only one potion per room; extras are discarded.
  - Monster (♣/♠):
    - Bare-handed: take full value as damage.
    - With weapon: damage = monster − weapon (min 0). If damage ≤ 0, monster is defeated and stacked; update weapon’s lastDefeated.
    - Non-increasing gate: you can only use the weapon if the monster's value ≤ the last defeated monster (or any monster if none defeated yet).
- End & Score:
  - Win by clearing the deck → score = remaining health.
  - Lose if health ≤ 0 → score = negative sum of remaining monster values in deck/room/carry.

## Files

- `index.html` — layout: HUD, room grid, action log, modals.
- `css/styles.css` — responsive design, color variables, animations, accessibility styles.
- `js/utils.js` — helpers: PRNG, Fisher–Yates shuffle, DOM utils, storage.
- `js/game.js` — core rules and game state model.
- `js/ui.js` — rendering and interactions.
- `js/main.js` — bootstraps the app.

## Accessibility

- Keyboard operable: tab to cards, Enter/Space to select, button to face.
- Visible focus states; ARIA live regions for the log.
- Honors `prefers-reduced-motion` for reduced flip animation.

## Notes on Rules Implementation

- Avoid restriction is enforced: cannot avoid twice in a row.
- Potion restriction: only one potion per room provides healing; others are auto-discarded.
- Non-increasing weapon rule: weapon can only be used on monsters with value ≤ last defeated with that weapon. If blocked, you take full damage.
- Carry: the unchosen 4th card is carried to the next room as the first card.
- Persistence: game state is saved to LocalStorage after each action to support Restart.

### Worked Examples (Weapon Gate)

- Suppose you equip ♦7. You defeat a 7 (damage 0), lastDefeated = 7. You can next use the weapon on monsters ≤ 7. If you try a 9 next, the gate blocks; you take full 9 damage.
- Equip ♦9, then you hit a 10 taking 1 damage (not defeated). Since no defeat occurred, the lastDefeated remains unchanged; the non-increasing gate continues to allow any value until your first defeat with this weapon.

## Development

No build step. Edit files and reload.

## License

This project recreates rules described openly and contains only original code and assets. No external libraries used.
