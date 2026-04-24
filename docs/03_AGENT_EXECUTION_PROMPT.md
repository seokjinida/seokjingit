# Agent Execution Prompt - Neon Pop Sweeper (v1.1)

아래 지시를 그대로 따르세요.

## Objective
Build a fully playable "Neon Pop Sweeper (Dynamic Vision)" prototype as a single `index.html` file using only HTML/CSS/Vanilla JavaScript and Canvas 2D API.

## Source of Truth
- Spec: `docs/01_GAME_SPEC.md`
- Rules: `docs/02_IMPLEMENTATION_RULES.md`

If any text conflicts, prioritize numeric constants and formulas from the spec.

## Hard Constraints
1. Output file must be exactly one file: `index.html`
2. No external libraries, assets, fonts, or network calls
3. Session must end at exactly `70000ms` (`performance.now()` based)
4. Map must be `2000x2000` and camera must never show outside map bounds
5. 1 player + 3 AI bots, no combat/penalty overlap
6. Show a start panel first, then begin on `Start Game`
7. Support both pointer joystick and keyboard (`WASD` + arrow keys)

## Must-Have Runtime Behavior
- Square neon dots with spawn maintenance and pickup scoring
- Character growth by combo (current tuned multiplier from spec)
- Dynamic camera zoom with smooth lerp and clamp
- Combo chain text in top-center UI (0.6 second chain window)
- Combo text color tier + size tier progression
- Real-time ranking board + final ranking modal + restart
- Pickup particles from dot position (not actor center)
- No pickup-triggered screen shake in current build

## Implementation Order
1. Canvas boot + resize + loop timing
2. Constants/state + utility helpers
3. Start panel / result panel flow
4. Input systems (pointer joystick + keyboard)
5. Actor movement + AI steering logic
6. Dot spawning + collision collection
7. Score/combo/fever/chain logic
8. Camera zoom and world clamping
9. Rendering layers (world, dots, actors, UI, combo text)
10. Restart and quality fallback handling

## Delivery Quality
- Opens and runs directly in browser
- No TODO/FIXME placeholders
- Pointer cancel and focus/blur edge cases handled
- Restart works repeatedly without broken state

## Acceptance Checklist
- [ ] Start panel appears before gameplay and Start button begins run
- [ ] Pointer joystick + WASD/arrow controls both function
- [ ] Dots are rendered as squares and pickups score correctly
- [ ] Combo chain resets when 0.6 second passes without pickup
- [ ] Combo text appears in top center with color/size tiering
- [ ] Session ends exactly at 70 seconds and final ranking is shown
- [ ] Restart restores a clean initial state
