# Resume Notes — Power Rankings Card Prototype v2

**Last updated:** 2026-05-15, mid-revision

This file is for picking up the work mid-stream after losing wifi or starting a fresh Claude session. Read this + `CLAUDE.md` + auto-memory `project_shareable_assets.md` to be fully grounded.

---

## Where we are

**v1 prototype is complete and works** (`athletic-power-rankings-prototype.html` on disk, viewable via local server). Walker reviewed it and gave 8 pieces of feedback. We were in the middle of executing v2 changes when wifi was about to cut.

To view v1 in any browser:
```
cd ~/Documents/claude-code-work/shareable-assets
python3 -m http.server 8765
```
Then open: http://localhost:8765/athletic-power-rankings-prototype.html

(A python3 http server may already be running on port 8765 from earlier in the session.)

---

## v2 feedback (locked, all confirmed by Walker, work in progress)

1. **Logo fallback isn't working** → use a photo for every card. Player action shot when the article names a clear player; stadium photo otherwise.
2. **Photo cropping is off** → tighten cropping. Specifically the Murakami photo on the White Sox card is in his Yakult Swallows uniform — confusing because the article's editorial premise is fictional-future where he plays for the White Sox. Replace with Rate Field stadium photo or the actual NYT Athletic article banner image.
3. **Show all 30 teams** on the page (currently only 6 cards are built). Use a JS data array + render function rather than 30 hand-written card HTML blocks.
4. **Pink swath → team color** — replace pink accents (insight band, share icon hover, highlight border, headline label, stat callout em) with a per-card `--team-color` CSS custom property set inline per card.
5. **Big-mover callout band on the in-article card** — for cards with movement of ±3 or more, add a prominent "Up X spots" / "Down X spots" callout band between the photo and the headline (mirroring the share-state stat callout treatment that Walker liked).
6. **"Share free" label next to the share icon** — communicate that the recipient gets open access. Small pill with both the icon and "Share free" text in upper-right of each card.
7. **Preserve the movement message in the iMessage bubble** — when the card downsizes for messaging, the "Up 6 spots" story is currently getting lost.
8. **Make the Send button work in the iMessage flow** — currently dead. Tap Send should flow into a "received message" perspective (Jamie's phone, incoming bubble), then click-through-to-article from there.

---

## What's already done

- v1 prototype HTML built (in working state — pink theming, 6 cards, share flow works end-to-end)
- 4 player photos sourced and saved locally to `prototype-assets/`: `acuna.png`, `ohtani.jpg`, `misiorowski.jpg`, `murakami.jpg`
- All 30 teams' article data extracted and stored in raw form (rank, prev rank, record, headline, body excerpt, byline) — see chrome-devtools snapshot file under `~/.claude/projects/-Users-walkerfullerton/<session>/tool-results/` if needed; or just re-fetch from the article via the Chrome DevTools MCP. **Source article:** https://www.nytimes.com/athletic/7264166/2026/05/12/mlb-power-rankings-week-7/
- Permissions allowlist updated in `~/.claude/settings.local.json` to enable autonomous work: `Bash(curl *)`, `WebFetch(domain:upload.wikimedia.org)`, `mcp__chrome-devtools__take_snapshot`, `mcp__chrome-devtools__wait_for`

---

## What was in flight when wifi was about to cut

- **Photo sourcing: COMPLETE** — all 27 stadium photos successfully downloaded to `prototype-assets/`. Combined with the 4 player photos already there, **every one of the 30 teams now has a photo asset.**
- **CSS rework not started yet** — was about to begin v2 build but reverted partial CSS change to keep file in working state. v1 prototype is intact and viewable.

## Photo asset map (every team covered)

Player action shots (top-of-rankings editorial showcases):
- `acuna.png` → Braves (#1)
- `ohtani.jpg` → Dodgers (#3)
- `misiorowski.jpg` → Brewers (#5)
- `murakami.jpg` → ⚠️ unusable (Yakult Swallows uniform — confusing). USE `whitesox.jpg` for White Sox card instead.

Stadium photos (all other teams):
- `yankees.jpg`, `cubs.jpg`, `rays.jpg`, `pirates.jpg`, `padres.jpg`
- `tigers.jpg`, `mariners.jpg`, `cardinals.jpg`, `rangers.jpg`, `athletics.jpg`
- `guardians.jpg`, `bluejays.jpg`, `marlins.jpg`, `redsox.jpg`, `whitesox.jpg`
- `diamondbacks.jpg`, `royals.jpg`, `reds.jpg`, `twins.jpg`, `nationals.jpg`
- `phillies.jpg`, `angels.jpg`, `astros.jpg`, `mets.jpg`, `orioles.jpg`
- `rockies.jpg`, `giants.jpg`

---

## Resume next steps

When you pick this up again:

1. **Check `prototype-assets/` folder** — see which stadium photos succeeded. If most are missing, relaunch the subagent (Bash(curl *) is already allowlisted now). Stadiums needed listed above.
2. **Decide build approach for v2:** the cleanest path is a full rewrite of `athletic-power-rankings-prototype.html` using a JS data array + render function (since 30 cards × hand-coding is too much). Keep v1 file as backup or rename to `_v1.html`.
3. **Build the data array.** All 30 team entries with: rank, prev rank, team name, record, avg vote, editorial headline, body excerpt, byline, photo path, primary team color, secondary team color.
4. **Update CSS for `--team-color` theming** — replace `var(--pink)` references in `.rc-headline`, `.rc-headline-label`, `.rc-share:hover`, `.rank-card.highlight`, `.sheet-mini .rc-stat-num em` with `var(--team-color)`. Set `--team-color` inline per card.
5. **Add new components:**
   - `.rc-mover-band` — full-width band, shown only when |movement| >= 3, with team-tinted background, large "▲ Up X spots" / "▼ Down X spots" text
   - `.rc-share-cluster` — pill with "Share free" label + share arrow icon, replacing the bare circular icon
6. **Update iMessage flow:**
   - Add a `screen-imsg-received` separate from `screen-imsg-sent`
   - Send button onclick → switch to received perspective with bubble on left side (incoming)
   - Movement chip / stat callout preserved in bubble
7. **Verify in Chrome via DevTools MCP** — server should still be running on port 8765, just navigate to the prototype URL again.

---

## Useful shortcuts

- View prototype: http://localhost:8765/athletic-power-rankings-prototype.html
- Restart server if needed: `cd ~/Documents/claude-code-work/shareable-assets && python3 -m http.server 8765`
- Source article: https://www.nytimes.com/athletic/7264166/2026/05/12/mlb-power-rankings-week-7/
- Photos folder: `~/Documents/claude-code-work/shareable-assets/prototype-assets/`
