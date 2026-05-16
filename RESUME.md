# Resume Notes — Power Rankings Card Prototype

**Last updated:** 2026-05-15 (end of v3 build + GitHub deploy)

Read this + `CLAUDE.md` + auto-memory `project_shareable_assets.md` to be fully grounded when picking the work back up.

---

## Where things stand

**Status:** v3 prototype is built, deployed, and waiting on stakeholder feedback. v1 (initial build) and v2 (8 feedback items) are archived.

**Live URL (share with stakeholders):**
https://walkerfullerton-nyt.github.io/power-rankings-prototype/

**Source repo (public):**
https://github.com/walkerfullerton-nyt/power-rankings-prototype

**Local working dir:** `~/Documents/claude-code-work/shareable-assets/` (this folder)

---

## How to resume editing

In any new Claude session, just open this folder and ask Claude to read this file and `CLAUDE.md`. Then describe what stakeholders said. Claude can:
- Edit any of the prototype files
- Verify changes in the local browser via Chrome DevTools MCP
- `git add . && git commit -m "..." && git push` to deploy live (~30s rebuild)
- Source new photos via subagent if needed

**Local preview while iterating:**
```
cd ~/Documents/claude-code-work/shareable-assets
python3 -m http.server 8765
# then open http://localhost:8765/
```

**Push to live site:**
```
cd ~/Documents/claude-code-work/shareable-assets
git add .
git commit -m "describe what changed"
git push
```

---

## What's in v3 (the current prototype)

### Layout (all 30 MLB teams rendered from a JS data array)

**In-article card** (each team):
- Hero photo zone (175px tall) with team-color accent stripe on the left edge
- Top-left: subtle "MLB POWER RANKING · WK 7" context label (compact + bubble states only)
- Bottom-left: massive `#N` rank with the movement chip immediately to its right
- Bottom-right: team name + `record · Last week #N`
- Top-right: "SHARE FREE" pill with arrow icon — clicks to open share sheet
- Big-mover band (only for ±3 or more): full-width "Up/Down X spots" callout between photo and headline, team-color tinted, pulse animation
- Editorial headline band ("ONE REASON TO BELIEVE" label + the writer's hook)
- Body: ~2 paragraphs of editorial commentary
- Footer: byline + The Athletic brand mark

**Share-sheet compact card:**
- Smaller scale (`.sheet-mini` overrides)
- Same elements as in-article EXCEPT no body — instead a 28-word teaser ending in "Read more →"
- "MLB POWER RANKING · WK 7" context label appears top-left

**iMessage bubble card:**
- Even smaller scale (`.bubble-card` overrides)
- Right-aligned bubble in compose state (sender perspective), left-aligned in received state (Jamie's perspective)
- "JAMIE'S PHONE — INCOMING" annotation in received state to make perspective explicit
- Tap card → recipient article landing

**Recipient article landing:**
- Green "Walker shared this with you / Read free, no subscription needed" banner at top
- Full article rendered (all 30 cards)
- Anchor-scrolls to the shared card (highlighted with team-color glow)
- No registration prompt at this stage — registration appears later in the flow

### Movement chip styling (locked decisions)

- Up: `▲ N` green pill
- Down: `▼ N` red pill
- No change: stacked `NO / CHANGE` mini pill (vertical stack to keep it narrow)
- Big movers (|m| ≥ 3): the chip gets a subtle pulse animation; the card also gets a team-color highlight border AND the prominent "Up/Down X spots" callout band below the photo
- Sizing differs across states:
  - In-article: 9px font, full text
  - Share-sheet: 7px font (`.sheet-mini` override)
  - iMessage bubble: 6px font (`.bubble-card` override)

### Photo strategy

- 30 player photos in `prototype-assets/` (3 named-player action shots from earlier — Acuña, Ohtani, Misiorowski — and 27 sourced via subagent)
- Per-team `photoPos` data field allows manual `object-position` override (Acuña uses `'center 6%'` to keep his face in frame)
- All other cards use aspect-aware auto-tune: `tunePhotoPos()` runs onload and picks an `object-position` band based on portrait/landscape ratio
- 27 stadium photos also remain in `prototype-assets/` (suffix-less filenames) — kept as available fallbacks but not used in v3

### Team-color theming

- Every card sets `--team-color` and `--team-color-2` inline
- `pickAccent()` chooses whichever of primary/secondary has higher luminance, then `brightenIfDark()` boosts it further if still too dark on the dark card background (so navy/black teams like Yankees, Tigers, White Sox still get a readable accent)
- Used by: insight band background+border, headline label color, share-cluster hover, highlight glow on big movers, stat callout em color

---

## QA validation rule (from CLAUDE.md — important)

Any visual change must be verified in **all four states** before reporting done:
1. In-article card
2. Share-sheet compact card
3. iMessage bubble card
4. Recipient article landing

Why: each state has its own CSS overrides (`.sheet-mini` and `.bubble-card`). A change to the base style can look fine in the article but break in the bubble. Take a screenshot in each state via Chrome DevTools MCP after every change.

---

## Open items / known limitations

- **Photo cropping:** auto-tune is good enough for the prototype but not perfect. Some cards may need per-team `photoPos` overrides on stakeholder review (just like Acuña has). Easy fix: add `photoPos: 'center N%'` to the team's data entry.
- **Yankees photo:** Aaron Judge's only Wikipedia infobox photo is "posing with Donald Trump" — politically loaded for a stakeholder mockup. We're using Cody Bellinger as the Yankees face instead.
- **White Sox photo:** the article's editorial premise has Munetaka Murakami on the White Sox in fictional 2026, but his only real-world photos are in his Yakult Swallows uniform. Using Luis Robert Jr. instead (Wikimedia infobox photo).
- **Two photos came down as PNGs with `.jpg` extension** (Royals/Witt, Twins/Buxton) because Wikipedia served the original. They render fine in browsers; if a true `.jpg` is needed for production, `sips` re-encode would handle it.

---

## Recent feedback rounds

**v1 → v2 (8 changes):**
1. Photo for every card (no monogram fallback)
2. Murakami photo problem fixed (was Yakult Swallows uniform)
3. All 30 teams rendered
4. Pink swath replaced with team-color accents
5. Big-mover callout band on in-article cards
6. "Share free" label next to share icon
7. Movement preserved in iMessage bubble
8. Send button works → received state → click-through to article

**v2 → v3 (5 changes):**
1. Rank label: `#` prefix + "MLB POWER RANKING · WK 7" context label in compact states
2. Body text lengthened (~2 paragraphs); compact state truncates to 28-word teaser + "Read more →"
3. Photo cropping audited; aspect-aware auto-tune added
4. All stadiums replaced with player photos
5. Recipient view: full article + anchor-scroll, registration prompt removed

**v3 polish:**
- Share-sheet "Free for the recipient" → "Free to read"
- "No change" chip: large text → stacked "NO / CHANGE" mini pill
- Movement chip moved next to rank number (was being pushed right by long context label)
- iMessage bubble chip sized down via `.bubble-card .rc-move.steady` override
- CLAUDE.md gained the QA-validation rule

---

## Useful shortcuts

- **Live site:** https://walkerfullerton-nyt.github.io/power-rankings-prototype/
- **Repo:** https://github.com/walkerfullerton-nyt/power-rankings-prototype
- **Local server:** `python3 -m http.server 8765` then http://localhost:8765/
- **Source article (the editorial reference):** https://www.nytimes.com/athletic/7264166/2026/05/12/mlb-power-rankings-week-7/
- **Photos folder:** `~/Documents/claude-code-work/shareable-assets/prototype-assets/`
- **Reference player card prototype:** `athletic-player-card-prototype.html` (predates this work; same family of formats)
