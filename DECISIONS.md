# DECISIONS.md — Wireframe prototype build log

Double Diamond "Develop" phase artifact. Structure & flow only. Every fork
resolved in-place; rationale one line each.

## Tech / delivery
- **Single standalone `index.html`, React + Babel via CDN, no build step.** Rationale: a wireframe for a course must open with a double-click; zero install beats a Vite project.
- **State-based router with a history stack (not react-router).** Rationale: keeps everything in one file, gives real back-navigation without a dependency.
- **Dashboard is the router root.** `nav.go("dashboard")` resets the stack; every other screen pushes. Rationale: dashboard is home base now that onboarding is gone; guarantees "Back home" always lands clean with no stale back-stack.

## Low-fi style
- **Kaomoji/ASCII plant faces (`^‿^`, `>_<`, `O_O`), not colored emoji.** Rationale: brief demands grayscale + one accent; ASCII reads more "wireframe".
- **Accent `#4A7C59` reserved for: primary buttons, active/selected state, key status dot, "N new" pill.** Rationale: brief says "sparingly". No bottom tab bar to accent anymore.
- **Photo/chart placeholders as dashed diagonally-striped boxes `[PHOTO]` / `[GROWTH CHART]`.** Rationale: signals unfinished content, matches brief.
- **System font, dashed borders, no shadows/gradients** (the one push-banner shadow is deliberate — a simulated OS notification floating above the app). Rationale: must not read as finished UI.

## Emotion enum → face + EN status label
content=All good, thirsty=Thirsty, overwatered=Overwatered, low-light=Needs light,
sleeping=Sleeping, greeting=Hi there!, cold=Cold, hot=Too hot, urgent=Urgent!, neutral=Okay.
Rationale: labels stay warm/short, one per enum value.

---

# Revision 2 — forks resolved this pass

## Global
- **App name removed everywhere → grey dashed `[APP NAME]` placeholder chip** (`.brand`). Rationale: name is undecided; a visibly-placeholder chip reads as "TBD" rather than a real brand, and lives only in true branding slots (dashboard header, profile footer).
- **All UI copy → English, warm/character-first tone.** Rationale: brief. Plant names transliterated: Felix (ficus, friendly), Margot the Monstera (drama queen), Gosha the Cactus (grump). Added Vera the Aloe (calm) + Basil (cheerful) — see plant-grid note.
- **Deleted the welcome screen; app opens directly on the Dashboard.** Rationale: brief; the get-acquainted / "I already have a pot" fork added a click with no insight.
- **Removed the bottom tab bar entirely.** Settings → a Profile screen reached from the avatar in the dashboard header; Chat reached from the header. Rationale: brief.

## Chat entry point (the "and/or" call)
- **Chose BOTH: a speech-bubble icon in the dashboard header AND a chat preview card lower on the dashboard.** Rationale: they do different jobs — the header icon is a persistent, predictable entry (mirrors the removed tab, always reachable regardless of scroll); the preview card surfaces the app's personality by showing the latest message, which is the reason to open chat in the first place. Keeping both costs little and each earns its place.

## Dashboard
- **Merged "overall health" + the encouragement summary into ONE top block.** Contains: "You're doing great", a `72% healthy` pill, a health bar, the 2/3-happy line, and the streak folded in as **"7 days, no incidents 🎉"**. Rationale: brief (5, 6); one warm block beats a stat readout.
- **Deleted the row of 4 stat cards.** Rationale: brief (6); the numbers either moved into the summary block or weren't worth the space at wireframe stage.
- **My plants = 2×2 grid, horizontally swipeable via CSS scroll-snap, with page dots.** First tile is a dashed **"+" add card**. Each plant tile is a **PHOTO placeholder as the avatar**, with the **emotion glyph in a small circle badge at the photo's top-right** — never the glyph as the avatar. Rationale: brief (7, 8). Added 2 plants (5 total) so there are 2 pages and the paging is actually demonstrable.
- **Today's tasks: checking a task sinks it to the bottom** (stable re-sort, done-items last, with a fade). Removed the plant avatars from the right of each row; kept the plant name as a small caption. Rationale: brief (9). Checking still routes through the plant-reaction beat before the task settles as done.

## Chat
- **Removed the "they chat with each other" toggle from the chat header.** It now lives only in Chat settings; the chat screen just reflects the setting. Rationale: brief (10). Added a Back button to the chat header since the tab bar is gone.

## Add-new-plant flow (expanded to 3 explicit steps + first meeting)
- **Step 1 = Bluetooth pairing only, no photo.** Interactive state machine: idle → searching (pulse) → pot found → connecting (spinner) → connected. Auto-advances on timers to feel real. Rationale: brief (11).
- **Step 2 = photo capture in detail:** camera-framing screen → processing (spinner + animated progress bar, ~2s) → best-match species list loading in. Rationale: brief (12).
- **Fixed "Choose species manually":** now opens a searchable, scrollable species list (`SpeciesList`) with a live-filter search field, reachable from both the framing screen and the results screen. Previously it jumped straight to character setup — a dead fork. Rationale: brief (13).
- **Avatars throughout the flow are PHOTO placeholders, never emotion glyphs** (pairing, capture, species, create, first meeting all use `Photo`). Extended the same principle to Plant detail, Growth, and Notifications for consistency (photo = avatar, emotion = pill/badge). Rationale: brief (14) + coherence.
- **Personalities expanded to 8, each with a one-line description:** Friendly, Drama queen, Grump, Calm, Cheerful, Shy, Sassy, Wise. Rationale: brief (15). These also feed the per-plant Personality picker.
- **Voice options get a play button + mock playback:** tapping ▶ shows an animated equalizer + "Playing a '…' sample" line; only one plays at a time. Rationale: brief (16).
- **Renamed the final button "Bring Felix to life" → "Say hello 👋".** Rationale: brief (17); "Say hello" is the warmest and most literal of the options (you're greeting a new friend), and stays true even before the plant is named.

## Settings → Profile
- **Settings now live inside a Profile screen** (reached from the header avatar). Rows: Plants & characters, Add a new plant, Chat & reactions, Language. Rationale: brief (4, 18).
- **Removed "Notifications" and "Redo onboarding" rows.** Rationale: brief (18); onboarding-as-a-screen no longer exists, and notification prefs fold into per-plant quiet hours.
- **Added Language settings: two separate controls — Interface language and Conversation language** (the language plants talk in). Rationale: brief (19).
- **Moved the Telegram integration inside Chat settings** (removed from the top-level settings list). Rationale: brief (20); Telegram is a chat-delivery channel, so it belongs with chat.

---

# Revision 3 — forks resolved this pass

## Dashboard
- **Removed the "7 days, no incidents 🎉" streak line** from the summary banner; it now holds only overall health (bar + %) + the encouragement line. Rationale: brief (1).
- **"My plants" blank-cards bug = Safari flex/grid/scroll-snap collapse.** A `display:grid` page nested as a `flex:0 0 100%` child under `scroll-snap` resolves its cross-size circularly and collapses to 0 height in Safari (the likely open target on macOS). Fix: pin the page to a fixed width (`358px` = phone content width) and give both `.pager` and `.page` explicit heights (`grid-auto-rows:150px`, `min-height`). Rationale: brief (2); removes the circular height dependency entirely rather than guessing at the trigger.
- **Chat preview → true miniature chat widget.** Replaced the one-line link with a scaled-down reproduction of the last 3 messages: small round photo avatars + bubbles (accent for "me", right-aligned), sender name captions, "3 new" pill. Whole card is tappable → full chat. Rationale: brief (3).

## Plant detail
- **Removed the "Chat with {plant}" button.** Chat stays reachable from the dashboard header + preview. Rationale: brief (4); also updated the tour copy that implied chat-from-plant.
- **Added an Auto-watering card:** on/off toggle, reservoir level bar (turns grey + "running low" under 25%, e.g. Margot at 12%), Schedule segmented control (2/3/5 days), Amount segmented control (Low/Med/High), and a "Mark reservoir refilled" button that routes through the plant-reaction beat. Schedule/Amount dim when auto is off. Rationale: brief (5, 7). Low-fi segmented chips, no real time pickers at wireframe stage.
- **Added a Bond card:** heart level (♥×5), level number + title ("Close friends"), and a % progress bar to the next level. New per-plant fields `bondLevel/bondPct/bondTitle`. Ties to the existing "+N to your bond" reaction beats. Rationale: brief (6).

## Global copy — watering → refill the reservoir
- **Every user-facing watering ACTION now says "refill the reservoir".** Tasks ("Refill Felix's reservoir"), the low-water alert ("Margot's reservoir is running low"), chat ("refilling Margot's reservoir right now" / "your reservoir got topped up yesterday"), Gosha's check-in/reaction ("Don't top me up" / "don't overfill it"), plant-detail stat ("Last refill"), and the diagnosis step ("Pause auto-watering for 5–7 days"). Rationale: brief (7).
- **Kept as-is:** the `thirsty` and `overwatered` emotion STATES, "Amount per watering" (a label for the pot's own automatic watering), and factual soil-moisture readings. Rationale: brief says the thirsty state stays; only the implied user action changes, and these describe state/mechanism, not a hand-watering action.

## Profile
- **"Plants & characters" list bug fixed + new PlantsList screen.** The row used to jump straight to Felix's card (not a list). It now opens `plantsList`, which renders all 5 plants (photo avatar + emotion badge + species·persona); tapping one opens its card → settings. Reachable, with Back + an "+ Add" in the header. Rationale: brief (8).
- **Renamed the section to "Plants"** and added a **duplicate "+ Add plant"** button beside the section heading (kept the standalone "Add a new plant" row too — the brief explicitly asked for a duplicate entry point). Both start the pairing flow. Rationale: brief (9).
- **Profile heading = user's name.** Replaced "Your garden" with a placeholder name ("Alex Rivera", `USER_NAME`, "from sign-up / social login") next to the existing avatar. Left the dashboard's "Your garden" title untouched — brief scoped this to the profile. Rationale: brief (10).

---

# Revision 4 — forks resolved this pass

## Dashboard
- **Order is now: summary → Today's tasks → My plants → Chat.** Tasks moved above the plants grid. Rationale: brief (1).
- **The big "+" grid tile is gone; "add plant" is now a small pill button in the "My plants" header** (`.addmini`). Plants get all four grid cells (page 1 = Felix/Margot/Gosha/Vera, page 2 = Basil). Rationale: brief (2); the full-cell + tile ate a plant's worth of space — a header button is noticeably smaller and still an obvious entry point.
- **Tour-replay bug fixed with a session-level `tourSeen` flag in App state.** Dashboard remounts on every return home (it's the router root), so component-local state couldn't remember the tour had shown. Now `tour = params.tour && !tourSeen`, and a mount effect + the dismiss handler both set `tourSeen`. Shows once, never again in-session. Rationale: brief (3).

## Plant detail
- **Removed the "Mark reservoir refilled" button; the reservoir is now a pure level indicator** (label + big %, then the bar, + a "getting low" hint under 25%). Rationale: brief (4); a wireframe shouldn't imply a working refill action, and the level is the useful thing to show.
- **"Last refill" → "Last watering"** with a date/time (`lastWatered`, e.g. "Today, 07:00") = when the pot last auto-watered. Dropped `refillAgo`. Rationale: brief (5).
- **"Growth diary" + "Growth progress" merged into one "Growth & diary" entry** → the `Growth` screen now shows progress *and* the diary timeline; deleted the standalone `Diary` component + `diary` route. Rationale: brief (6).

## Plant settings
- **Plant name is the screen title at the very top** (kicker demoted to "Plant settings"). Rationale: brief (7).
- **Added a photo card at the top** (round photo + emotion badge + "Change photo" button) to view/replace the add-flow photo; kept the single Name field just below. Rationale: brief (8).

## Add-new-plant flow
- **New `SpeciesDetail` screen** (species info: reference photos + care-at-a-glance rows) with a **"This is my plant"** confirm → `create`, and a "keep looking" back-out. Reached from BOTH the suggestions rows and the manual search list. Rationale: brief (9).
- **Suggestions rows now have two paths:** tapping the left region opens `SpeciesDetail`; a dedicated **"Select" button** confirms straight to `create` without opening details. Manual list rows open the detail screen. Rationale: brief (10).

---

# Revision 5 — forks resolved this pass

Re-applied against the original full brief; two earlier revisions had drifted the
opposite way on two points, so these are deliberate reversals of Rev 3/4.

## Dashboard
- **Streak line folded back into the summary block** as a playful line: "🎉 7 days, no incidents — keep it up.", sitting under the health bar. Rationale: brief (6) explicitly asks for the streak folded into the summary; this reverses Rev 3's removal. The row of 4 stat cards stays deleted (also brief 6).
- **"+" add card restored as the FIRST cell of the My plants grid** (dashed card, big `+`, "Add plant"), starting the pairing flow (`nav.go("pair")`); removed the `.addmini` header pill it had been swapped for. Rationale: brief (8) explicitly wants the first grid card to be a "+" card; this reverses Rev 4. Items now chunk as `[+add, Felix, Margot, Gosha] / [Vera, Basil]` → 2 pages, paging still demonstrable, plants still get near-full grid use.

---

# Revision 6 — forks resolved this pass

## Regressions
- **Removed the "7 days, no incidents" streak line** from the summary banner; it now holds encouragement + overall health (pill + bar) only. Rationale: brief (1). (Directly reverses Rev 5 — the newest instruction wins.)
- **Dashboard header now shows the user's name (`USER_NAME` = "Alex Rivera", placeholder for sign-up/social login) instead of "Your garden"**, with the avatar button already sitting next to it in the header row. Moved `USER_NAME` up into the data block so both the header and Profile share one source. Rationale: brief (2) — the "Your garden" heading with the adjacent avatar is the identity slot the brief means.

## Dashboard
- **Completed tasks collapse into a compact "N done" row** (Show/Hide toggle) even while active tasks remain; active tasks stay expanded above it, finished ones render as short strikethrough rows only when expanded. The Today's-tasks section is always visible. Rationale: brief (3); replaces the old "done sinks to bottom as full rows" behaviour. Un-checking from the expanded group returns a task to active.
- **"+" add control made compact + secondary again** — no longer a full grid cell (it "ate a plant's worth of space"). It's now a short dashed `.addtile` bar sitting *above* the 2×2 plant grid on page 1 (`.page` is a flex column: add-tile + `.pgrid`). Rationale: brief (4). This supersedes Rev 5's full-cell "+" card; "compact secondary tile, not a peer of the plant cards" is best served by taking it out of the equal-size grid entirely while keeping it inside the My-plants section.

## Plant detail card
- **"Your bond" block moved above the Last watering / Light tiles.** Rationale: brief (5).
- **Removed the auto-watering toggle caption** ("On — pot waters itself" / "Off — paused"); label + toggle only. Rationale: brief (6).
- **Removed the "Getting low — top up the reservoir soon" hint**; the % + bar carry it. Rationale: brief (7).
- **Schedule + Amount moved off the card onto a new `WaterConfig` screen**, reached via a "Configure watering" row. The auto-watering card now shows only toggle + reservoir level. The Configure row dims when auto is off. Rationale: brief (8).
- **Removed the grey subtitle under "Growth & diary"** (title + chevron only). Rationale: brief (9).
- **"Plant settings" entry renamed "Plant personality"**, subtitle removed; also updated the target screen's kicker to "Plant personality" for consistency. Rationale: brief (10).

## Alert / emergency flow
- **Diagnosis "What to do" is now individually checkable tasks** styled exactly like the dashboard task list (`.list`/`.li`/`.check`/`.done-txt`). Removed the single "I've done all this" button; the flow auto-advances to the thank-you screen ~0.5s after the last box is ticked. Interpreted "styled exactly like the dashboard list" as the checkbox-row styling, not the collapse behaviour (collapsing ticked recovery steps would hide the user's progress). Rationale: brief (11).

## Chat settings
- **Reordered:** Plants talk to each other → **Message frequency** → Group reactions → Forward to Telegram. Rationale: brief (12).
- **"Message frequency" second line "Currently: moderate" removed**; the "Moderate" pill on the right already carries the value. Rationale: brief (13).
- **"Telegram" row renamed "Forward to Telegram"** (kept its subtitle). Rationale: brief (14).
- **Removed the bottom caption** "Every plant chats in the style of its personality". Rationale: brief (15).

## Add-new-plant — photo step
- **Real camera screen:** the `frame` state now returns a full-bleed dark viewfinder (`.camwrap`, breaks out of the `.screen` padding) with a large round iPhone-style **shutter** centred at the bottom and a **gallery thumbnail** to its left; a "‹ Back" chip up top keeps navigation alive. No stacked text buttons. Rationale: brief (16). Both shutter and gallery advance to processing (gallery isn't a dead control).
- **Lead-in restructured:** pairing's final button "Next: add a photo" → **"Continue"**, which now opens a short **`PhotoIntro`** explainer (why the photo is needed) → "Continue" → the full-screen camera. Rationale: brief (17).
- **"Choose species manually" moved off the camera** entirely; its home is the results screen's "None of these — choose manually" button. Rationale: brief (17).

## Add-new-plant — species info card
- **Reframed from care → identification.** Removed the "Care at a glance" table (light/watering/difficulty/origin/toxicity). Replaced with a "How to identify it" list (leaves shape/edges/venation, colour & texture, growth habit, size at maturity, stem/trunk) plus an **"Often confused with…"** section (3 look-alikes + the tell-tale difference). Rationale: brief (18) — on this screen the user is verifying the species, so identification beats care.
- **Single round photo + separate reference block → one swipeable image carousel** at the top with dot indicators (`.carousel`/`.cslide`, reusing the pager scroll-snap technique). Rationale: brief (19).
- **Kept both "This is my plant" and "Not quite — keep looking" buttons.** Rationale: brief (20).

## Add-new-plant — voice step
- **Removed the "Playing a '…' voice sample" status line** below the voice list; playback state now lives only inside each option's play button (▶ ⇄ animated equalizer), so nothing below shifts. Rationale: brief (21).

---

# Revision 3 — forks resolved this pass

## Global / typography
- **All fonts → Urbanist (Google Fonts), loaded via `<link>` in `<head>`.** Global on `html,body`; added the single hook `input,button,textarea,select{font-family:inherit}` because form controls don't inherit type by default and would otherwise fall back to the UA sans. Removed every other stack: monospace from `.face`/`.badge`, `inherit` from inputs & camera controls, the inline `fontFamily` on the Telegram face, and the bootstrap error-fallback stack → `'Urbanist',sans-serif`. Rationale: brief (16); one family everywhere, no leftover competing stacks.
- **Kaomoji faces now render in Urbanist (monospace removed).** Rationale: acceptable — `^‿^`/`O_O` are ordinary glyphs; keeping a mono stack just to draw them violated the single-family rule.

## Prototype shell (outside the app)
- **Replaced the flat frame with a simplified iPhone: dark body 412×844, 13px bezel padding, 44px inner radius, dynamic-island pill.** Rationale: brief (14); recognisable without being hyper-real, still grayscale.
- **`StatusBar` rendered ONCE in the App shell, above `<Screen>` — not per screen.** Solid-ink SVG cellular/wi-fi/battery + `9:41`. Rationale: brief (14) says it's device chrome, constant on every screen incl. the dark camera view; putting it in the shell guarantees that and keeps it out of the wireframe layer. Push banner nudged `top:32px→56px` to clear it.
- **`FlowIndex` panel is `position:fixed` to the right, NOT a flex sibling.** Rationale: brief (15) "keep the phone centered" — fixed positioning leaves `#root` centering untouched. Grouped by the 5 flows; active item matched by screen name (params ignored) in grey/#4A7C59; scrollable; hidden below 1100px viewport so it never overlaps the centered phone.

## Dashboard
- **"Add plant" moved OUT of the pager into a standalone full-width `.addtile` ABOVE it.** Chose above over below (reads as a primary action before the collection). Rationale: brief (3) — outside the horizontal scroll it can't vanish on page 2 or shift the layout.
- **Removed `<Brand/>` from the dashboard header only; kept the `[APP NAME]` chip in the Profile footer.** Rationale: brief (2) scoped to the header; the footer is still a legitimate "TBD brand" slot.

## Alert / diagnosis
- **`[MOISTURE CHART · 3 DAYS]` → inline SVG line chart** (`MoistureChart`): 8 readings climbing 38→92%, 5 grey gridlines + 2 axes, one #4A7C59 polyline, no fills/dots/gradients, day labels. Wrapped in a card with a "92% now" pill. Rationale: brief (4).
- **Removed the auto-advance `useEffect`; added an explicit sticky-footer button, disabled until all steps ticked.** Its label doubles as the instruction ("Tick each step to continue" → "All done — continue"). Rationale: brief (5); ticking must never navigate on its own.

## Add-new-plant
- **Removed the `[BLUETOOTH]` box (pairing idle).** Rationale: brief (6).
- **"Pot found" card: replaced the status dot with a `[POT]` photo placeholder** (kept the "Found" pill for status). Rationale: brief (7) — makes the found device visually concrete.
- **PhotoIntro: removed the round `[PHOTO]`; heading+text now vertically centered between two spacers.** Rationale: brief (8).
- **Captured-photo placeholder unified to 190px on both processing & results** (was 250/150). Rationale: brief (9) — no frame jump between steps.
- **Species carousel bug = flex-child collapse under scroll-snap (same Safari issue as the plant pager). Fix: `min-height:180px` on `.carousel` — CSS only, no JSX change.** Rationale: brief (10).
- **"Often confused with" cards made tappable → recursive `SpeciesDetail` for that species.** Introduced `speciesInfo(species)` so each species gets its OWN carousel shots + look-alike set (identification copy is shared generic placeholder). Chose recursion over a full per-species DB — keeps it wireframe-light and every look-alike reachable, no dead ends. Rationale: brief (11).
- **Bottom actions pinned via a sticky `.footer`** (`position:sticky;bottom:0;margin-top:auto`) on SpeciesDetail (and reused on Diagnosis). Rationale: brief (12) — visible without scrolling to the end.

## Character & greeting
- **Centered the round `[PLANT PHOTO]` via `margin:0 auto`** on Create-character & First-meeting. Root cause: `.center` is `text-align`, which can't center a fixed-width flex block. Rationale: brief (13).

## Verification
- Validated by transforming `#app-src` through the page's own Babel config (no `import` emitted) and rendering the full app in jsdom: dashboard + all flow-index targets render with **0 console errors**; confirmed chart SVG/polyline, 4 carousel slides, tappable confused→recursive nav, diagnosis button gating (disabled→enabled, no auto-advance), and the "Pot found" photo.

---

# NEW DELIVERABLE — Bloomling landing-page configurator (2026-07-22)

> This is a **separate deliverable** from the mobile-app wireframe logged above.
> The wireframe that previously occupied `index.html` was preserved as
> `index.wireframe-backup.html` before `index.html` was rewritten as the
> configurator. Nothing above was deleted.

Single self-contained `index.html`. No build step, no imports/exports, no external
requests. Deploys to Vercel as-is (drop the file in — it's the site root).

## Tech / delivery
- **Vanilla JS + inline CSS**, NOT React/Babel. Rationale: the interactions are a
  small state machine (2 pickers + 2 sliders + a sleep/wake flag). Vanilla is more
  robust for a "deployable as-is" static file — zero CDN/network dependency, so it
  can't fail to load a script. Everything is inlined.
- Pot is a **CSS-layer + inline-SVG illustration** (no photos): stacked absolutely
  positioned layers for base/bowl/glass/rim/aura; SVG for plant silhouettes + face.

## State model
- Single `state` object `{bowl, plant, warmth, chat, awake}`.
- **No manual sleep/wake toggle.** `awake` is derived from which step the user
  touches: interacting with the Bowl or Plant picker → **sleeping**; touching the
  Personality step (pointerdown / focusin / slider input) → **awake**. Default is
  sleeping so the chosen bowl reads clearly through clear glass. The touched card
  also gets an `.active` highlight; the scene caption echoes the state.

## The pot (layers, bottom → top)
1. `aura` — soft warm outer glow; brightens + scales on wake.
2. `base` — speckled cream-sand stoneware (layered radial-gradient specks + seam).
3. `bowl` — inner ceramic; color driven by the selected variant via CSS vars.
4. `cavity` — dark elliptical opening the plant rises from.
5. `plant` — SVG silhouette, anchored in the bowl, foliage rising *above* the pot
   (crisp) while the stem sits *behind* the glass (frosts on wake).
6. `rim` — sensor rim light (glowing ellipse); dim when asleep, bright + gently
   pulsing when awake.
7. `glass` — PDLC outer shell. **Sleep:** near-transparent, `backdrop-filter:blur(0)`,
   faint reflection streak → bowl visible. **Wake:** translucent warm-white fill +
   `backdrop-filter:blur(7px)` + inner glow. This ~500ms transition is the hero
   moment. `backdrop-filter` degrades gracefully (translucent fill alone still reads
   as frosted where blur is unsupported); `-webkit-` prefix included for Safari.
8. `face` — minimal glowing almond eyes (SVG); hidden asleep, fades/scales in awake.

## Bowl variants (5)
matte white, warm sand, sage, terracotta, charcoal — each a two-stop vertical
gradient (`--bowl-hi`/`--bowl-lo`) for a matte-ceramic read. Chips show a shaded
swatch.

## Plants (5, fixed)
Monstera, Ficus, Succulent, Fern, Basil — each a distinct hand-written SVG
silhouette built by small JS helpers (lobed monstera leaves, teardrop rosette,
feathered fronds, oval-pair basil, stemmed ficus) in a shared green palette.
Cross-fade on swap. No custom text input. Chips reuse the silhouettes as minis.

## Personality (3×3 matrix)
- **Warmth** (3): caring · warm-teasing · playfully sarcastic.
- **Chattiness** (3): quiet · measured · talkative.
- Expression carried by **eye tilt/shape** (per brief): caring = soft upward arcs
  (outer corners up, slight squint); balanced = neutral almonds; sarcastic =
  matched side-tilt + one narrowed eye. Chattiness drives animated "talk" dots
  (0 / 1 slow / 3 fast).
- **9 hardcoded intro phrases**, written in brand voice (warm, a little magical,
  never corporate), selected plant name substituted live (`{p}`). Phrase +
  expression update on every slider move. The brief's example maps to the
  warmth=sarcastic × chat=talkative cell.

## Design
- Cream `#f7f3ec` ground, warm terracotta accent, `#ffd8a1` glow family, rounded
  geometry, generous whitespace. Sleep↔wake transitions 300–500ms.
- Two-column on wide screens (pot sticky on the left, steps scroll on the right);
  single centered column ≤880px. `prefers-reduced-motion` kills idle animation.

## Order CTA
"Pre-order Bloomling" → satisfying pressed micro-state: press-scale, a 14-particle
warm glow burst, button flips to a green "Reserved — we'll be in touch" with a
check + a playful subnote. No real checkout; idempotent (guards on `.done`).

## Verification
- Executed the page in **jsdom** (`scratchpad/test.js`): **0 runtime errors** and
  **24/24 behavior checks pass** — 5+5 chips built; default sleeping; bowl/plant
  picks update labels, apply the bowl color var, and keep the pot asleep with the
  right card active; arrow nav clamps; personality touch wakes the pot; phrase
  substitutes the plant name and matches every one of the 9 matrix cells (all 9
  distinct, no leftover `{p}`); sarcastic narrows one eye; talkative shows 3 dots;
  returning to a picker re-sleeps; CTA reaches the done/Reserved state and emits 14
  particles.

---

# Revision 4 — file recovery, flow-index simplification, deploy

## 0. File safety check (what was found & moved)
- **Found: `index.html` at the project root had been OVERWRITTEN by the configurator** (a different app — "Bloomling — meet your pot", terracotta `--accent:#c98a5a`, pot/plant/personality pickers). It contained **zero** wireframe markers (no `.phone`, `.flowindex`, `Dashboard`, `GroupChat`, `StatusBar`).
- **Recovered the wireframe from `index.wireframe-backup.html`** (73 821 bytes, 1540 lines) — a local backup that still held the full Revision-3 wireframe (verified markers: `Alexandra`, `MoistureChart`, `Urbanist`, `StatusBar`, `FLOWS`). No git history existed (folder was not a repo), so the backup was the recovery source. Rationale: the backup was byte-for-byte the last good wireframe; faster and safer than reconstructing.
- **Separated the two projects so they can never clobber each other again:**
  - `./wireframes/index.html` — the wireframe prototype (canonical), plus its `DECISIONS.md`, `.gitignore`, `vercel.json`.
  - `./configurator/index.html` — the smart-pot configurator, moved out of the root untouched.
  - Deleted the now-redundant root `index.wireframe-backup.html` (preserved as `wireframes/index.html` and in git). Rationale: each app now owns a folder; nothing lives at the shared root to overwrite.

## 1. Flow index panel — drastically simplified
- **Collapsed to a flat list of 6 entries, one per flow, each jumping to that flow's ENTRY screen:** Add new plant→`pair`, Dashboard→`dashboard`, Group chat→`chat`, Notifications→`notifs`, Plant card→`plant{id:felix}`, Alert simulation→`alert`. Removed the "Flow index" heading, the "Jump to any screen" subtitle, all group labels and all individual screen names (and their now-dead `.fi-title/.fi-sub/.fi-head` CSS). Kept the `#4A7C59` active highlight and the plain dev-tool look; panel is now short and narrower (180px). Rationale: brief (1).

## 2. Dashboard — removed the alert/push simulation
- **Removed the "⚡ Simulate a 'Felix needs help' push" button**, plus the now-unreachable push-banner JSX and its `push` state. Rationale: brief (2) — the alert flow is now reachable via the flow-index "Alert simulation" entry, so the in-dashboard simulator is redundant.

## 3. Deploy (GitHub + Vercel)
- **Git repo initialised in `./wireframes/` only** (the deployed folder), with `.gitignore` (`node_modules/`, `.DS_Store`, `.vercel`) and a minimal static `vercel.json` (`framework:null`, `buildCommand:null`, `outputDirectory:"."`). Rationale: brief (3) — static single-file site, no build step; the configurator stays out of this repo.

### Deploy results (live)
- **GitHub repo (public):** https://github.com/alexandrashelenkova/bloomling-wireframes
- **Vercel production URL:** https://bloomling-wireframes.vercel.app  (HTTP 200, serves the wireframe `index.html`; verified live)
  - Vercel scope: `wannabe-course` · project `bloomling-wireframes` (GitHub repo connected for auto-deploys).
  - Alt production aliases: `bloomling-wireframes-wannabe-course.vercel.app`, `bloomling-wireframes-alexandrashelenkova-wannabe-course.vercel.app`.
- No interactive login was required — both `gh` and `vercel` were already authenticated (`gh`: alexandrashelenkova; `vercel whoami`: alexandrashelenkova). `vercel link` auto-added `.env*` to `.gitignore` (kept).

---

# Revision 5 — prototype shell polish (dark canvas, centering, flow-index restyle)
Shell-only changes; the phone mockup and everything inside the app screen are untouched.

- **Canvas → dark grey `#2b2b2b`** (`html,body`). Rationale: brief (2); mid-point of the requested #2A2A2A–#303030 range.
- **Phone vertically + horizontally centered** via `#root{display:flex;justify-content:center;align-items:safe center;min-height:100vh}`. Chose the `align-items:safe center` keyword over plain `center` so that when the viewport is shorter than the 844px phone it falls back to top-alignment and the page scrolls — the phone is never clipped out of reach. Rationale: brief (1).
- **Flow index panel de-chromed & centered:** removed the background card, border, radius and padding container — text now sits directly on the dark canvas; vertically centered against the phone with `top:50%;transform:translateY(-50%)`. Text bumped to **17px** Urbanist, line-height 1.55. Rationale: brief (3).
- **Inverted flow-index colours for dark bg:** inactive `#B0B0B0` (contrast 6.53:1), hover `#dcdcdc` (10.33:1), active `#7FB08C` (5.73:1). Chose the light accent tint `#7FB08C` over the brand `#4A7C59` because the latter only reaches 2.91:1 on `#2b2b2b` (fails WCAG AA); the tint keeps the accent identity while staying legible. Verified all ratios ≥ AA. Rationale: brief (3).

---

# Revision 6 — personality model sync with the landing-page configurator

## Single source of truth
- **One `PERSONALITIES` constant** = `{ common:[…], presets:[{k,d,sample,sliders}] }`. `common` holds the shared **Chattiness** slider (every preset gets it); each preset carries its own `sliders`, its one-liner `d`, and one hardcoded `sample` voice phrase. Helpers `personaByName()` and `slidersFor()` derive everything. Rationale: brief (4) — presets + sliders + labels + samples live in exactly one place, kept structurally diff-able with the configurator.
- **Presets: the exact 8 with the exact one-liners** from the brief (names/order unchanged from before, so no plant data drifted). Rationale: brief (1).
- **Fine-tune sliders per preset** exactly as specified (Chattiness + Energy / Drama level + Self-pity / Crust + Hidden warmth / Zen depth / Pep / Openness / Bite / Proverb density). Rationale: brief (2).

## Shared UI (both places mirror exactly)
- **New `PersonalityPicker` component is the ONLY personality UI**, rendered identically by onboarding `CreateCharacter` and `PlantSettings`: preset list → fine-tune sliders for the selected preset → live voice-preview line. Rationale: brief ((a)+(b)) — one component guarantees the two screens can't diverge.
- **`PlantSettings` upgraded from compact chips → the same opt-list + sliders + preview.** Rationale: "mirror EXACTLY" means the settings screen shows the same one-liners and fine-tune as onboarding, not a reduced chip row.
- **Sliders are wireframe-flat 3-dot controls** (`.slider`/`.sdot`): grey track, three fixed positions, selected dot fills `#4A7C59`. Default = middle; **selecting a preset resets fine-tune to neutral** (fine-tune is relative to the chosen preset). Rationale: brief (2) "3 positions each, wireframe-flat".

## Voice folded into personality (removed the old separate Voice picker)
- **Deleted the old `VOICES` picker (Warm/Bright/Soft/Playful + play/eq animation) and its orphaned `.play`/`.eq` CSS.** "Voice" is now the preset's `sample` phrase, per brief (5) — keeping a second, independent voice axis would have created a competing source of truth, contradicting brief (4). Rationale: one model, one place.
- **Voice preview line updates on BOTH preset and slider changes:** it shows the preset's sample phrase plus a second muted line echoing each slider's current end-label (e.g. "talkative · balanced · tragic"), so moving any slider visibly changes the preview. Rationale: brief (5).

## Mock plants
- **Felix→Friendly, Margot→Drama queen, Gosha→Grump were already exact — no change needed** (Vera→Calm, Basil→Cheerful also valid presets). Check-in/chat copy left as-is. Rationale: brief (3).

## Out of scope (noted)
- The **configurator project still carries the older 2-slider Warmth/Chattiness model**; syncing it is a separate task (different project). This wireframe now holds the canonical 8-preset model, and `PERSONALITIES` is shaped so the configurator can adopt the same structure with a trivial diff. Rationale: task scope is the wireframe (`./wireframes/index.html`).

## Verification
- Transformed `#app-src` through the page's Babel config (no `import`) and rendered in jsdom with **0 console errors**. Confirmed on **both** screens: 8 presets in order; Friendly→2 sliders (Chattiness+Energy), Drama queen→3 (Chattiness+Drama level+Self-pity), Grump→3 (Chattiness+Crust+Hidden warmth); preview phrase changes per preset and the slider-word line updates when a slider moves; no `PERSONAS`/`VOICES`/`.play`/`.eq` leftovers.

---

# Revision 7 — "Create their character" rebuilt as a live-preview personality tuner

## Screen structure
- **New order: PREVIEW (sticky) → NAME → PERSONALITY → FINE-TUNING → VOICE → CTA.** `CreateCharacter` is now bespoke (not the shared picker) but is composed from shared parts so the model stays single-source. Rationale: brief.

## Live preview (brief 1)
- **Sticky preview block** (`.preview`, `position:sticky;top:0`) so it stays visible while tuning below. Holds a **low-fi speech bubble** (plain 1px border + CSS-border downward tail, no shadow) with the in-character line, a **PLAY button beside it** (▶ ⇄ eq animation, fixed `.play` size → no layout shift), and the **plant PHOTO placeholder below** (100px in create, 90px in settings — smaller to keep the sticky block short). Rationale: brief (1).
- **`sampleLine(preset,tune)`** builds the line live: `drama=2`→a "big"/theatrical variant, `chattiness=0`→a short "terse" variant, otherwise the "base"; `warmth 2/0` appends an affectionate / dry tag. Each preset has its own distinct copy (Grump reads nothing like Drama queen). So changing preset OR any slider changes the bubble. Chose this small-variant composition over a full per-slider line matrix — keeps the copy authorable while every slider still visibly matters. Rationale: brief (1).

## Presets: 8 → 6 (brief 3)
- **Kept Friendly, Drama queen, Grump** (map to Felix/Margot/Gosha) **plus Calm and Cheerful** (so mock plants Vera=Calm and Basil=Cheerful stay valid presets, no data breakage) **plus Sassy** (sharp, high-contrast voice). **Dropped Shy and Wise** — no mock plant used them. Rationale: brief (3); minimise plant breakage + maximise tonal variety.
- **Compact 3×2 grid** (`.pgrid6`, `grid-template-columns:1fr 1fr 1fr`), name + a very short caption per tile. Chose **3×2 over 2×3**: verified the longest name ("Drama queen") fits one line at the ~114px column width in the phone, so 3 columns don't feel cramped. Selected tile uses the `#4A7C59` border/tint. Rationale: brief (3).

## Fine-tuning: fixed 3-slider model (brief 4)
- **Replaced the old per-preset variable sliders with THREE fixed sliders for every preset:** Warmth (Dry↔Affectionate), Drama (Understated↔Theatrical), Chattiness (Rarely↔Often). Each preset ships **sensible default positions**; the user nudges from there, and any change updates the preview. This is a **model-wide change** (`PERSONALITIES.sliders` + per-preset `defaults`), so plant settings adopts it too (single source of truth). Rationale: brief (4).
- **Low-fi slider** (`RangeSlider`): grey track, `#4A7C59` fill + round handle, no shadow; three equal click zones snap to 0/1/2. Rationale: brief (4) styling note.

## Voice section RESTORED (brief 5)
- **Reversed Revision 6's "fold voice into personality" decision.** Brief (5) asks to keep the existing Voice section (which Rev 6 had removed), so the **`VOICES` picker (Warm/Bright/Soft/Playful) and `.play`/`.eq` CSS are restored** as `VoicePicker`, with the in-button eq playback (no layout shift). Voice (timbre) is now again distinct from personality (what it says). Two mock-play affordances exist: the preview PLAY (line in the selected voice) and each voice row's sample play. Rationale: brief (5).

## Shared parts / plant settings
- Extracted `SpeechPreview`, `PresetGrid`, `FineTuning`, `RangeSlider`, `sampleLine` and reused them; `PersonalityPicker` (plant settings) now = preview bubble + preset grid + fine-tuning, mirroring the onboarding tuner's core. One source of truth preserved. Configurator remains on its older model (out of scope).

## Verification
- Babel transform (no imports) + jsdom render, **0 console errors**, on both screens. Confirmed: sticky preview (computed `position:sticky`); 6 tiles [Friendly|Drama queen|Grump|Calm|Cheerful|Sassy] in a 3-col grid; 3 sliders [Warmth|Drama|Chattiness]; 4 voices [Warm|Bright|Soft|Playful]; distinct per-preset bubble copy; bubble updates when a slider moves (Drama queen "big" → base when Drama→Understated, + warm tag when Warmth→Affectionate); preview & voice play both swap to the eq indicator with no layout shift; plant settings renders the same tuner with Felix→Friendly.

---

# Revision 8 — voice rows simplified + continuous sliders

## 1. Voice options (brief 1)
- **Removed the per-row play button and the `voicePlaying`/`onPlay` state.** Voice rows are now plain selectable `.opt` cards (tap the row to select, `#4A7C59` selected state). Playback lives only in the sticky preview, which already speaks the sample line in the selected voice. Rationale: brief (1); one playback affordance, no duplicate.

## 2. Sliders: discrete → continuous (brief 2)
- **Replaced the 3-position custom slider with a native `<input type="range" min=0 max=100 step=1>`.** Chose the native range over a hand-rolled pointer handler because it gives smooth mouse + touch dragging, tap-to-position on the track, and keyboard control for free; `touch-action:none` on the input stops the screen scrolling while dragging the handle. Rationale: brief (2) — robust cross-input dragging with the least custom code.
- **Low-fi styling kept:** `appearance:none`, grey track, accent round handle (`::-webkit-slider-thumb` / `::-moz-range-thumb`), accent fill via an inline `linear-gradient` (webkit) and `::-moz-range-progress` (Firefox), no shadows.
- **Value model migrated 0/1/2 → continuous 0–100.** Per-preset `defaults` remapped to {16 / 50 / 84} (preserves each preset's original opening line and lands in the right word bucket). `sampleLine` now flips variants on thresholds `HI=66` / `LO=34` instead of exact steps, so dragging any slider across a threshold changes the bubble.
- **Right-hand label spreads across words** via `wordFor(words,val)` (equal fifths): Warmth = Dry → Reserved → Balanced → Warm → Affectionate; Drama = Understated → Subtle → Balanced → Dramatic → Theatrical; Chattiness = Rarely → Occasional → Balanced → Chatty → Often. The label updates continuously as the handle moves. Rationale: brief (2).
- **Shared components → plant settings sliders become continuous too** (single source of truth; consistent behaviour on both screens).

## Verification
- Babel transform (no imports) + jsdom render, **0 console errors**. Confirmed: voice rows contain 0 play buttons (1 play button total = the preview), tap selects exactly one row; three `input[type=range].rng` with max=100/step=1 and `touch-action:none`; Warmth cycles through all five words 0→100 with the fill gradient tracking the value and settling at arbitrary points (e.g. 37 → "Reserved"); the preview bubble reacts to continuous drags (drama 90→Theatrical "big" line, 40→base; chattiness 5→"terse"; warmth 5→appends the dry tag).
