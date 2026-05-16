# Shareable Visual Assets

Project home for The Athletic's shareable visual content formats — player cards, stat blocks, quote cards — designed to feel compelling as standalone units in a share sheet, with deep links back to source content.

## Status
Player card prototype built. Awaiting refinement based on prior feedback round.
Power Rankings card prototype v2 built (May 15, 2026) — ready for second review. v1 backup preserved as `athletic-power-rankings-prototype-v1.html`.

## Source of truth
Comprehensive project context lives in auto-memory: `project_shareable_assets.md`. That file holds the strategy, success metrics, design direction, key decisions, prototype details, parallel tracks (Power Rankings card), and open questions. Read it at the start of any session in this folder.

## What's in this folder
- `index.html` — landing page that redirects to the current Power Rankings prototype, also lists the v1 archive and the player card prototype.
- `athletic-power-rankings-prototype.html` — Power Rankings card prototype (current). All 30 MLB teams; mix of player action shots and stadium fallbacks; conditional emphasis for big movers; share flow → iMessage compose → received → recipient article landing.
- `athletic-power-rankings-prototype-v1.html` — Power Rankings prototype v1 (archived).
- `athletic-player-card-prototype.html` — Player card prototype (separate format).
- `prototype-assets/` — player photos + stadium photos sourced from Wikimedia Commons.

## Live deployment
Hosted on GitHub Pages at **https://walkerfullerton-nyt.github.io/power-rankings-prototype/**

Repo: https://github.com/walkerfullerton-nyt/power-rankings-prototype (public)

To push edits live:
```
cd ~/Documents/claude-code-work/shareable-assets
git add .
git commit -m "what changed"
git push
```
Pages rebuilds automatically in ~30–60 seconds. Or just ask Claude to "deploy" or "push" — the working directory is already a git repo with origin set.

To view locally instead (e.g. while iterating):
```
cd ~/Documents/claude-code-work/shareable-assets
python3 -m http.server 8765
# then open http://localhost:8765/
```

## Working principles
- Frame every design choice against two questions: (1) Does this feel visually compelling as a standalone unit? (2) Does the design make sharing feel obvious and natural?
- Primary platform is app (iOS/Android); web should be supportable.
- Success is measured by share rate lift and net new weekly registered users from share channels.

## QA validation rule (IMPORTANT)
Both prototypes render the same card data in **multiple states/screens** with different sizes. A change to one state can look broken in another because each scaled-down state has its own CSS overrides.

For the Power Rankings prototype, every visual change must be validated in **all four states**:
1. **In-article card** — full size, in the article scroll
2. **Share-sheet compact card** — `.sheet-mini` scaling (rank ~52px, photo 145px tall)
3. **iMessage bubble card** — `.bubble-card` scaling (rank ~40px, photo 110px tall)
4. **Recipient article landing** — full size, scrolled-to position with highlight

For the player card prototype, validate equivalents (article → share sheet → iMessage → recipient).

**Process:** after any CSS or template change, take a screenshot in each affected state via Chrome DevTools MCP and confirm the change reads correctly. Don't just verify the state where the change was made.

When sizes need to differ across states (e.g. a chip that's 9px in-article but should be 6px in iMessage), add explicit `.sheet-mini .X` and `.bubble-card .X` overrides — don't rely on a single base style cascading correctly.

## Parallel track
Power Rankings card (per-team shareable card embedded in ranking articles). No prototype yet — blocked on accessing article format details.
