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

---

# Revision 9 — smart pot IA: dashboard = compact hero + full plant chat

Concept shift: **the pot is self-aware.** It already knows the reservoir was
refilled or the plant was moved, so a manual to-do list is meaningless. The
dashboard becomes a compact hero over a full-height plant chat.

## 1. Removals (brief 1)
- **Deleted the entire "Today's tasks" block** — `TaskList`, `TASKS_INIT`, the `tasks`/`toggleTask` app state, the collapsed "done" group and its `.donerow`/`.donecompact` CSS. Rationale: brief (1); a self-aware pot has nothing to ask the user to tick.
- **Deleted the `Reaction` screen too.** It was reachable *only* from a task checkbox, so removing tasks orphaned it; the praise it delivered now lives in the chat as a plant message. Rationale: no dead routes — the reward moment moved, it wasn't lost. (`PLANTS[].reaction` copy is kept as the in-character praise voice reference.)
- **Moved the 2×2 plant grid + "Add plant" off the dashboard** (see §3) and **dropped the mini chat-preview widget** and its `.mini*` CSS — the real chat is now the dashboard body, so a preview of it would be a preview of itself. Rationale: brief (1, 4).

## 2. Hero (brief 2)
- **Hero = greeting + all-clear card + 3 nav icons**, wrapped in `.hero` (`flex:0 0 auto`) so it never grows. Measures **~215px of ~770px usable (~28%)** — deliberately at the low end of "roughly the top third"; the brief says keep it tight and every pixel saved goes to the chat. Rationale: brief (2).
- **Kept the slim progress bar** (optional per brief) — it carries the 72% figure visually at ~12px cost, cheaper than a second line of copy.
- **Status copy rewritten to sell the concept**: "Everything's fine 🌿" + "The pot handles the watering — you just keep them company." Rationale: the status line must explain *why* there are no tasks.
- **Nav row = exactly three targets: 🌿 plants list · 🔔 notifications (badge dot kept) · avatar → profile.** **Chat icon removed** — chat is the screen. Rationale: brief (2).
- **Chose a leaf glyph (🌿) for the plants-list target** over a grid/list glyph: it reads as "my plants" rather than "a view mode", and the file already uses 🌿/💚 in plant copy.

## 3. Plants list screen (brief 3)
- **`PlantsList` rewritten from a flat row list into the dashboard's old pager**: swipeable pages of a 2×2 `.pgrid`, `+ Add plant` tile **pinned above** the pager (outside the horizontal scroll, so it can't vanish on page 2), page dots below. Behaviour unchanged, just relocated; tapping a card still pushes `plant`. Rationale: brief (3) — "just relocated" means moving the component, not re-designing it.
- Reachable from **both** the hero leaf icon and the existing Profile → Plants row; no duplicate screen was added.

## 4. Plant chat as the dashboard body (brief 4)
- **Folded the standalone `GroupChat` screen into `PlantChat`, rendered inside `Dashboard`.** One chat, same messages, same send behaviour; the `chat` route is gone from `SCREENS`. Rationale: brief (4) — two copies would drift.
- **Layout: `.dash` sets `overflow:hidden`** so the dashboard itself never scrolls; only `.chatscroll` (`flex:1;min-height:0;overflow-y:auto`) scrolls, and `.composer` stays pinned at the bottom of `.chatpane`. Rationale: a page-level scroll would push the composer off-screen and break "pinned input".
- **Auto-scroll to the newest message on mount** (`useRef` + `useEffect` on message count). Rationale: the urgent message sits at the bottom of the mock stream — without this it would be invisible on arrival.
- **Message row = small round photo-placeholder avatar + name + bubble** (`.mrow/.mava/.mname/.mbub`), user messages mirrored to the right in accent. Rationale: brief (4).
- **Mock stream covers all three required behaviours**: Felix warm ("the light is glorious today"), Gosha grumpy ("It's a windowsill, Felix. Contain yourself."), Margot dramatic ("I very nearly DIED of thirst"); **plant-to-plant exchange** = Gosha's jab + Felix's reply; **praise** = Margot's "thanks, reservoir's full again 💚" and Gosha's grudging "The pot says you shifted me into the light. …Fine." Rationale: brief (4).
- **Praise is triggered by the pot's own knowledge, never by a user tick** — the user's line is "The pot topped you up at 7am, Margot." Rationale: the copy has to carry the concept, or the removal of tasks reads as a missing feature.
- **`cross:true` flag replaces the old `who==="felix"` filter** for the "plants talk to each other" setting. The old filter hid *every* non-Felix plant, which mismatched the setting's label; now it hides exactly the plant-to-plant messages. Rationale: bug fix surfaced by re-siting the chat.
- **URGENT = `.mbub.urgent`** (accent border + accent text + "NEEDS YOU NOW" tag + "See what happened ›"), tap → `nav.go("alert")` → existing diagnosis flow, untouched. **Chose Felix as the urgent sender** so the message matches the existing alert content (Felix, overwatered + cold) rather than inventing a second incident. Rationale: brief (4).
- **Chat header = kicker + title + `.gear`** → `chatSettings` (plants talk to each other / frequency / Telegram), which is otherwise unchanged. Its "Group reactions" copy was rewritten off "when you finish a task" to "when the pot sees a need was handled".

## 5. Flow index (brief 5)
- **"Group chat" → "Chat settings"** (`chatSettings`), since the chat itself is now the Dashboard entry. Order and active-item highlight (`.fi-link.on`) preserved; all six entries resolve to live routes. Rationale: brief (5).
- **Welcome-tour copy rewritten** to explain the smart pot and point at the new three icons instead of tasks and the speech bubble.

## Verification
- Babel classic-runtime transform + `new Function()` parse of the emitted JS: **clean, no imports**.
- Headless-Chrome render of `dashboard`: hero ≈28% of the device, chat filling the rest, stream auto-scrolled to the accent urgent message, composer pinned, flow index showing "Chat settings" with Dashboard highlighted.
- Headless-Chrome render of `plantsList`: 2×2 grid + pinned "＋ Add plant" + page dots (2 pages, 5 plants).
- Grepped clean: no remaining references to `TaskList`, `TASKS_INIT`, `toggleTask`, `GroupChat`, `Reaction`, `nav.go("chat")` or the `.mini*`/`.done*` CSS.

---

# Revision 10 — taller hero, chat settings → profile, growth timeline on the plant card

## 1. Hero +10% (brief 1)
- **Measured before changing anything**: the hero's content height was **179.6px** in headless Chrome at the 412px device width. Set **`min-height:198px`** (= 179.6 × 1.10) rather than guessing at padding. Rationale: "roughly 10% taller" is a number, so it was worth measuring instead of eyeballing.
- **The extra 18px is spent as breathing space, not content**: `justify-content:space-between` + `gap:10px→12px` + `padding-top:4px` distribute it around the same greeting / status card / icon row. Rationale: brief (1) — layout inside stays the same.
- The chat pane absorbs the loss automatically (`flex:1`), 562px → 543px; nothing else needed touching.

## 2. Chat settings moved to Profile (brief 2)
- **Gear removed from the chat header** (and its `.gear` CSS deleted); the header is now just kicker + title. Rationale: brief (2).
- **Profile's existing settings row relabelled** "Chat & reactions" → **"Chat settings"**, subtitle "Plants talk to each other · frequency · Telegram", sitting next to Language. No new screen or row was created — the route already existed there, so this is a pure removal of the second entry point. Rationale: one entry point per destination.
- **Flow index: "Chat settings" now builds the stack `profile → chatSettings`** instead of jumping straight to it, so the shortcut lands where the user would land and **Back correctly returns to Profile**. This needed a new `nav.jump(path)` (replaces the whole stack) and FLOWS restructured from tuples to `{label, path}` objects. Rationale: brief (2) says it "opens from within profile" — a shortcut that skipped Profile would misrepresent the IA. Active-item highlight now matches on the *last* screen in the path.

## 3–4. Growth timeline at the top of the plant card (brief 3, 4)
- **"Growth & diary" row deleted** from the plant card; the timeline owns growth now. Rationale: brief (3).
- **`GrowthTimeline`: a 352px-tall (≈46% of the 770px usable screen — "about ½") horizontally snap-scrolling pager of photo-placeholder frames.** Same explicit width/height + `scroll-snap-align` recipe as the existing plant pager and species carousel, which the file already documents as required (a flex child under scroll-snap collapses to zero height in Safari). Rationale: brief (4).
- **Stages live on the frames.** Six frames, oldest → newest: Sprout 12 Mar · Seedling 29 Mar · Young plant 18 Apr · Established 24 May · Growing strong 21 Jun · Now Today. Each frame carries a white stage+date plate pinned to its bottom edge, so scrubbing the timeline *is* reading the stage history — no separate progress section. Rationale: brief (4).
- **Opens on "now" and swipes backwards into the past** (`scrollLeft = scrollWidth` on mount). Rationale: the current state is what you came to see; history is the deliberate gesture.
- **Dot indicators** reuse the existing `.pgdot` / `.dots`. **Scroll position is computed from the real frame pitch** (`firstChild.offsetWidth + gap`) rather than the container width the older pagers use — with 6 frames the container-width approximation drifts and lights the wrong dot.
- **Emotion badge on the CURRENT frame only** (top-right, the shared `.badge` component). Rationale: the emotion is a *now* fact — stamping it on a photo from March would be a lie; the brief's "consistent with how avatars show emotion badges elsewhere" is satisfied by reusing the same badge in the same corner.
- **Progress-chart entry point = a small round icon pinned top-LEFT of the block**, absolutely positioned over the pager (outside the scroller) so it never swipes away, opposite the emotion badge. Opens the unchanged `growth` screen. Rationale: brief (4) — "small icon entry point somewhere on this block".
- **Icon is an inline SVG chart glyph, not a 📈 emoji.** The emoji renders full-colour (red/blue) and would break the grayscale + single-accent rule; the SVG uses a grey axis + one `#4A7C59` polyline, matching the file's other inline SVGs. Caught in a render check.
- **Old round photo + emotion badge removed** from the status card, which now holds just the status pill and the plant's spoken line. Rationale: brief (4) — the timeline replaces it.
- **Growth screen kept fully intact** (%, chart, stage list, diary, time-lapse) — the brief moved its *entry point*, not its content. Two small alignments: retitled "Growth" → "Growth progress", and its stage list is now generated from `GROWTH_FRAMES` so the two views can't contradict each other.

## 5. Everything below the timeline (brief 5)
- Unchanged and in the same order: spoken status line → Your bond → Last watering / Light → Auto-watering + reservoir → Configure watering → Plant personality.

## Verification
- Babel classic-runtime transform + `new Function()` parse: **clean, no imports**.
- Headless-Chrome measurement: hero **179.6px → 198.0px (×1.102)**.
- Headless-Chrome renders: dashboard (taller hero, no gear in the chat header); plant card at frame 6 (badge present, "Now / Today"); plant card scrolled to frame 3 (badge correctly absent, "Young plant / 18 Apr", third dot lit, monochrome chart glyph).
- Scripted click-through of the flow index: "Chat settings" → lands on **Chat settings**, entry highlighted, **Back → Profile**.

---

# Revision 11 — major restructure: gauge hero, Windowsill, flat profile, immersive plant card

## 1. Dashboard
- **1a — avatar moved LEFT of the greeting; only two icons on the right** (🌿 plants list, 🔔 notifications with its badge dot). The profile is now reached by tapping your own face, which is where a user looks for it.
- **1b — the health banner is replaced by a semi-circle gauge** (`HealthGauge`): one grey `--box` track arc plus one `#4A7C59` arc drawn over it with `stroke-dasharray`. Chose **two overlaid `<path>` arcs over an SVG `<circle>` + rotation** because a half-ring needs only a single `A` command and the accent length is then just `πR × pct` — no transforms, no gradient, no shadow. **97%** and **healthy** sit inside the arc.
- **1c — status line: "Everyone's thriving — Margot's being dramatic, but that's just Margot."** Chosen because it names a *specific* plant: a generic line ("all systems normal") reads as a status field, whereas naming Margot makes it sound like a household. Kept the brief's own phrasing over inventing a weaker variant.
- **1d — hero `min-height:252px` ≈ the top third** of the 770px content area; the chat takes the remaining ~⅔ with the composer pinned at the very bottom (`.dash{overflow:hidden}`, only the message stream scrolls).
- **1e — the chat is now "Windowsill 🌿"** (subline: the plant names). Chose it over "The group chat 🌿" and "Leaf it to us": it names the *place* the plants share, so the header reads like a room they're all in rather than a feature label — and unlike the pun it doesn't wear out by the tenth read. Messages and behaviour unchanged.

## 2. Profile & settings — flattened to one page
- **2a/2b** — "7 days together" gone; the **plant count is the link** to the plants list (underlined, tappable).
- **2c** — the Plants section card, the "Add a new plant" row and the "+ Add plant" button are all gone from Profile. **Adding a plant now exists in exactly one place**: the plants-list screen.
- **2d** — chat settings are **expanded inline** under a subheading, two controls only: "Plants talk to each other" (toggle) and "Message frequency". Frequency is a **3-up segmented control (Quiet / Moderate / Chatty) rather than another sub-screen** — with only three values, a drill-down would cost a screen to save nothing. "Group reactions" was dropped (the brief allows two controls) and **Telegram is deleted everywhere** — the `telegram` screen, its route and the forwarding row.
- **2e** — Language is inline too: two rows showing the current value, each opening `langPick`, which renders a **plain `.list` of options with a ✓** on the current one (not the old full-width `.chip` tiles). Selecting returns immediately. `uiLang`/`convLang` moved into app state so the Profile row and the picker share one truth.
- **2f** — **Log out is a small underlined muted text button** (`.linkbtn`), deliberately not a `.btn`. It returns to the dashboard root because this prototype has no sign-in screen — the alternative was inventing an auth flow the brief didn't ask for.
- The `chatSettings` and `language` screens were deleted; nothing else linked to them.

## 3. Plant detail — rebuilt as an immersive screen
- **3a — the photo is a fixed full-bleed layer** (`.pd` replaces `.screen`, so the page itself never scrolls). Only the sheet translates; the photo never moves.
- **3b — ⋯ context menu** top-right: Plant personality / Rename / Quiet hours / Remove plant. **Personality navigates** (it is a whole tuner); **Rename, Quiet hours and Remove are in-place dialogs** — leaving an immersive screen to flip one toggle would defeat the point. Remove asks for confirmation, then **`nav.jump` to plants list** so the card you just deleted isn't sitting behind Back. `PlantSettings` was stripped to the personality tuner, since name/quiet/remove now live in this menu.
- **3c — initial state**: big pot with the emotion on it, the plant's check-in line in a speech bubble above it, the timeline near the bottom of the photo, and the "Your bond" sheet peeking (`PEEK = 96px`).
- **3d — drag**: `SHEET_H 560 / PEEK 96`, so the sheet travels 464px. Pointer events (mouse + touch in one path) with `touch-action:none` on the grab area; a drag under 6px counts as a **tap that toggles**, anything longer **snaps to the nearer end**. Progress `t` drives the dim (`0.55·t`) and fades the timeline out (`1 − 3t`). Content order is exactly as briefed: bond → tiles → auto-watering → configure → personality.
- **3e — universal milestones: Seed → Sprout → Young plant → Established → Thriving → Now**, the same six for every plant. Built the timeline as a **scroll-snap strip of milestone ticks** rather than a custom drag handle: it gives real momentum swiping, keyboard/trackpad scrolling and the labels for free, and index comes from `scrollLeft / tick width`. Opens on "Now" and swipes back into the past.
- **3f — proportions follow the timeline**: `plantH 70→220`, `potH 58→150`, and the emotion's opacity/size ramp `(g−0.2)/0.8` so it is fully **absent at Seed and Sprout** and prominent at Now. At past stages the frame is about how the plant grew; at Now it's about how it feels. The speech bubble is replaced by a **stage + date chip** on any past frame — the plant isn't talking to you in March.
- **3g — the growth chart, the chart icon, `ChartGlyph`, `GROWTH_FRAMES` and the whole `growth` screen are deleted.** The timeline is the growth view now.

## 4. Notifications — a digest, not a feed
- Rewrote the data: **"Felix needs a look" (urgent), "Margot's reservoir is empty", "Gosha needs more light"**. Social chatter is gone — it lives in the chat.
- Urgent → the alert/diagnosis flow. Everything else → **the chat, focused on that plant**: `nav.go("dashboard",{focus:id})` scrolls the stream to that plant's most recent message and outlines it (`.mbub.focus`). Chose this over dropping the user at the top of an unrelated chat, which would have made the tap feel like it did nothing. No checkboxes anywhere; the closing "That's everything for today 🌙" stays.

## 5. Add new plant
- **5a** — the idle pair screen is now a proper welcome: centred art placeholder, "Let's meet your new plant 🌱", a warm line, and the button at the bottom. Every other pairing state got a one-line lead-in so none of them are a bare card.
- **5b** — the "Connected ✓" state and the photo explainer are **one screen with one Continue** ("Take a photo"). `PhotoIntro` deleted.
- **5c/5d — the back-navigation bug**: the cause was that "Best matches" was an internal *state* of the camera screen, so returning to it remounted the camera. Split it into its own **`matches` screen** that capture pushes on success. Back from species detail now lands on Best matches. For "Not quite — keep looking" that isn't enough — the user may have hopped through several "often confused with" species — so added **`nav.backTo(screen)`**, which pops the stack back to the named screen (falling back to a single step if it isn't there).
- **5e** — gender added to character setup as a 3-up segmented control: **Male / Female / Rather not say**, defaulting to "Rather not say" so the neutral option is never a leftover.

## 6. Diagnosis — split by who can actually act
- Checkboxes and the "Tick each step to continue" gate are gone, along with the `.check`/`.taskrow` CSS.
- **In-app section first** ("One thing to change in the app"): Pause auto-watering has a **real button** → confirmed state, with the reason stated — *only you can change this, the pot won't override your watering settings*. Put it above the advice because it's the only thing on the screen that needs a tap.
- **Physical section** ("What to do in the room"): three recommendations as `.adv` rows, each with a line about how the pot will notice, closing with "No need to tick anything off — the pot notices on its own." Added a third piece of advice (skip hand-watering) so the section reads as guidance rather than a two-item list.
- The moisture chart stays at the top; the footer button is now a plain, always-enabled "Got it".

## 7. Onboarding tutorial removed
- The "This is your garden" overlay, its copy, the `tour` param, the `tourSeen` state and the `{tour:true}` argument on "Start caring" are all deleted. `.overlay` survives (re-used by the ⋯ dialogs); `.tourcard` was renamed `.dlg`.

## 8. Flow index
- Entries are now **Add new plant · Dashboard · Notifications · Plant card · Alert simulation · Profile & settings**, active-item highlight preserved.

## Verification
- Babel classic-runtime transform + `new Function()` parse: **clean, no imports**.
- Headless-Chrome renders: dashboard (avatar-left header, gauge, Windowsill), plant card at "Now" (big pot + face + speech bubble), plant card scrubbed to "Seed" (small pot, **no** emotion, stage chip), sheet dragged open (photo dimmed and stationary, timeline hidden, correct content order), flat profile, notifications digest, diagnosis, pair welcome.
- Scripted click-throughs, all passing:
  - species detail → Back = **Best matches**; nested look-alike → "keep looking" = **Best matches** (5c/5d).
  - ⋯ menu lists all four items; Rename dialog opens/cancels; Remove → confirm → **My plants**.
  - notification tap → chat with **Margot's message outlined**, header "Windowsill 🌿".
  - Profile → Conversation → 4-option list → pick Español → back to Profile showing **Español**; Log out → dashboard.
  - full add-plant run: searching → found → connect → **merged connected+photo screen** → camera → shutter → **Best matches** → Select → character setup with **Male/Female/Rather not say** → Say hello → Start caring → dashboard with **no overlay**.
  - alert → diagnosis: **0 checkboxes**, 3 advice rows, 1 chart, Pause button reaches the confirmed state, "Got it" → "Thank you!".

---

# Revision 12 — uniform diagnosis list, scrolling plants grid, two-state timeline, immersive character setup

## 1. Diagnosis
- **1a** — the "Soil moisture · last 3 days" card is gone, along with the whole `MoistureChart` component and its `.chart` CSS. The two diagnosis cards above already state the 92% reading, so the chart was restating a number the user had just read.
- **1b — one block, one heading ("What to do"), four identical `.adv` rows.** The in-app step carries a `btn sm primary` *inside its own row*; the other three are the same row with no button. Chose to keep **"Pause auto-watering" first** rather than last: it is the only thing on the screen that needs a tap, and burying it under three pieces of advice would make it easy to miss. The distinction between "you do this" and "the pot notices this" now lives in each row's sub-line rather than in the layout.
- **1c** — "No need to tick anything off…" removed. With no checkboxes on screen there was nothing to explain away.
- Net effect: the whole screen now fits without scrolling.

## 2. My plants
- **2a/2b** — the pager is gone. One `.pgrid` that flows down and scrolls; `.pager` and `.page` CSS deleted, and `.pgrid` gained `flex:0 0 auto` so its fixed 150px rows aren't squashed by the flex-column screen. The swipe hint and the page dots went with it.

## 3. Plant detail
- **3a — corrected to TWO framings, not a gradual zoom.** `atNow` picks between them:
  - **NOW** — plant 148×200, pot 188×150 with the current emotion on it.
  - **PAST** — one snap to a zoomed-in framing: plant 208 wide, pot a constant **246×26 rim** with `border-radius:8px 8px 0 0` so it reads as the pot continuing below the frame, and **no emotion** (a past frame is a photograph, not a mood).
  Within the past **only the plant height moves**, `96 → 296px` across Seed…Thriving via `idx/(last-1)`. The pot no longer scales with timeline position at all. Verified by measurement: pot is 246×26 at Seed, Young plant *and* Thriving while the plant reads 96 / 196 / 296.
- **3b** — the ⋯ menu is a single row, **"Personality & Settings"**. Kept it as a menu rather than making ⋯ a direct link: a one-item popover still announces that this is *the* place everything about the plant lives, and leaves room for future entries. That screen now holds **name, personality tuner, active hours (quiet-hours toggle + a 10pm–8am / 11pm–7am / Custom window) and a small "Remove plant"** as a muted `.linkbtn` low on the page with a confirm dialog — destructive actions never get a primary affordance. The old Rename / Quiet hours / Remove dialogs and the `QuietHours` component are deleted.
- **3c** — the "Plant personality" row is out of the bottom sheet.
- **3d** — "Configure watering" is no longer its own row: it's a **`.tlink` text link ("Configure schedule ›") inside the auto-watering card**, dimmed and inert when the toggle is off. A setting that only exists because of that toggle belongs in the same card as the toggle.

## 4 & 5. Removed copy
- "That's everything for today 🌙" (notifications) and "Swipe the photos and check these features against your plant." (species detail) both deleted. The carousel's dot indicators already say "swipeable".

## 6. Create their character
- **6a** — the top **~⅔ is an immersive plant + pot** (`.cchero`), built from the same `.pdplant` / `.pdpot` parts as the plant card so the two screens read as the same product. Every control is pushed below it.
- **6b** — the live line is a **`.pdspeech` bubble coming off the pot**, the identical component the plant card uses, replacing the old `.speech` bubble/`SpeechPreview` block.
- **6c** — the round play button is replaced by a **text "▶ Listen" button** under the bubble, `min-width:152px;height:38px` so switching to "Playing…" with the eq bars **cannot shift the layout** (measured 152×38 in both states). `.play` CSS deleted.
- **The bubble + Listen are `position:sticky` inside the hero.** This was the one real fork: a purely static hero satisfies the composition but hides the line you are actively tuning, which is the entire point of a live preview. Sticking just the bubble keeps the requested framing at rest — bubble above plant above pot — and keeps the line on screen once the pot scrolls away, with no duplicated element. Verified: after scrolling 420px the bubble sits 8px from the top of the scroll area.
- `SpeechPreview` became **`LinePreview`** (bubble + Listen) and is shared with the plant-settings tuner, so both screens got the text button and there is still one source of truth.
- The pot in this hero wears the **greeting face `^o^`** — the plant is introducing itself and has no sensor history yet, so a mood glyph would be meaningless.

## Verification
- Babel classic-runtime transform + `new Function()` parse: **clean, no imports**.
- Source grep confirms all five removed strings are gone (swipe hint, tick line, end-of-list line, species swipe line, moisture chart heading) — note `document.body.textContent` includes probe-script source, so removals were checked against the file, not the rendered DOM.
- Renders: diagnosis (0 charts, one "What to do" heading, 4 uniform rows, 1 button, fits without scrolling), plants list (5 cards, 0 pagers, 0 dots), plant card at Now and scrubbed into the past, Personality & Settings, create-character hero.
- Scripted checks: framing measurements above; ⋯ menu = one item → Personality & Settings; sheet contains no personality row; "Configure schedule" → Watering; Remove plant → confirm → My plants; Listen button 152×38 in both states with 0 round play buttons; sticky bubble after scroll.
- Regression re-run of Revision 11's flows — full add-plant run, species-detail Back and "keep looking", notification→focused chat, language picker, log out — all still pass.

---

# Revision 13 — pot face follows the tuned line, "Try it", notifications kicker

## 1. Face ⇄ line sync on the personality tuner
- **Faces are keyed to the LINE VARIANT, not just the preset.** Extracted `lineVariant(t)` (big / terse / base) so the sentence and the face are chosen by the *same* function — they cannot drift apart by construction. `faceFor(preset,tune)` then reads a new per-preset `faces:{base,big,terse,warm,dry}` map that mirrors the existing `lines` keys exactly.
- **Chose per-variant faces over one face per preset.** A single face per preset would go stale the moment a slider swings the wording — the brief explicitly asks the face to follow when the mood of the wording shifts, and e.g. Grump's theatrical "In MY day…" is a different mood from Grump's flat "Hmph. You again."
- **Mapping (all from the existing EMO set, no new emotions):**
  | preset | base | big | terse | warm tag | dry tag |
  |---|---|---|---|---|---|
  | Friendly | content `^‿^` | greeting `^o^` | content | greeting | neutral `._.` |
  | Drama queen | thirsty `>_<` | urgent `O_O` | overwatered `@_@` | content | neutral |
  | Grump | low-light `-_-` | hot `>~<` | low-light | content | cold `:<` |
  | Calm | sleeping `-.-` | content | sleeping | content | neutral |
  | Cheerful | greeting `^o^` | urgent `O_O` | content | greeting | neutral |
  | Sassy | hot `>~<` | hot | neutral | content | cold |
  Rationale per pick: `-_-` is the canonical unimpressed face for Grump; `@_@` reads "barely alive" for Drama queen's "Ugh. Fine. Alive. Barely."; `O_O` is wide-eyed for the two over-the-top "big" lines; `-.-` reads serene rather than asleep for Calm; Sassy stays smug whether understated or theatrical, so base and big share `>~<`.
- **Precedence rule — the warmth tag re-colours the face EXCEPT on the "big" line.** The tag is appended to the end of the sentence, so it normally sets the parting mood; but a theatrical line dominates whatever is appended to it, and letting a warm tag turn Drama queen's "Call the papers — my FINAL act!" into a soft smile would read as a bug. This was the one real fork.
- **Applied to both personality-setup surfaces**: the create-character pot renders `faceFor(...)`, and the Personality & Settings tuner's round photo now carries the same value as its emotion badge — one rule, both screens.

## 2 & 3. Copy
- "Listen" → **"Try it"** (button and the two comments that named it). Size is unchanged, so the no-layout-shift guarantee still holds.
- Notifications' "Needs your attention" kicker removed; the three items already say what they need.

## Verification
- Babel classic-runtime transform + `new Function()` parse: **clean, no imports**.
- Scripted sweep of the create-character screen reading the pot glyph and the bubble text together:
  - preset switch: Friendly `^o^`, Drama queen `O_O` + "FINAL act!", Grump `:<` + "Don't touch anything.", Calm `-.-`, Cheerful `^o^`, Sassy `>~<` + "SERVING chlorophyll realness".
  - slider thresholds on Grump: drama→90 flips to the big line **and** `>~<`; chattiness→80 returns the base line; warmth→90 appends the warm tag and the face becomes `^‿^`; warmth→5 restores the dry tag and `:<`.
  - the precedence rule holds: Drama queen with warmth 95 keeps `O_O` on the big line, but drops to `^‿^` once drama falls to 10 and the line is base + warm tag.
- Renders confirm Grump and Drama queen on the pot, the "▶ Try it" button, and Notifications with no kicker.

---

# Revision 14 — VISUAL DESIGN PASS (Figma "concept2")

Scope: the Dashboard is **fully rebuilt** to the two new mockups; every other
screen gets a **restyle only** — colours, fonts, bubble/button styling. No
layout, structure, navigation or content changed outside the dashboard.

## 0. Design source — Figma MCP (access OK, nothing approximated)
- File `KLRJyy5g05eK4RgZ99zlqy`, section `353:321`. The two frames named
  **concept2** are the two states of one screen:
  - **Frame A `343:6`** — collapsed (default).
  - **Frame B `344:126`** — expanded state of the same screen.
- `get_design_context` returned full reference code + screenshots for both;
  `get_variable_defs` returned `{}` — **the file has no design variables**, so
  every token below was read off raw fills/type/geometry and re-declared by hand
  in `:root`. Node ids are cited per token so they can be re-checked.
- The blob bubble backgrounds are vectors, not rectangles — their SVG paths were
  downloaded and measured (`Ellipse 19/20/21`) to derive the CSS radii.

## 1. Colour tokens (exact, from Figma)
| token | value | source |
|---|---|---|
| `--bg` page (milky off-white) | `#EDEEE9` | frame A fill `343:6` |
| `--bg2` chat sheet / scrim | `#EFEFEF` | `345:213`, `350:259` |
| `--sage` header pill + hero flood | `#C9D6BC` | `343:60`, `344:130` |
| `--ink` body text | `#050505` | `344:97` |
| `--serif-ink` display headline | `#1E0C00` | `344:125`, `345:215` |
| `--icon` icon strokes | `#1F1F1F` | mic / bell / leaf exports |
| `--input` composer pill | `#FFFAFA` | `344:74` |
| `--ava` avatar placeholder | `#D9D9D9` | `344:92` |
| `--c-margo` Margot bubble | `#BCCAD7` | `Ellipse 19` fill |
| `--c-felix` Felix bubble | `#E3D2CA` | `Ellipse 20` fill |
| `--c-me` user bubble | `#FFFFFF` | `Ellipse 21` fill |
| timestamps | black @ 40% | `344:96` |
| bubble hairline divider | black @ 10% | `350:235` |

**Derived (not in Figma) — logged as required:**
- `--c-gosha #E0D3B8` — the third character tone the brief asked for: warm
  sand/ochre. Picked to sit between the blue-grey and the terracotta at the same
  chroma/value so the three read as one family.
- `--c-vera #CBD5C9` (soft sage-grey) and `--c-basil #D3D0DC` (muted
  lavender-grey) — the garden has five plants, not three; same muted register.
- `--sage-deep #6E845C` — `#C9D6BC` is a *surface*, unreadable as text or a 1px
  active state. This darkened sibling carries accents (checkmarks, links,
  slider fills, active ticks) on the restyled screens.
- `--box #E1E2DB` — warm neutral replacing the old cold `#e4e4e4` fills.
- Prototype-shell canvas `#2b2b2b → #262521`, so the dark frame around the phone
  is warm-neutral instead of clashing blue-grey.
- Frame A's page fill (`#EDEEE9`) and frame B's (`#EFEFEF`) differ by ~1%. Kept
  both: A's value is the page, B's is the chat sheet — which is how the two
  frames actually use them.

## 2. Type
- **Urbanist removed entirely**; the Google-Fonts `<link>`s are gone. All four
  faces are local `@font-face` with relative paths (`fonts/…`); the Riccione
  filename contains a space, referenced as `%20`.
- **Riccione Serial Light — display headlines only.** Used on: the dashboard
  headline in both states, and the three centred "moment" headlines (First
  meeting / Alert / Thank-you) via a new `.display` class. **Fork:** those three
  are the only non-dashboard titles that are a hero moment rather than UI
  chrome; ordinary screen titles stay Circular.
- **Circular Std** everywhere else — Book 400 body, Medium 500 for names,
  labels, buttons, active states. Bold 700 is wired up but unused.
- **Type scale LOCKED to the four sizes in the frames** — nothing else exists:
  | token | px | Figma source |
  |---|---|---|
  | `--t-sm` | 13 | names + timestamps `344:95/96` (letter-spacing 0.52px) |
  | `--t-md` | 16 | body, buttons, inputs, inline CTA `344:97` |
  | `--t-lg` | 22 | collapsed display headline `344:125` |
  | `--t-xl` | 44.7 | expanded display headline `345:215` |
- **Off-scale sizes mapped (closest step), one line each:**
  | was | now | note |
  |---|---|---|
  | 10, 11, 12 px (kickers, tiny meta, tile labels) | 13 | closest step |
  | 14, 15 px (body copy, buttons, inputs) | 16 | closest *semantic* step — 16px is the Figma body size; 13 would have shrunk all body copy |
  | 16 px (`h2`) | 16 | unchanged |
  | 17, 18 px (screen titles, flow index) | 22 for titles, 16 for the flow index | **Fork:** literal-closest for an 18px title is 16px, which would flatten every screen to a single size. Titles take the next real step (22, Circular Medium); the shell's flow index is not app UI so it takes 16. |
  | 19, 20 px (two onboarding h2s) | 22 | closest step; stays Circular — not display headlines |
  | 21 px (old dashboard greeting), 36 px (gauge %) | — | deleted with the old dashboard |
  | 32 / 40 px ASCII faces (`^‿^`, `O_O`) | unchanged | **Fork:** kaomoji are illustration built from characters, not type — sizing them off the text scale would shrink the plant's face to nothing. Explicitly exempt. |
- Figma's frame A also contains a stale 18px **Playfair Display** headline
  (`343:48`, "97% healthy") sitting behind the pill. Ignored as leftover: the
  live headline `344:125` is Riccione, and 98% is the number in both frames.

## 3. Shape
- Header pill radius **36px** (`343:60`); composer pill **36.932 → 37px**
  (`344:74`); white CTA pill **28px** (`345:216`); chat-sheet top corners
  **36px** (`345:213`).
- **Organic bubble.** The Figma bubbles are vector blobs, not rounded rects, so
  they can't be copied literally. Measured off the paths: corner radii run to
  roughly half the bubble height, deliberately unequal. Rendered as an elliptical
  four-corner radius — `10px 56px 44px 64px / 10px 44px 60px 52px` — four
  different large radii plus one tight corner, mirrored for user messages
  (`--bub-me`). Reused for every chat-style bubble in the app (`.pdspeech` on
  plant detail and create-character).
- **Fork:** in Figma the *tight* corner is not the one nearest the avatar (the
  blobs are near-symmetric pills). The brief explicitly asks for a tight corner
  by the avatar, mirrored for the user — the brief wins; it also gives the
  bubbles a direction the Figma vectors lack.
- Cards lose their 1px borders and become white 24px surfaces; buttons become
  full pills (white default, sage primary). Dividers are the Figma black-10%
  hairline.

## 4. Dashboard rebuild
- The old **gauge + location groups + bordered chat pane are gone.** The
  collapsed screen is: sage pill (serif headline left; leaf, bell, avatar right)
  over the group chat, which is now the main and *only* content.
- **One element morphs between the two states** (`.gsurface`: pill → full
  bleed). The headline `left: 27px → 50%` + `translateX(-50%)`, `top: 23 → 121`,
  `font-size: 22 → 44.7` all transition on the same 460ms
  `cubic-bezier(.22,.9,.28,1)` ease-out, so the text scales with the container.
  Date and the white "See your plants" pill fade in (280–300ms, delayed 120ms).
- **Fork — how the green gets behind the chat sheet.** The naive version (green
  at 100% height, chat above it) needs a z-index flip mid-animation, which
  flashes. Instead the green surface animates to exactly the sheet's top edge
  (434px) and a separate full-bleed sage underlay fades in beneath the chat, so
  the sheet's rounded corners still show green. No stacking order ever changes.
- **Fork — the status bar.** Figma floods green to the pixel row 0, but the
  status bar is device chrome that lives outside the screen element. Used
  `.device:has(.dash.open){background:var(--sage)}` so the flood reaches the top
  edge; browsers without `:has()` just keep a milky status bar.
- Expanded chat is pushed to 434px, gets the `#EFEFEF` sheet, `saturate(.42)`
  and a top scrim, and is pinned to its newest message for the whole transition
  (rAF loop) so it reads as *pushed down* rather than *scrolled away*. Tapping
  the green **or** the dimmed chat collapses it.
- Collapsed state has the same scrim, solid for the first 96px, so messages
  dissolve under the header instead of colliding with it (Figma `350:261`).
- **Inline CTA is plain text over a hairline** ("Check what happened"), never a
  boxed button; it stops propagation so it navigates instead of collapsing.
- Composer: elongated pill, **microphone replaces the send arrow**; Enter still
  sends, and the mic is wired to the same handler so the prototype stays usable.
- Timestamps added to `CHAT_INIT` (07:12 → 17:30) — the Figma bubbles show
  `name · time`. Message *content* is unchanged.
- Icons (leaf, bell, notification dot, mic) are the **exported Figma assets**,
  inlined as SVG at their designed 24×24 boxes, leaf and bell at the designed 40%
  opacity. **Fork:** the brief puts the notification dot on the plant icon;
  in both Figma frames it is unambiguously on the **bell** (x=314 vs bell
  297–321, and 331 vs 314–338 in frame B). Followed Figma — a notification dot
  belongs to the bell anyway.
- **Fork — the user avatar.** Figma uses a photo of a real person. Kept the
  existing striped placeholder circle instead: this is a wireframe, and shipping
  a stranger's face into the prototype is worse than an obvious placeholder.
- **"See your plants" → `plantsList`**, the existing plants overview. That
  screen already existed, so it was restyled only — grid and cards untouched.

## 5. Housekeeping
- Flow index kept working; restyled to warm-neutral text with the sage itself as
  the active tint (readable on `#262521`), 16px per the locked scale.
- Removed dead CSS with the old dashboard (`.hero`, `.gauge`, `.chatpane`,
  `.mrow/.mbub/.composer`, `.utag/.ulink`, the `.pdspeech` tail triangles). Every
  other class kept its name and structure so no other screen shifted.

## Verification
- Rendered in headless Chrome at device size, both dashboard states plus
  plants list, plant detail, notifications, profile, alert, create-character and
  pairing: fonts load locally, no Urbanist reference remains anywhere in the
  file, no layout regressions on the restyled screens.
- Collapsed and expanded states compared side by side against the two Figma
  frame screenshots — headline, icon row, bubble colours, scrim, sheet, CTA and
  composer all land in the designed positions.

---

# Revision 15 — collapsed dashboard: blob bubbles, pot-character avatars, new script

Scope: the **collapsed** dashboard state only. The expanded hero, every other
screen, all layouts and all navigation are untouched apart from the one removal
listed in §3.

## 0. Design source — Figma MCP (access OK)
- Bubble reference: section `353:321`, frames `343:6` / `344:126` (the vectors
  `Ellipse 19/20/21` inside them).
- Avatars: section `360:555` — three composed avatar frames, `360:549`
  (monstera), `360:550` (cactus), `360:551` (ficus), plus the user photo avatar
  `360:552` (not used, see §2).

## 1. Bubble silhouette — rebuilt from the vector paths
- **Why the previous pass read as a rounded rectangle:** it used `border-radius`,
  which can only draw elliptical corners. The mockup's bubbles are *vector blobs*
  whose cubic control points sit **on the corner vertex** — the outline hugs the
  edge and turns late, a superellipse. Measured from `Ellipse 19`: its
  bottom-right corner passes through (251.2, 103.9) where a true ellipse of the
  same radii would pass through (229.5, 97.3) — i.e. visibly fuller than anything
  `border-radius` can express.
- **Now painted from the path itself**: each bubble gets an inline SVG
  data-URI background (`blobBg()`), `preserveAspectRatio="none"` +
  `background-size:100% 100%`, so one path adapts to any message length. Chosen
  over `mask-image` because a mask also clips the text; a background cannot.
- Extracted geometry, per shape (viewBox = the Figma vector's own box):
  | shape | viewBox | Figma source |
  |---|---|---|
  | plant message | 269.327 × 114 | `Ellipse 19` |
  | alert message | 269.383 × 165.181 | `Ellipse 20` |
  | user message | 268.673 × 70 | `Ellipse 21` |
- **Fork — "use the values exactly" vs "four DIFFERENT radii".** In Figma all
  four corners run at half the box (136 × 57 on the plant bubble), so three of
  them read identically — which is exactly the too-even look being complained
  about. Kept Figma's *construction* (controls on the vertex) and its largest
  radius, then staggered the rest: plant `TR 90×44 · BR 136×57 · BL 70×52`,
  alert `TR 100×62 · BR 136×82.7 · BL 64×74`, user `TL 90×24 · BL 70×35 ·
  BR 120×28`. Uneven silhouette, same curve character.
- **Tight corner at the avatar: 16px**, top-left on plant messages, mirrored to
  top-right on user messages. This is a brief-over-Figma call (repeat of
  Revision 14): the Figma blobs have no tight corner at all, but it is what gives
  the bubble a direction and anchors it to its avatar.
- Verified no text escapes the narrower shape: the tightest case is the alert
  CTA, where the outline is at x=258.5 while the text box ends at x=254.
- `--bub` / `--bub-me` stay defined — `.pdspeech` on plant detail and
  create-character still uses them, and those screens are out of scope.

## 2. Plant avatars — illustrated pot characters
- The three characters are **composed** assets, not raw exports. Each Figma
  avatar frame draws one source illustration **twice**: once masked to the
  37.81px circle (the pot), once clipped to a rectangular band (the plant, which
  overhangs the circle). `get_screenshot` will not upscale past 1x, so the
  composition was rebuilt at **4x** from the 1024² source art plus the frame
  geometry, and the white backing circle (`Rectangle 24`) baked in:
  | file | frame | circle | source placed at | band |
  |---|---|---|---|---|
  | `assets/avatar-margot.png` (monstera) | 51 × 65.81 | (5, 28) d37.81 | (−22, −8) 92×92 | (0,0,51,54) |
  | `assets/avatar-gosha.png` (cactus) | 39 × 42.81 | (0, 5) d37.81 | (−24.57, −22.72) 86×87.07 | (0,0,39,27) |
  | `assets/avatar-felix.png` (ficus) | 38 × 47 | (0, 9) d37.81 | (−15, −7) 67×67 | (−15,−7,67,38) |
  34–66 KB each, RGBA, transparent outside the circle ∪ band.
- **Character mapping is by species, not by frame order:** monstera → Margot,
  cactus → Gosha, ficus → Felix.
- Placed with the pot sitting exactly on the 37.81px avatar circle (`AVATAR`
  offsets `l/t` in the source), so the plant overhangs upward as designed —
  Margot's monstera by 28px, Felix 9px, Gosha 5px. The overhang lands in the
  empty avatar column of the row above, so nothing collides.
- **Fork — Vera and Basil have no artwork.** They are not in the new script, and
  the avatar frame only ships three characters; they fall back to the grey
  placeholder circle. No invented art.
- **User messages keep the grey `#D9D9D9` circle** — that is what both mockup
  frames show for the user; the photo avatar `360:552` is header-only, and a
  stranger's face still does not belong in a wireframe (Revision 14 call).

## 3. Header
- **Leaf icon removed**, along with its `nav.go("plantsList")` handler and the
  `IconLeaf` component (deleted, not just unmounted). The plants overview is now
  reached only from "See your plants" in the expanded hero — and from Profile's
  existing "5 plants ›" row, which is out of scope and was left alone.
- **Badge dot ring is now the sage pill colour** `#C9D6BC` instead of the
  exported `#D9D9D9`, so the dot reads as cut into the header rather than
  floating on it. This is a deliberate override of the exported asset.

## 4. Chat script
- Replaced with the six-message script verbatim, in order: Felix's new leaf →
  user's congratulations → Margot's photoshoot outrage → Gosha's deadpan → user
  telling Gosha off → Margot's alert. Timestamps continue the existing style
  (09:02 → 09:11, alert at 17:30).
- Bubble colours are unchanged and still keyed to the character, so the alert
  bubble is now **Margot blue-grey** rather than Felix terracotta.
- **Fork — `cross:true` flags.** Messages 3, 4 **and 5** carry it, including the
  user's reply: the banter is one exchange, and hiding Gosha's line while keeping
  "Calm down, Gosha" would dangle. With "plants talk to each other" off the chat
  reads Felix → congratulations → Margot's alert, which still holds together.
- **Known inconsistency, deliberately not fixed:** the alert is now Margot's, but
  the alert/diagnosis screens it opens still say Felix. Those screens are
  explicitly out of scope this pass.

## Verification
- Rendered collapsed and expanded in headless Chrome at device size: avatars
  load from `assets/`, the four corners of every bubble differ, the tight corner
  faces the avatar (mirrored on user messages), the header carries only bell +
  avatar, and the expanded hero is pixel-identical to Revision 14.

---

# Revision 16 — dashboard: header spacing, reactions, motion, scroll behaviour

Scope: the dashboard only (collapsed header, chat behaviour, expand animation).
No other screen touched.

## 1. Header spacing — measured off the mockup (`343:6`)
| what | mockup | was | now |
|---|---|---|---|
| headline left edge | x=41 (pill starts at 14 → **27px inner padding**) | `left:27px` — measured from the *screen* edge, so only 13px of padding | `left:41px` |
| icon row right inset | avatar right edge 371 of 402 → 31 | 31 | 31 (unchanged) |
| bell → avatar gap | 10 (bell 297–321, avatar 331) | 10 | **14** |
| badge dot | 8px core at (314,84) — 1px past the bell's right edge, ring extends 3px | `right:-9px` — leaned 9px into the gap | `top:-3px; right:-3px` |
- **Fork — the gap is 14px, not Figma's 10px.** Figma measured 10px with *three*
  items in the row; with the leaf gone (Revision 15) two items at 10px read
  cramped. The real culprit was the badge dot hanging 9px into the gap, which is
  fixed to the mockup's placement; 14px is the deliberate extra on top.

## 2. User avatar (`360:552`)
- Exported to `assets/avatar-user.jpg` — centre-cropped square, 160px (4× the
  40px header circle), JPEG q88, 8 KB. JPEG over PNG because it is a photograph
  with no transparency (50 KB → 8 KB).
- Used in the collapsed header (40px, `object-fit:cover`) and next to every user
  message (37.81px), replacing the grey circle. It joins the `AVATAR` map as
  `me` with a `round` flag, so one code path serves plants and user; the plant
  entries keep their overhang offsets, the user's is a plain circle crop.
- This reverses the Revision 14/15 call to keep a placeholder — the avatar is
  part of the design file and its use was asked for explicitly.

## 3. Reactions
- Data: `rx:[[emoji, count], …]` on a message. Rendered as chips absolutely
  positioned at `bottom:-11px`, hooked over the bubble's bottom edge, 15px in
  from the bubble's outer edge — left on plant messages, right on user messages,
  so they never sit under the tight avatar corner. White pill, 23px tall,
  `0 1px 4px` shadow to lift them off same-coloured bubbles.
- Seeded set (reactions come from everyone, both directions):
  | message | reactions |
  |---|---|
  | Felix — "New leaf today!" | 💚 2 (user + Margot), 🎉 1 |
  | user — "Congrats, Felix!" | 🌿 1 (Felix) |
  | Margot — "…photoshoot?!" | 😂 2 (Felix + user) |
  | Gosha — "A leaf. Everyone's celebrating a leaf." | 💅 1 (Margot), 😂 1 (user) |
  | user — "Calm down, Gosha." | 👍 1 (Felix) |
  | Margot — alert | none |
- **Fork — the alert bubble gets no reactions.** Emoji on a "my soil's soggy"
  alert undercuts it; the alert is the one message that asks for action.
- **Fork — the count always shows, even at 1.** Uniform chip width reads calmer
  than a mix of bare emoji and emoji+number, and matches the referenced style.
- Chip text is `--t-sm` (13px), the locked scale's smallest step — no new size.
- Rows carrying chips get 24px bottom margin instead of 11px, so the chip never
  collides with the next message.
- Static only, as specified: no picker, no add/remove.

## 4. Chat top fade — now conditional
- The fade is opacity-0 by default and gets `.on` only while
  `scrollTop > 4`. At the very first message there is nothing hidden above, so
  the gradient would be dimming a bubble for no reason.
- Driven by the scroll handler plus a refresh after the programmatic
  scroll-to-bottom lands, so the flag is right on first paint too. 220ms fade so
  it never pops.

## 5. Expand animation
- Two easings: `--spring: cubic-bezier(.16,.84,.28,1.02)` — a gentle
  ease-out-back with ~2% overshoot, used on everything expanding; and
  `--soft: cubic-bezier(.22,1,.36,1)` for the reverse.
- **Stagger (expand, ~530ms total):** green surface 500ms from frame 0 →
  headline 460ms at +40ms (so it reads as carried *by* the surface, not racing
  it) → date 260ms at +200ms → CTA 280ms at +250ms. Date and CTA also slide
  (−6px / +10px) as they fade.
- **Collapse is the same in reverse and faster:** everything 380ms with no
  delays, and date/CTA drop out in 140ms so the pill's contents are clean before
  it finishes shrinking.
- **Blur:** `saturate(.45) blur(2.5px)` on `.gscroll` — the content — rather than
  on `.gchat`, so the sheet's fill and its 36px corners stay crisp while the
  conversation goes soft-focus. 460ms in (+60ms), 340ms out. `filter` over
  `backdrop-filter` because the thing being blurred is the element's own content,
  not what's behind it.

## 6. Scroll-to-collapse
- With the hero open the stream is frozen (`overflow-y:hidden`), so a wheel or
  touch-drag over the chat is unambiguous: both collapse the hero, alongside the
  existing tap. The collapse runs the faster 380ms reverse curve.

## 7. Bubble width hug
- `.gbub` goes from a fixed `width:269px` to `width:auto` with
  `max-width:min(269px, calc(100% - 48px))`. Short messages now produce short
  bubbles; long ones still wrap at the mockup's 269px.
- The blob background stretches to whatever width results, so a narrow bubble
  keeps proportional corners rather than a squashed silhouette.

## Verification
- Rendered collapsed (scrolled and at top), and expanded, in headless Chrome:
  no gradient at the top of the stream, gradient present once scrolled, chips
  attached to every seeded message, user avatar in header and chat, bubbles
  hugging their content, expanded chat blurred with the sheet edge crisp.

---

# Revision 17 — "My plants" rebuilt as a wallet-style card stack

Scope: the plants overview only. The dashboard, plant detail and everything else
are untouched; the only shared change is an additive `word` field on `EMO`.

## 1. Layout
- The 2-column photo grid is gone. Every plant is now a **full-width coloured
  card, 176px tall, pulled up 56px over the card above** — so 120px of every card
  shows (the brief's 110–130px band) and the last card stands at full height.
- **Later cards paint on top** (`z-index` set inline, `i+1`). This is the fork in
  reading "each next card tucks under the previous one": if the next card went
  *behind*, the card above would cover its top and you'd see each card's
  **bottom** — the opposite of "mostly the upper portion of every card is
  visible", and the last card could not be the full-height one. Painting later
  cards in front satisfies both of those sentences and is the wallet/stack look.
- **Only the top corners are round (26px).** Every card's bottom is tucked under
  the next one, so rounding it just leaked little notches of page background at
  the sides; `:last-child` gets all four corners since it is the one card whose
  bottom is visible.
- `box-shadow:0 -1px 10px rgba(30,12,0,.07)` — an *upward* shadow onto the card
  above, which is what actually sells the stack.
- `flex:0 0 auto` on the stack so the flex-column screen can't squash it; the
  five cards total 656px and scroll inside the screen as designed.
- **Fork — cards keep the screen's 16px gutter** rather than bleeding to the
  device edge. "Full-width" is read as full content width: it keeps the rhythm of
  the "Add plant" pill above them and of every other screen.

## 2. Card content
- Emotion face top-left in a 28px white disc — the same white-disc treatment the
  old photo cards used for their emotion badge, so the language carries over.
- Species in `--t-sm` at 50% opacity, then the name at `--t-lg` (22px) Circular
  **Medium** — category-over-merchant hierarchy from the reference. Serif is not
  used here; the only display headline on this screen is the title.
- **Fork — the name is the first word only** ("Margot", not "Margot the
  Monstera"). The species already sits directly above it, so the full name would
  repeat it; this also matches how the chat labels its speakers.
- Status word on the right, centred on the **visible 120px band** rather than on
  the card's full height — centring on the full height would push it into the
  area hidden by the next card.
- Colours are the established character palette, unchanged:
  Felix `--c-felix #E3D2CA`, Margot `--c-margo #BCCAD7`, Gosha `--c-gosha
  #E0D3B8`, and the two already-derived tones now get their first real use —
  Vera `--c-vera #CBD5C9` (soft sage-grey), Basil `--c-basil #D3D0DC` (muted
  lilac). Applied as `var(--c-<plant id>)`, so the ids and the tokens stay in
  lockstep.
- Text is `--serif-ink #1E0C00` on every card — one warm dark neutral, as asked,
  rather than per-card contrast tuning.

## 3. Status words
- `EMO` gains a short lower-case `word` per emotion (additive — `label`, `face`
  and `tone` are untouched, so `StatusPill` and every other consumer is
  unaffected):
  happy · thirsty · soggy · gloomy · sleepy · chirpy · cold · too hot · urgent ·
  **grumpy**.
- **Fork — `neutral` reads "grumpy"**, not "okay". Its only holder is Gosha the
  cactus, its face is the deadpan `._.`, and the brief names "grumpy"
  explicitly. Noted as data-dependent: a different plant carrying `neutral` would
  inherit the word.

## 4. Header and interaction
- The screen builds its own header instead of using `Top`, so the title can be
  Riccione: `.display` at `--t-lg` — **the same face and size as the dashboard's
  collapsed header pill**. `font-weight:400` is set inline because `.topbar h1`
  (specificity 0,1,1) would otherwise beat `.display` (0,1,0) and synthesise a
  bold weight on a light serif.
- Back control and the dashed full-width "Add plant" pill are unchanged.
- Press feedback: `scale(.985)` + `brightness(.97)` over 180ms on the shared
  `--soft` curve. Tapping still routes to `plant` with the same id.

## 5. Housekeeping
- Removed the now-dead `.pgrid` / `.plantcard` rules (they belonged to this
  screen alone — `.pgrid6`, `.dots` and `.pgdot` are different components and
  stay).

## Verification
- Rendered the screen in headless Chrome: five cards stacked with 120px bands,
  square tucked bottoms, rounded last card, serif title, correct colour per
  character, and the status words reading happy / thirsty / grumpy / happy /
  happy. Dashboard re-rendered afterwards to confirm no shared-style regression.
