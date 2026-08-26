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

---

# Revision 18 — "My plants" redesigned to the mockup (Figma 364:585)

Scope: this screen only. Nothing else touched; the additions to `:root` are new
tokens, not edits to existing ones.

## 0. Design source — Figma MCP (access OK, nothing approximated)
- Mockup frame `364:585` ("concept2", 402×874) — layout, fills, type, radii.
- Assets section `364:776` — the four pot renders, all 1024² transparent PNG.
- Frame `364:585` also carries a leftover `#efefef` gradient scrim
  (`364:658`, copied from the dashboard frame) that paints *behind* the cards
  and is invisible. Ignored.

## 1. Extracted values
| element | value |
|---|---|
| title "My Plants" | Riccione 44.712px (`--t-xl`), `#1E0C00`, left edge x=43, top 69 |
| chevron | 24×24, x=14, `M15 18L9 12L15 6` 2px stroke, **40% opacity** |
| "Add" pill | white, radius 28, padding 15/24, 16px Circular `#050505`, x 310–388 |
| card | x=5 w=392, **h=192 on a 140px step**, `radius 40px 40px 0 0` |
| card shadow | `0 -4px 60px rgba(61,61,61,.2)` — on every card except the first |
| last card | 319px tall |
| status chip | 1px solid black, radius 24, padding `2px 10px 5px`, 16px Circular |
| chip / species / name | card-relative top **14 / 76 / 101**, all at left 43 |
| species | 16px Circular `#050505` at **40%** opacity |
| name | Riccione **22px** (`--t-lg`) `#1E0C00` |
- Card fills are **two-stop gradients**, not flat tints — each runs from the
  character's colour into that pot's glow colour. Taken verbatim as `--g-*`:
  | plant | gradient |
  |---|---|
  | Felix | `126.35deg, #E7D1C9 38.007%, #E2F7CF 115.19%` |
  | Margot | `126.35deg, #B9CBD8 38.007%, #E39B84 115.19%` |
  | Gosha | `126.35deg, #D6D0BA 38.007%, #C3F5EE 115.19%` |
  | Vera | `113.89deg, #C9D5C8 38.007%, #E7FCD6 115.19%` |
  | Basil | `126.35deg, #D3D0DC 38.007%, #F0E6FA 115.19%` — **derived**, same
    construction from the existing `--c-basil` lilac into a pale lilac |
  The base stops confirm the existing palette (Felix `#E7D1C9` vs `--c-felix
  #E3D2CA`, Margot `#B9CBD8` vs `#BCCAD7`, Vera `#C9D5C8` vs `#CBD5C9`); Gosha's
  moves furthest (`#D6D0BA` vs `#E0D3B8`). The mockup values win on this screen;
  the flat `--c-*` tokens keep driving the chat bubbles.
- All four type sizes are already in the locked scale — no new sizes.

## 2. Pot renders
- Exported the four to `assets/pot-{felix,margot,gosha,vera}.webp`, resized
  1024² → 720² (2× the largest on-screen size, 363px) and encoded WebP q86:
  **26–46 KB each vs 210–280 KB as optimised PNG**, with no banding in the glow.
  Quantised PNG was the runner-up (36–45 KB) but risks posterising the gradients.
- **Kept the full uncropped square.** Every placement in the mockup is expressed
  as a crop window over the whole square render, so tight-cropping would have
  invalidated all of that geometry.
- Placement is per-plant, taken from the mockup, and anchored to the card's
  **right** edge rather than its left so it survives the 402→386px device width:
  | plant | window right/top/w/h | render size | offset in window |
  |---|---|---|---|
  | Felix | −46 / 0 / 258 / 218 | 258 | 0, −40 |
  | Margot | 0 / 0 / 265 / 172 (top-right radius 40) | 363 | 0, −144 |
  | Gosha | 9 / −4 / 140 / 187 | 309 | −88, −80 |
  | Vera | −58 / −55 / 282 / 282 | 282 | 0, 0 |
  Felix's and Vera's windows deliberately run past the screen edge, as in the
  mockup; Margot's is clipped to the card's rounded top-right corner.
- **Fork — stacking.** In the mockup each pot is painted *below* the next card,
  so the next card slices it. The brief asks for the opposite: the pot must
  render "above the next card's surface but below that card's own content". A
  z-index can't reach out of its card's stacking context, so **each pot is
  rendered inside the DOM of the card below it**, shifted up one 140px step
  (`PotShot hosted`). Result: surface → overhanging pot → chip/species/name, in
  exactly that order, with no z-index juggling. The last card hosts its own pot.
  The pots never reach the card after next (max overhang 87px < the 140 step),
  so nothing is clipped unintentionally.
- Each pot ends at its render's own flat base, so the overhang reads as a whole
  pot resting on the next card rather than a sliced one. Added
  `drop-shadow(0 10px 16px rgba(30,12,0,.16))` so it visibly floats above that
  card — without it the pots read as telescoped into one another.
- **Basil has no render** (the mockup only draws four cards). His card keeps a
  clean empty right side, as instructed. Vera's pot is hosted by Basil's card,
  so it still gets its full breakout.

## 3. Deviations from the mockup, and why
- **Cards are edge-to-edge (0 margin), not the mockup's 5px inset** — the brief
  asks for full-bleed, and 5px reads as a rendering artefact at this size. The
  stack uses `margin:0 -16px -24px` to break out of the screen's padding; the
  negative bottom lets the last card run off the device edge instead of ending
  on a strip of page background.
- **Last card is 250px, not 319px.** The mockup's last card holds Vera's pot;
  ours holds Basil, who has no render, and 319px of empty lilac was dead space.
- **Chip outline and text use `--ink` (#050505), not pure black** — the design
  system's ink, visually identical at 1px.
- **Basil's species label is suppressed**: his species *is* "Basil", and
  printing "Basil / Basil" reads as a bug. `:has()` moves the name up into the
  freed slot.
- Name is still the first word only ("Margot", not "Margot the Monstera") —
  the mockup shows exactly that.

## 4. Removed / interaction
- The full-width "+ Add plant" bar is gone, replaced by the compact white "Add"
  pill in the header; it still routes to `pair`. The `.addtile` rule is now
  unused but left in place — it is generic and cheap.
- The emotion face disc is gone; the status word moved into the outlined chip.
- Tap still opens `plant` with the same id. Press feedback softened to
  `scale(.99)` + `brightness(.98)` — the previous `.985` was too much travel on
  a card this large. Scrolling is unchanged.

## Verification
- Rendered in headless Chrome at device size, top and fully scrolled: header
  (chevron · serif title · Add pill), five gradient cards on the 140px step,
  chips/species/names at the mockup's offsets, four pots breaking out of their
  cards with the last card bleeding off the bottom edge. Dashboard re-rendered
  afterwards — no regression.

---

# Revision 19 — "My plants": four plants, sized pots, clean cutouts

Scope: this screen only.

## 1. Basil removed
- **Fork — deleted from `PLANTS` rather than filtered out on this screen.** He has
  no pot render, so he cannot appear here; leaving him in the data would make
  Profile advertise a plant the list doesn't show. He appears in no other data
  (`CHAT_INIT` and `NOTIFS` only reference Felix, Margot, Gosha), so the only
  other effect is Profile's count, which derives from `PLANTS.length` and now
  correctly reads "4 plants ›" — no layout change anywhere.
- His `--g-basil` card gradient (created last revision purely for that card) is
  removed; `--c-basil` stays in the bubble palette, harmless and ready if he
  returns.
- The last card (Vera) now takes `flex:1 0 auto`, so it grows to the bottom of
  the screen as in the mockup instead of ending on a fixed height.

## 2. Pot sizing — the real cause of the collisions
- The previous pass placed each pot as the mockup does: a **crop window over the
  full 1024² square**. Because the renders carry wide transparent margins, the
  window is much bigger than the pot, and the geometry was impossible to reason
  about — the pots ended up 178–282px tall and stacked into one another.
- **Now every asset is a tight cutout**, so the pot *is* the box. Placement
  collapses to one set of numbers for all four cards:
  | | value |
  |---|---|
  | height | **165px** (86% of the 192px card) |
  | top | **16px** from its own card's top |
  | right | **22px** — a clear margin, the pot never touches the screen edge |
  | width | each cutout's own aspect at that height: Felix 103 · Margot 113 · Gosha 120 · Vera 106 |
- That puts the pot's bottom at 181px against a 140px card step: **41px, or 25%
  of the pot, laps onto the card below** — inside the brief's 20–30%. Vera's card
  is the tall last one, so hers sits entirely inside it.
- Widths land at 103–120px against a 386px card — roughly the right third.
- **Fork — the right margin is uniform 22px, not the mockup's values.** The
  mockup is inconsistent there (Gosha 14px from the frame, Margot flush to the
  card edge, Felix and Vera bleeding off-screen); the brief asks for a clear
  margin, so one value is used for all four.
- **Fork — pots still overlap each other by ~25px.** With a 140px step, a pot
  that is both "roughly the card's height" and "overlapping 20–30% onto the next
  card" mathematically must overlap the pot below by pot_height + top − 140.
  Only leaf tips are involved; every **face** stays clear, which is the
  constraint the brief actually sets. Sizing down from 178→165px shrank this from
  38px to 25px.

## 3. Gosha's rectangular backdrop — diagnosed
- The source render's alpha bbox was the **entire 1024² frame**: a sub-25 alpha
  wash of stray pixels reaching every edge (the other three bbox tightly around
  their pot). Invisible on its own, but `drop-shadow` casts from *every*
  non-transparent pixel, so it rendered a soft full-frame rectangle around the
  pot. That was the "flat tinted box", not a baked background.
- Fixed by zeroing alpha ≤ 24 before cropping — Gosha's bbox goes
  `(0,0,1024,1024) → (294,265,743,884)`. The same clean-then-crop pass ran on all
  four; the other three were already clean (their bboxes moved by ≤1px).
- Re-exported as tight WebP with alpha at 2× display height (360px tall):
  **18–26 KB each**, down from 26–46 KB, with more pixels on the actual pot.

## 4. Stacking — unchanged and verified
- Chain per card: surface → the previous card's pot (hosted here, shifted up one
  140px step) → this card's own pot if it is the last → chip/species/name at
  `z-index:2`. So each pot paints above its own card and above the next card's
  surface, but under that card's text — the order the brief specifies.
- Verified visually: every face is unobstructed, no pot reaches any card's text
  zone (leftmost pot edge 244px vs text ending ~150px).

## Verification
- Rendered in headless Chrome: four cards, Vera's running to the bottom edge, no
  scroll, no rectangular artefact on Gosha, pots clear of the right edge and of
  each other's faces. Profile re-rendered — reads "4 plants ›", layout untouched.

---

# Revision 20 — "My plants": pots anchored by their base, growing upward

Scope: pot placement on this screen. Nothing else touched.

## 1. What the mockup actually measures (and why the old read was wrong)
Measured each render's opaque geometry against the mockup's placement of it
(`364:585`), rather than reading the crop boxes:

| | pot-body width | pot right edge, from card edge | base, below card top | plant top, above card top |
|---|---|---|---|---|
| Felix | 124.0 | 21.0 | 193.8 | 13.5 |
| Margot | 121.2 | 21.8 | 179.7 | **110.0** |
| Gosha | 135.2 | 13.1 | 182.5 | 4.0 |
| Vera | 126.7 | 19.7 | 200.6 | 27.2 |

Two things fall out of this:
- **The mockup's pot bodies are consistent** — 121–135px wide on a ~20px right
  axis. The previous pass scaled each cutout to a fixed *total height*, so a
  render that is mostly leaves (Margot: her monstera is 110px of foliage above
  the card) came out with a small pot. That was the "Margot is tiny" bug: she was
  being sized by her leaves, not by her pot.
- **The mockup hangs the bases low** (180–200px below a card that is 192 tall)
  and hides the overflow under the next card, while clipping each plant at its
  own card's top. What reads as "the pot ends at the card boundary" is a *crop*,
  not the pot's real base.

## 2. The fix — anchor by the base, grow upward
- Every pot is now scaled so its **body renders exactly 100px wide**, with the
  body's right edge on one axis **24px in from the card edge**. Both come from
  the same per-render measurement (`scale = 100 / pot_body_width_in_source`), so
  the four are identical by construction rather than by eye.
- The **base is the anchor: y=134**, six pixels above the 140px line where the
  next card tucks over. The plant grows up from there, so:
  | | rendered h | rises above its own card |
  |---|---|---|
  | Felix | 168 | 34px (into the header gap) |
  | Margot | 239 | 105px into Felix's card |
  | Gosha | 138 | 4px |
  | Vera | 180 | 46px into Gosha's card |
  Nothing hangs downward any more.
- **Fork — 100px pot bodies, not the mockup's ~126px.** With the base moved up
  ~60px from where the mockup puts it, the mockup's scale would send Margot's
  monstera 167px above her card — past the header and over Felix entirely. 100px
  keeps every plant inside one card's reach while holding the widths equal.
- **Fork — Gosha barely pokes up (4px).** The cactus render is simply short
  (619px of opaque height vs 824 for the others), so at a matched pot width it
  does not reach. This is exactly the mockup's own value (4.0px), so it is left
  alone rather than scaling him out of the shared width.
- **Fork — Vera's base stays at 134 like everyone else.** That leaves her base
  129px clear of the screen bottom ("comfortably above") *and* lets her aloe
  reach into Gosha's card — both halves of the brief — where anchoring her near
  her own card's bottom would have killed the upward overlap.
- Assets re-exported at 2× each pot's new on-screen size (13–38 KB).

## 3. Z-order — one flat context for the whole stack
The requirement "a plant's leaves show over the card above but not over its text"
plus "leaves pass behind the pot above" cannot be done with a z-index per card:
a card with `z-index` becomes a stacking context and traps its pot inside it.
So the stack is now flat:
- `.plcard` carries **no z-index, no transform, no filter** — any of the three
  would create a stacking context. Its gradient moved to an inner `.plsurface`,
  which paints in DOM order, so each card still tucks over the one above it.
- Pots and text get explicit **descending** z: pot `80 − 10i`, text `85 − 10i`.
  Every one clears all surfaces (positive z beats the auto layer), while a lower
  card's pot passes *under* the pot and text of the card above it. That is what
  keeps Margot's leaves behind Felix's pot and off his name.
- `.plhead` gets `z-index:100` so Felix's leaves pass behind the header.
- **Press feedback is now darken-only** (`brightness(.965)` on `.plsurface`) —
  the previous `scale()` on the card would have created a stacking context on
  every tap and re-ordered the pots mid-press.

## 4. Bug found while re-rendering
Splitting the surface out dropped `.plcard:first-child{margin-top:0}`, so the
first card kept the −52px pull and the whole stack rode up under the header,
colliding with the title. Caught by probing live `getBoundingClientRect` values
(card 1 sitting 52px above `.pstack`), restored.

## Verification
- Rendered in headless Chrome and measured: the four pot bodies align on one
  right axis at equal width, every face is unobstructed, Margot's and Vera's
  leaves read as passing behind the pot above them, no pot reaches any text, and
  nothing overlaps downward.

---

# My Plants — pot clipping and spacing (Figma 364:1065)

Fixes the pots spilling out of their cards and onto their neighbours, and
re-derives every inset on the screen from the updated mockup node
(`KLRJyy5g05eK4RgZ99zlqy` → `364:1065`, "concept2"). Nothing outside this
screen was touched.

## 1. The clipping rule
- `.plcard` now carries `overflow:hidden` **plus** its own `border-radius`, so
  each card clips its render to the card's real silhouette — including the 40px
  top corners, not just a square box.
- `.plcard:last-child{overflow:visible}` is the single exception: Vera's aloe
  breaks out 55px above the card's top edge, exactly as the mockup shows.
- **The radius and the shadow moved from `.plsurface` to `.plcard`.** An inner
  element's outward `box-shadow` is eaten by the parent's `overflow:hidden`,
  which would have flattened the whole stack; an element's *own* shadow is
  painted outside its border box and survives its own overflow. The gradient
  stays on `.plsurface` so the press-darken still has something to act on.
- **All z-index on pots and text is gone.** The descending-z scheme existed only
  to referee break-outs between cards. With nothing escaping, paint order is
  plain DOM order: later cards tuck over earlier ones, and within a card the
  chip/species/name follow the render and so sit above it.

## 2. Placement — measured, not eyeballed
Each pot is now a **window** (`.plwin`, `overflow:hidden`) holding the square
1024px source at an offset, which is exactly how the mockup is built. Both boxes
are read off the node and re-expressed against the card's **top-right** corner:

| plant  | mockup frame        | window (right, top) | source inside window |
|--------|---------------------|---------------------|----------------------|
| Felix  | 185,135 · 258×218   | −46, 0              | 258 @ 0, −40         |
| Margot | 132,275 · 265×172   | 0, 0                | 363 @ 0, −144        |
| Gosha  | 248,411 · 140×187   | 9, −4               | 308 @ −88.2, −80.2   |
| Vera   | 173,500 · 282×282   | −58, −55            | 282 @ 0, 0           |

- **Anchoring on the right, not the left, is the one judgement call.** The
  mockup frame is 402px wide; the device here is 386px. Right-anchoring keeps
  every pot's distance from the card edge exact and dumps the 16px difference
  into the empty gap between the text column and the pot, where it is invisible.
  It also makes the break-outs land right: Felix's and Vera's renders are cut by
  the device edge at the same pixel the mockup cuts them at, because the card
  edge and the screen edge shift together.
- **The pot assets were replaced with the mockup's own sources** (the four
  1024px squares from the node, re-exported at 768px webp, same filenames). The
  previous files were hand-trimmed cutouts with no known relationship to the
  square frame, so the mockup's offsets could not have been applied to them
  without guessing the trim. Vera's is a newer render than the one that was in
  `assets/` — the node's image is the one now shipping.
- The `drop-shadow` filter on the render is **removed**: the mockup has none,
  and inside a clipping window it would have drawn a shadow along the crop edge.

## 3. Insets re-checked against the node
- Cards are **not** edge-to-edge: 5px each side (`.pstack` margin −16px → −11px,
  which cancels 16px of screen padding down to 5). Card width follows.
- Already correct, confirmed unchanged: 192px card height on a 140px step
  (−52px overlap), 40px top corners, chip at 43/14 with `2px 10px 5px` padding
  and a 24px radius, species at 43/76, name at 43/101, and all four gradients.
- **Skipped:** node `364:1069`, a full-bleed `#efefef`→transparent rectangle
  sitting behind the first card. It is an ambient wash that is invisible against
  the `#edeee9` page and is not part of the clipping or spacing brief.

## Verification
Headless render plus a live `getBoundingClientRect` probe. Measured on the
device: cards at L=5 R=5, tops 134.8 / 274.8 / 414.8 / 554.8 (mockup 135 / 275 /
415 / 555), height 192, radius 40px, `overflow:hidden` on the first three and
`visible` on the last; every window and inner source landing on the table above
to the tenth of a pixel; chip 43/14, species 43/76, name 43/101 on all four
cards. A zoom on the Felix–Margot seam shows Felix's pot cut flat at his card's
bottom edge and Margot's leaves starting exactly at hers — no bleed either way.

---

# Revision 21 — user avatar · hero gradient · Vlad · Plant Detail rebuild · settings-card rollout

Four work packages in one pass. Every number below was pulled through the Figma
MCP from a named node; anything not in a node is marked **DERIVED** with the
reason.

## 0. Design sources

| what | node | how it was read |
|------|------|-----------------|
| user avatar | `364:1064` (inside section `364:1042`) | `download_assets`, png @4× → 160×160 |
| My Plants | `364:1065` | `get_design_context` + `get_metadata` |
| Vlad's pot render | `415:1419` / `415:1423` (in section `364:1100`) | raw 1024px source off the node |
| Plant Detail | `415:1426` | `get_design_context` + `get_metadata` |

`364:1042` and `364:1100` are **sections**, not single assets — the first holds
the three pot avatars plus the user avatar, the second holds all five pot
renders. The specific child nodes above are the ones the brief actually meant.

---

## A. User avatar — replaced everywhere

- `assets/avatar-user.jpg` **deleted**; `assets/avatar-user.png` added (the node
  export at 4×, 160×160, alpha preserved — the crop is circular in Figma and the
  PNG keeps that, where the old JPEG had a square matte).
- One constant feeds every site: `USER_AVATAR`. It reaches the **collapsed and
  expanded dashboard header** (`.gava` — one element, both states) and **user
  chat messages** (`AVATAR.me`).
- **Profile** was the one place that did *not* use it: the identity row drew a
  striped `.avatarbtn` placeholder. That placeholder is now the real avatar and
  the `.avatarbtn` rule is deleted — it had no other user.
- Swept for others: no remaining reference to the old file, and no other avatar
  affordance in the app.

## B. Expanded hero — the chat gradient

The expanded state used to put **two** things over the chat: a
`saturate(.45) blur(2.5px)` filter on `.gscroll`, and a `.gfade` gradient with
its own softer ramp (`#EFEFEF → 0` at 78%).

- **The blur and the desaturation are both gone.** The brief removes the blur;
  the saturation drop went with it, because the collapsed state has *no* filter
  at all and the brief caps the dimming at "nothing stronger" than the collapsed
  gradient. Leaving the desaturation in would have been a second, stronger
  effect the collapsed state does not have.
- `.gfade` in the open state now carries the collapsed state's **exact**
  geometry and stops: `height:244px`, solid to `96px`, transparent at `244px` —
  where it previously ran 230px tall on a percentage ramp.
- **One deliberate difference:** the tint. Collapsed, the gradient is `--bg`
  (`#EDEEE9`, the page); open, it is `--bg2` (`#EFEFEF`, the sheet the chat now
  sits on). Same intensity and same character, but a gradient must start from
  the surface underneath it or it draws a colour seam at the sheet's top edge.
- `.dash.open .gfade` also forces `opacity:1`. The `.on` flag only fires when
  the chat is scrolled, and the open state freezes the stream — the gradient
  must not blink out just because the scroll position settled.

## C. My Plants

### C1. Vlad, and the break-out rule moving to him

- Fifth plant: **Vlad**, species **"Bonsai"**. The mockup labels him "Aloe"
  (`403:1263`) — a copy-paste from Vera's card. The brief says Bonsai, the render
  is unmistakably a bonsai, so the brief wins. **Logged as an override.**
- `assets/pot-vlad.webp` — the node's own 1024px square, re-exported at 768px
  webp to match the other four.
- Placement, read off the node and re-expressed against the card's top-right
  corner (the convention `SHOT` already used):

| plant | mockup frame | card top / right | window (right, top) | source inside window |
|-------|--------------|------------------|---------------------|----------------------|
| Vera  | 173,555 · 224×227 | 555 / 397 | 0, 0 | 282 @ −0.1, −78.9 |
| Vlad  | 184,651 · 267×267 | 695 / 397 | −54, −44 | 267 @ 0, 0 |

  Vera's frame now starts exactly on her card's top-left, so her render is fully
  clipped like Felix/Margot/Gosha. Vlad's starts **44px above** his card and runs
  **54px past** its right edge — he is the break-out. No CSS change was needed
  for the hand-over: the rule was already `.plcard:last-child{overflow:visible}`,
  and Vlad is now last. Vera simply stopped being the exception.
- Card gradients from the node: Vlad `linear-gradient(113.89deg,#B9CBD8 38.007%,
  #E7FCD6 115.19%)`. **Vera's changed too** — the current frame starts her at
  `#E7D1C9` (dusty pink), not the `#C9D5C8` sage-grey an earlier frame carried.
  Re-read and updated.
- Vlad's card fills the remaining space to the screen bottom via the existing
  `flex:1 0 auto` on the last card.

### C2. Mood chips

`403:1345` builds the chip as **white fill with the whole chip group at 40%
opacity**, 24px radius, `2px 10px 5px` padding — not two separate alphas. That
group opacity is the style: fill and word wash back into the card gradient
together. Implemented literally (`background:#fff; opacity:.4`), replacing the
1px `--ink` outline on a transparent chip.

### C3. Species → name spacing

Node says species at y=215 and name at y=236 against a card top of 135 → **80px
and 101px** from the card top, a 21px step. The build had species at 76px, a
25px step. Species moved down 4px; the name did not move. The
`:not(:has(.plspecies))` fallback moved with it.

---

## D. Plant Detail — rebuilt

The old screen (full-bleed striped placeholder, hearts bond panel, draggable
bottom sheet, snap-strip timeline) is **gone**, along with its CSS: `.pd`,
`.pdphoto`, `.pddim`, `.pdbar`, `.pdname`, `.pdstack`, `.pdstage`, `.pdtl`,
`.pdrail`, `.pdtlstrip`, `.pdtick`, `.sheet*`, `.grabber`, `.tiles`, `.tile`,
`.hearts`, `.resbar`, `.tlink`, and the `Hearts` component. Kept because other
screens still build on them: `.pdspeech`, `.pdplant`, `.pdpot` (create-character
hero and the plant-settings preview), `.pdmenu`.

### D1. Layout, top to bottom

1. **Nav bar** — the same unified header as My Plants: chevron `364:1070`,
   plant name in Riccione at `--t-xl` (44.712px), `more-horizontal` `415:1472`
   on the right. The chevron and ⋯ glyphs were pulled out into shared
   `IconChevronLeft` / `IconMore` components, so My Plants and Plant Detail now
   draw the identical mark.
2. **Species + mood chip row** — species `#050505` @40% at 16px on the left;
   the chip on the right is `415:1487`: fill and `drop-shadow(0 0 10px …)` in the
   **same hex**, which is what produces the glow. Felix's is `#CCF5B0`.
3. **Speech blob** — `415:1463`'s actual vector path, added to `BLOB` as
   `speech` and painted by the existing `blobBg()`. Symmetric, white, no quote
   marks (the old screen wrapped the line in `“ ”`; the shape says speech now).
   The soft `#EFEFEF → transparent @78%` scrim behind it is `415:1430`.
4. **Video stage** — see D2.
5. **Timeline scrub** — see D3.
6. **Info cards** — the canonical settings-card style, see D4.

### D2. The video stage is built video-first

`415:1434` frames the pot render as a **square source 1.4154× the frame width**,
centred, nudged up 4.33%, inside a 402×551 window. Those exact ratios drive the
stage, so the layout is independent of what is inside it.

The element is a real `<video>` with `poster={that plant's pot render}` and
`src="assets/plant-<id>.mp4"`. While no mp4 exists the poster simply stays up —
a `<video>` keeps showing its poster when the media fails to load — so the
screen is complete today and goes live the moment the file is dropped in, with
**no code change**. Every plant gets its own filename, so they can arrive one at
a time.

- **DERIVED:** a gentle `rgba(30,12,0,0→.22)` scrim over the stage's bottom
  150px. The mockup draws the timeline in pure white directly over the pot's
  pale speckled base, where it is barely legible; over an arbitrary video frame
  it could vanish entirely. The scrim is the smallest thing that guarantees the
  white rail reads on any frame.

### D3. Timeline scrubbing

- Rail spans the stage; **"now" is the right end**, and the screen opens there.
  (The mockup draws the rail stopping at x=201 with "now" mid-frame — that is a
  half-drawn artboard, and the brief states "now at the right end". Brief wins.)
- **Position maps linearly to `currentTime`**: `currentTime = frac × duration`,
  so the rightmost pixel is the final frame and dragging left runs the growth
  backwards. Verified against a stubbed 10s duration: 60% → 6.000s, 100% → 10s.
- **Smoothing:** pointermove fires far faster than a video can seek, so the
  handler only records the wanted fraction and a single `requestAnimationFrame`
  applies it. One seek per frame, no queue.
- Pointer events (one code path for touch and mouse) with
  `setPointerCapture` + `touch-action:none`, so a drag never scrolls the page
  under it. A wheel/trackpad gesture over the rail scrubs too.
- **Labels — a judgement call.** The mockup shows two words, "thriving" and
  "now", because it only draws two dots. The app has six milestones and six
  words will not fit across a phone. Resolution: **all six dots are drawn, and
  exactly two are labelled — the one you are standing on, and "now"**. That
  reproduces the mockup's read at every scrub position (at "thriving" you get
  precisely the mockup) and degrades to a single "now" at the right end.
- Active dot: solid white with a soft white ring; the rest white @55%; the rail
  fills white up to the handle.

### D4. Info cards

All measured off the node, all in the new `.scard` style:

- **Your bond** — `415:1433`, 392×117 @40px radius, 24px inset. Title Riccione
  22px; `Level 4 · Close friends` as 16px @40% either side of a 4px dot @20%;
  progress track `#EDEEE9` / fill `#C9D6BC`, 8px tall, 40px radius. The fill
  width is data-driven (`bondPct`) rather than the mockup's frozen 76.5%.
- **Three stat cards** — 128×125 @40px radius on a 5px gap (mockup: 4px between
  392px-wide edges; 5px matches the vertical rhythm and the 1px is invisible).
  Label 16px @40% at 24px from the top, value centred below. Values are Riccione
  at 22px, **except `64%` which the mockup sets at 44.712px** — kept, because
  short values earn the display size and the mockup is explicit about it.
- **Reservoir fill** — `415:1518` is a wave-topped block at 50% opacity with a
  `#B9CBD8 → white` vertical gradient. Shipped as that exact path in a data-URI
  SVG, stretched so **its height is the reservoir percentage**. In the mockup
  the shape happens to sit at ~71% against a stated 64%; driving it from the
  number is the honest reading of "a soft fill level indication inside".
- **Auto-watering** — `415:1512`, 392×74, title Riccione 22px, switch on the
  right.

### D5. Navigation preserved (and one addition)

- From **My Plants** — unchanged, `nav.go("plant",{id})`.
- From **the chat** — there was no such path before; the brief asks for one.
  A plant's avatar in the chat now opens that plant's card. Smallest possible
  affordance, and it reads naturally next to the name.
- `alertDone` → Felix's card, and the flow-index shortcut, both unchanged.
- The old screen reached `waterConfig` through a "Configure schedule ›" text
  link inside the auto-watering card. The new card is title + switch only, so
  that link is gone — **`waterConfig` would have become unreachable**. It moved
  into the ⋯ menu as "Watering schedule", next to "Personality & Settings".

---

## D2 (global). Settings-card style rollout

The mockup's card style is now the app's canonical settings/data surface.

- `--r-card` **24px → 40px** (from `415:1433/1499/1512`). This flows to `.card`,
  `.pdmenu`, `.push`, `.camguide`.
- `--r-sub` **28px** — **DERIVED**, for cards nested *inside* a card
  (`.opt`, `.ptile`). The mockup's 40px is measured on a 125px-tall card; on a
  58px preset tile it would round into a pill and lose the card read.
- `.card` padding **16px → 20px 22px**. The mockup's inset is 24px, but `.card`
  also carries dense 48px-photo rows across the add-plant flow; 22px lands close
  to the mockup without forcing those rows to re-wrap. Cards explicitly tagged
  `.scard` get the full 24px.
- **Type roles**, applied via `.card.scard`: row titles (`b`) become Riccione
  22px `#1E0C00`; captions (`small.muted`) become 16px black @40%. Tagged on
  **Profile** (identity, both chat-settings cards, the language list),
  **Plant Settings** (identity, active hours, messages), **Notifications** (all
  three rows) and **Diagnosis** (both readouts). No markup around them moved —
  the rollout changes surface, radius and type role only, as briefed.
- **The switch was unified too.** `.toggle` was 44×26 with a `--sage-deep` track;
  it is now the mockup's control (`415:1523/1524`): 50×28, 40px radius, `--bg`
  track, 22px knob, `#C9D6BC` when on. One switch everywhere — Profile, plant
  settings and the new auto-watering card all draw the same object.
  **DERIVED:** the *off* knob is `#C6C7C0` — the mockup only ever draws the on
  state, and `--box` on an `--bg` track has no contrast at all.

### Emotion glow colours — DERIVED

The mood chip's tint has to follow the plant's emotion, and the node only gives
one value. `EMO.content = #CCF5B0` is measured; the other nine are derived in
the same bright-pastel register, each pulling toward the colour that emotion
already wears elsewhere in the app (warm coral for thirst, cool teal for the
grump, and so on). Listed inline in the `EMO` table.

---

## Verification

Headless Chrome over CDP against a local server. Boot clean, **zero console
errors and zero exceptions** on every screen.

- **A** — new avatar renders in the collapsed header, the expanded header, the
  user's chat bubble and the Profile identity row; no reference to the old file
  anywhere in the tree.
- **B** — expanded state screenshotted: no blur, no desaturation, chat legible
  under the same 96/244 ramp the collapsed state uses.
- **C** — five cards, Vlad last and labelled "Bonsai", his bonsai breaking out
  over Vera's card while Vera's aloe is cut flat at her own card edge; chips
  soft-filled with no outline; species/name step measured at 21px.
- **D** — the rebuilt screen matches the mockup top to bottom. Scrub driven
  through synthetic pointer events: dragging to 42% moves the fill to 42%, lights
  dot 2 of 6, and shows exactly "young plant" + "now"; against a stubbed 10s
  duration, 60% → `currentTime` 6.000s and the right end → 10s (final frame).
  Entered from My Plants, from a chat avatar (Margot, whose chip correctly glows
  coral rather than green) and from the flow index. ⋯ menu reaches both
  Personality & Settings and Watering schedule.
- **D2** — Profile, Plant Settings and Notifications re-rendered on the new
  surface with their layouts and content untouched.

**Not shipped:** any `.mp4`. The stage is wired and waiting; dropping
`assets/plant-felix.mp4` in is the whole integration.

---

# Revision 22 — Plant Detail: full-bleed stage, raised bottom interface

Plant Detail only; no other screen's layout or content was touched.

## 0. The mockup moved — and it is a different node

The brief pointed at `415:1426`, but that node has itself changed since rev 21
(the top scrim `415:1430` is gone, the pot frame is now 515×500 at −57,217, and
the whole lower block sits 40px higher). The node that actually describes what
the brief asks for is **`417:1537`**, reached from the placeholder link — a
**402×874** frame, i.e. a whole iPhone 16 Pro logical screen, with the photo
running the full height. Everything below is measured off `417:1537`; `415:1426`
agrees with it on every shared value, so nothing is in conflict.

## 1. The placeholder image

`417:1537` → `image 68`, an 848×1264 photograph of the pot on a windowsill.
Exported to `assets/stage-felix.webp` (cwebp q84, 67KB — the same treatment the
pot renders get).

It is a real photograph, not a cutout on transparency, which is the whole reason
the layout could go full-bleed. The old poster was `pot-<id>.webp`, a square
cutout that only ever worked inside a centred box.

- **Video-first is unchanged.** The `<video>` still carries
  `src="assets/plant-<id>.mp4"` and is postered by the still, so dropping the
  film in is still the entire integration.
- **One still, all plants.** `STAGE` is a map with Felix in it and
  `stageFor(id)` falling back to him. Felix is the sample the mockup was drawn
  for; the others borrow his frame until their own still or film lands, and the
  swap is one line each. The alternative — leaving the other four on their square
  pot cutouts — would have meant two different stage layouts in one screen.

## 2. Full-bleed layout

The screen is now **two layers**: a photo that never moves, and one scroller
that rides over it.

### The photo layer (`.pdbg`)

- `top:-48px` reaches back up **behind the status bar**, so the bleed starts at
  the true top of the screen rather than under the device chrome. `.device`'s own
  `overflow:hidden` + 44px radius clip it back to the phone.
- **The status bar goes white on this screen and only this screen.**
  `StatusBar`'s glyphs were changed from a hard-coded `#050505` to
  `currentColor` — a no-op everywhere, since every other screen inherits
  `--ink` — and one rule, `.device:has(.pdscreen) .statusbar{color:#fff}`, flips
  it. `IconChevronLeft` and `IconMore` got the same treatment; Figma returns both
  glyphs as `stroke="white"` in this frame and as ink in `364:1065`, so
  `currentColor` is the honest encoding of that.
- **Two image layers, as Figma stacks them.** `417:1585` is the same source
  blurred 5px at 465×886 (`left −5.22%`, `width 115.67%`, `height 101.37%`);
  `418:1642` is the sharp one at 586×874 (`left −22.89%`, `width 145.77%`,
  `height 100%`). Today the sharp layer is opaque and hides the blurred one
  completely — but the moment an mp4 whose aspect differs from the frame drops
  in, that backdrop is what fills the gaps. It costs nothing and it is in the
  mockup, so it ships.
- Framing check: the source is 848×1264 (0.6709) and the sharp box is 0.6705, so
  Figma shows it uncropped. At 386×818 the box is 0.688, so `object-fit:cover`
  trims ~10px top and bottom and nothing horizontally. The pot lands at 58.4% of
  the screen width against the mockup's 58.5%.

### The scrims — the brief said "check", and the mockup has two

- **Top, `417:1587`:** `linear-gradient(180deg, rgba(0,0,0,.7) 0%,
  rgba(0,0,0,0) 103.57%)`, 266px tall, full width. Not subtle at the very top —
  70% black — but it fades fast, and it is what carries the white nav over a
  bright window.
- **Bottom, `417:1589`:** drawn flipped in Figma (`-scale-y-100` over a
  `black → transparent @78.061%` ramp at 20% opacity), which unflips to
  `rgba(0,0,0,0) 21.939% → rgba(0,0,0,.2) 100%`. 271px tall, running 5px past
  the frame's bottom edge.
- Both sit **behind** all content, exactly as the Figma layer order has them —
  including behind the speech bubble, whose top the scrim therefore tints
  slightly. That is the mockup.
- **The derived scrim from rev 21 is gone.** Last revision invented a
  `rgba(30,12,0,0→.22)` wash to keep the white timeline legible; the mockup's own
  bottom scrim does that job, so a guess was replaced by a measurement.

### Type over the photo

Title, chevron, ⋯ , species and timeline labels are all white (`417:1538`,
`417:1581`, `417:1555/1556`). The species lost its 40% black — at 40% over a
photo it would have disappeared. Species and timeline labels carry a soft
text-shadow, which the mockup does not have: **DERIVED**, and the reason is that
the mockup only has to survive one photograph while this has to survive any
video frame.

**Fixed a rev-21 miss:** the species label was sitting at x=14, under the
chevron. The mockup puts it at **43**, under the title. A 29px indent on `.pdsp`
corrects it without moving the right-aligned mood chip.

## 3. Bottom interface — raised

### Two anchors, and why

The mockup frame is **874** tall; this prototype's device is **818**. Mapping
everything from the top would push the stat cards 44px above the bottom edge
instead of the mockup's 12, and the "peeking" the brief asks for would read as a
half-shown row. So:

- **Top block** (nav, species + chip, bubble) is **top-anchored** — mockup y
  lands on device y unchanged.
- **Timeline and cards** are **bottom-anchored** — measured up from the frame's
  bottom edge.

| element | mockup y | up from frame bottom | device y |
|---|---|---|---|
| title | 69 | — | 69 |
| chip / species | 132 / 134 | — | 132 / 134 |
| bubble | 161 (h 116) | — | 161 |
| timeline labels | 684 | **190** | 628 |
| timeline rail | 715 | **159** | 659 |
| "Your bond" card | 740 (h 117) | **134** | 684 |
| stat cards | 862 (h 125) | **12** | 806 |
| auto-watering | 992 (h 74) | — | 936 |

The 56px difference is absorbed by the photo, which is the one element that
does not care how tall the screen is.

The spacer between the two blocks is one constant:
`height: calc(100% - 411px)`, where 411 = 221 (the top block, pinned to an exact
height so a longer greeting cannot drift the layout) + 190 (the labels' offset
from the bottom). Everything else is the mockup's own internal gaps: labels→dots
7px, dots→bond card 19px, card→card 5px.

### The timeline scrolls with the cards

The mockup fixes the rail **25px above the bond card** (rail 715, dots to 721,
card 740). Making it a flow child of the scrolling block preserves that
relationship at *every* scroll position; pinning it to the photo instead would
have held it only at rest and then let the cards bury it. It keeps its own
pointer capture and `touch-action:none`, so a scrub never scrolls the sheet.

- **Wheel scrubbing narrowed to horizontal.** The sheet under the timeline is a
  scroller now, so a vertical wheel over the rail must scroll the page rather
  than scrub — otherwise the cursor lands on a 40px strip that traps the scroll.

### Only the cards area scrolls

`.pdtop` is `position:sticky; top:0`, which makes the brief's wording literally
true: the nav bar, species row and bubble hold their rest position for the whole
218px scroll range while the timeline and cards climb the photo.

Without it, the back chevron scrolled away entirely and the white bubble rode up
under the white title — both were visible in the first render. Sticky was the
right tool over a fixed overlay because the scroller still covers the whole
screen, so a wheel or a drag anywhere scrolls, and the chevron and ⋯ stay
tappable inside it. `top:0` (not `8px`) because the offset resolves against the
scroller's content box, which already starts below its 8px padding.

The lower block tops out at device 410 against the top block's 277, so the two
never collide and the sticky layer needs no scrim of its own.

## 4. Held deliberately

**The speech bubble stays centred.** The mockup puts it at x=17 in a 402 frame,
i.e. noticeably left of centre. The previous brief specified "centered" in as
many words and this one does not revisit the bubble, so the explicit instruction
stands over the implicit geometry.

## Verification

Headless Chrome over CDP, zero console errors and zero exceptions.

Measured on the device (386×818), against the targets in the table above:
title top **69** / left **43**; chevron left **14**; chip + species row top
**131.8** with species at left **43** and the chip's right edge at **372**
(mockup 14 from the frame edge); bubble top **160.8**, height **116**; labels top
**628** = **190** up from the bottom; rail **655/163**; bond card **684** = **134**
up, width 376 at a 5px inset; stat cards **806** = **12** up — the peek. Photo
layer measured at top **0**, height **818**: the full device, status bar included,
with the bar's computed colour `rgb(255,255,255)` there and `rgb(5,5,5)` on My
Plants.

Scroll range **218px**. At full scroll the top block is unmoved (title still 69,
bubble still 160.8) while the lower block has travelled to 410 / 466 and the
auto-watering card sits 24px off the bottom edge. Scrub re-checked in the new
layout: dragging to 33% moves the fill to 33%, lights dot 2 of 6 and shows
"young plant" + "now". ⋯ menu reaches both destinations.

No regression on the screens that share the changed components: My Plants renders
its chevron at `rgb(5,5,5)` and its status bar unchanged; Notifications, Profile
and the dashboard re-rendered clean.

---

# Revision 23 — expanded hero: the chat fade cut back to a top-edge veil

One rule changed: `.dash.open .gfade`. Nothing else on the dashboard or any
other screen was touched.

## What was actually wrong

Rev 21 removed the blur and gave the open state the collapsed gradient's *exact*
stops — 96px solid, transparent at 244px. Identical intensity was the brief then,
but it does not survive the move: collapsed, that gradient hangs off the top of a
full-height scroll; open, it hangs off a sheet that is only 336px tall. The same
244px covers most of it.

Measured on the device before touching anything:

| | device y |
|---|---|
| sheet top | 482 |
| composer top | 730 → readable chat is **248px** |
| alert bubble | **541 → 707** |
| old fade | 482 → 726, solid to **578** |

So the gradient was at **full opacity** where the alert bubble started, and still
ramping across the whole of it and its CTA. That is the washout.

## The change

`height:244px → 52px`, stops `96px/244px → 14px/52px`.

- **52px is set by the alert bubble, not by a percentage.** The brief asks for
  roughly the top 20–25% *and* for the alert bubble and its CTA to read at full
  clarity. Those two only agree if the ramp finishes before device 541: 52px ends
  at 534, seven pixels clear. It is 21% of the 248px readable chat.
  Where the two readings of "visible chat" disagreed — 20–25% of the whole 336px
  sheet would be 67–84px, which lands *inside* the alert bubble — the explicit
  "no dimming on the alert bubble" requirement won.
- **Character kept, proportion softened.** Still an opaque cap then a ramp to
  nothing, still tinted `--bg2` so the sheet's top edge has no seam, still no
  blur. The cap is now 27% of the gradient against the collapsed 39% — shorter
  *and* softer, as asked.
- The 14px cap is kept rather than dropped to zero: the sheet's top corners are
  36px, and a hairline of solid keeps the first message from appearing to touch
  the rounded edge. It is invisible as a wash, since the sheet under it is the
  same `--bg2`.
- `height` was added to both `.gfade` transitions. The two states used to share
  one height so it never animated; now that they differ, it has to travel with
  `top` or the fade would snap mid-morph.

**Unchanged:** the collapsed gradient (still 244px, `--bg`, 96/244), verified by
computed style after collapsing again.

## Verification

Headless Chrome over CDP, no console errors.

Open state: fade measured at 482 → 534, 52px. Computed alpha from its own stops
is **0 at the alert bubble's top (541)** and **0 at its CTA (660)** — no dimming
anywhere in the middle or bottom of the chat. Only the tail of the message above,
clipped at the sheet edge, sits under the veil. Collapsed state re-measured at
244px with `rgb(237,238,233)` to 96px — untouched.

---

# Revision 24 — Plant Detail: the real films and the scroll transition

Scope was the Plant Detail stage only. No other screen was touched. The one
change outside `PlantDetail` is the flow-index shortcut label "Plant card",
which now opens Vlad instead of Felix — see below.

## What replaced what

The stage was a single `<video>` pointed at `assets/plant-<id>.mp4`, a file that
never existed, so in practice it was a poster: `assets/stage-felix.webp`, blurred
once as a backdrop and printed sharp on top. All of that is gone — `STAGE`,
`stageFor`, `.pdbgblur` and the `poster` attribute with it. In its place two real
films that actually play:

| | file | role |
|---|---|---|
| still | `assets/video/still-vlad.mp4` | the "now" state, looping, muted, autoplay |
| growth | `assets/video/growth-vlad.mp4` | the timeline, never played — seeked |

Both are 976×2124, 5.042s. Both `muted playsInline preload="auto"`, no controls.

## The direction of the growth film — the file overrode the brief

The brief said to start the growth film on its **last** frame and run toward the
first as the scrub goes back in time. The file is authored the other way round.
Decoded and compared frame by frame:

| | growth-vlad frame 0 | growth-vlad last frame |
|---|---|---|
| | fully grown bonsai | seedling |

and growth frame 0 against still-vlad frame 0 is a **0.51/255 mean pixel
difference** — the same image. The file runs grown → sprout.

**Decision: "now" maps to `t = 0`, and scrubbing back in time runs *forward*
through the file — `t = (1 - pos) * duration`.** Rationale: the brief's *intent*
is unambiguous (right = now = grown, left = earlier = smaller) and it is the
intent that has to be true on screen; taking the frame index literally would have
made the plant *grow* as you scrubbed into the past, which is the one outcome
the brief rules out. Logged here because it is a deliberate contradiction of a
written instruction.

The happy side effect: because growth's t=0 *is* the still film's image, the
cross-fade between them has nothing to hide.

## Vertical positioning — read off the mockup, not guessed

Figma node `437:93` holds the two frames side by side. Both contain a rect named
`still 1`, and those rects are the film:

| frame | rect | x | **y** | w × h |
|---|---|---|---|---|
| `428:1782` default | `437:88` | -27 | **-184** | 457 × 995 |
| `437:8` growth | `437:90` | -27 | **0** | 457 × 995 |

457 × 995 is ratio .45930; the sources are 976 × 2124, ratio .45951. The rect
**is** the film at scale — nothing is cropped, and `object-fit:cover` was
dropped along with the old `left:-22.89%; width:145.77%`.

- Horizontal is proportional: `-27/402 = -6.716%`, `457/402 = 113.681%` of the
  device width, so it survives the 402px mockup → 386px device difference.
- Vertical is **literal px**, -184 and 0, following this screen's existing
  convention that mockup y lands on device y unchanged (the same rule that pins
  the title at 69 and the bubble at 161). Measured in the browser: 438.8 × 954.9
  at left -25.9, top -184 / 0.
- `aspect-ratio:976/2124` is declared so the box is correct before metadata
  lands and the first paint cannot flash a wrong height.
- Y rides on `transform`, not `top` — compositor-only.

## The transition

A five-state machine held in a ref (`still → down → growth → fadeOut → up →
still`); only `sy` (the still film's Y) and `gOn` (the growth film's opacity) are
React state, because re-rendering on every beat would only fight the scrub.

**Going back:** glide the still film -184 → 0 over **420ms** `cubic-bezier(.33,
.72,.26,1)`, and only once it has landed cross-fade the growth film in over
**180ms**. Both films sit at 0 at the swap and are showing the same frame, so
there is nothing to flash or jump. Then the still film pauses.

**Coming home:** the same three beats reversed — the growth film has to reach
its "now" frame first, then it fades out over 180ms, and only then does the
still film climb back to -184 and resume looping.

Decisions inside that:

- **`shown` is frozen at "now" until the growth film is actually on screen.**
  The scrub keeps two numbers: `want` (where the finger is, which drives the rail
  instantly) and `shown` (where the film is, eased toward `want` at 0.22/frame in
  a rAF loop). Freezing `shown` during the slide is what *guarantees* the
  cross-fade happens with both films on the same frame however far the finger
  has already travelled — and it means the growth film visibly rewinds into
  position after the swap instead of jump-cutting. That reads better than the
  alternative and it is why the hand-over cannot be caught out by a fast drag.
- **420ms as a fixed-duration CSS transition, and the swap on a matching
  timer.** A CSS transition interrupted half-way re-runs over its full duration
  from wherever it stands, so the timer is exact even on a reversal — no
  `transitionend` bookkeeping needed.
- **Leaving "now" starts the sequence; a bare tap *on* "now" does not.** The
  trigger is `pos < 0.9994`, not pointerdown. Rationale: the brief says "starts
  scrubbing back from now", and touching the rail without moving is not that.
- **Reversal is handled at every beat.** Turn round during the slide down and
  the growth film is never shown at all — the still film just climbs back. Turn
  round inside the return cross-fade and the growth film fades straight back in
  at 0, no positional jump, because the still film has not started climbing yet.
- **rAF-coalesced seeking, ~3ms dead-band.** A pointermove fires far more often
  than a video can seek. Also the last 40ms of duration is held back, because
  seeking to the exact tail is unreliable across browsers.
- `preload="auto"` on both: ~9.4MB on entering the screen. Accepted — instant
  scrubbing is the point of the screen and this is a prototype, not a shipping
  app.

## Vlad's films everywhere, and the shortcut now opens Vlad

There is footage for exactly one plant. `FILM` is a flat pair of paths, not a
per-plant map, so this screen plays Vlad whichever card opened it — as the brief
allows for the prototype. The map is the seam where per-plant films would land.

The consequence was that the flow index's "Plant card" opened **Felix**, so the
screen read "Felix / Ficus" over Vlad's bonsai. **The shortcut and the screen's
own no-id fallback now point at `vlad`.** Rationale: Vlad is Bonsai, which is
what the mockup and the footage both show; leaving it on Felix would have made
the one screen this revision is about contradict itself. Nothing about Felix or
any other screen changed — only which id this shortcut passes.

## The strip under the film

At the -184 framing the film ends 95px short of the bottom edge. Below it,
`.pdbg` used to paint `--bg` (#EDEEE9), which would have shown as a band at the
very end of the scroll. It now paints
`linear-gradient(90deg,#D4D0C8,#CDC9BD)` — sampled off the mp4's own bottom row,
left and right — so the sliver reads as more studio floor rather than as app
chrome. Verified at `scrollTop 217/218`: no visible seam.

## Verification

Headless Chrome over CDP, driven with real `Input.dispatchMouseEvent` so the
pointer-capture path is exercised. **No console errors, no exceptions.**

| step | still film | growth film |
|---|---|---|
| on load | top -184, playing | top 0, opacity 0, t 0 |
| pos 0.55 | top 0, paused | opacity 1, **t 2.251** (= (1-0.55)×5.002) |
| pos 0.02 | top 0, paused | **t 4.902** (= (1-0.02)×5.002) |
| back at now | top -184, playing | opacity 0, t ≈ 0 |

Edge cases, all clean: bail out mid-slide (still reversed from -39.7 → -157.6 →
-184, growth never shown); turn round inside the return cross-fade (recovered to
growth at t 3.50 for pos 0.30); tap exactly on "now" (nothing moves); scroll to
the bottom at the default framing (films unmoved).

One note for anyone testing locally: **python's `http.server` does not support
HTTP Range requests**, and without them Chrome silently refuses to seek the mp4 —
`currentTime` is written and reads back 0. The scrub looks broken and is not.
Serve with something that answers `206`, as Vercel does.

## Shipped

Commit `18ce159` → `origin/main`. Vercel production deployment
`dpl_BTqdwsNYKdBREC9RL1pdnHXxYbLV`, aliased to
**https://bloomling-wireframes.vercel.app** — unchanged URL, `bloomling-wireframes.vercel.app`.

Re-verified against the live site, not just locally: `accept-ranges: bytes` and
`206` on both mp4s, no console errors, films at -184 / 0 measured at 438.8 ×
954.9, growth **t 2.251 at pos 0.55** and **t 4.902 at pos 0.02**, and a clean
return to the looping still film at -184.

The CLI needed `--scope wannabe-course`; without it the newest CLI answers
"Not authorized" even though `vercel whoami` succeeds.

---

# Revision 25 — Plant Detail: interface polish

Scope was the Plant Detail screen. No other screen was touched. Reference is
Figma `428:1782`, which has been **redrawn** since rev 24 — the old label nodes
(`428:1799` / `428:1800`) no longer exist and several values changed.

## 1 — Header and mood chip

The top gradient is gone, and with it the reason the nav was ever white. The
mockup now sets the whole row in ink over the bright film, and every value it
asks for was already this file's default:

| element | mockup (428:1782) | how it lands |
|---|---|---|
| chevron `428:1784` | black, **opacity .4** | `.plback`'s own rest state |
| title `428:1783` | `#1E0C00`, Riccione Light **44.712px** | `--serif-ink` at `--t-xl` (44.7px) |
| ⋯ `428:1809` | black, **opacity .4** | had to be added — see below |
| species `428:1825` | `#050505` at **40%**, Circular Book 16px | `--ink` + `opacity:.4` |
| chip `428:1816` | `#CCF5B0`, glow `0 0 15px` same hex, radius 24, **pad 5/14/8**, 16px black, 73×33 | `.moodchip` |

So the change is mostly **deletion**: `.pdscrimt`, `.pdscreen .pltitle{color:#fff}`
and `.pdscreen .plback,.pdscreen .plmore{opacity:1;color:#fff}` all came out and
the light-screen styles simply apply.

Three things did not fall out for free:

- **`.plmore` never carried an opacity.** `.plback` has `opacity:.4` in the
  shared rule but `.plmore` only ever had an `:active` state — the deleted
  override had been supplying its resting value. Set to `.4` on `.pdhead .plmore`
  rather than on the shared rule, so My Plants keeps its own appearance.
- **The chip grew 27px → 33px and the mockup moved the row up to meet it.**
  Title 69 + 52 = 121 is exactly the chip's top, so `.pdrow`'s `margin-top`
  went 11px → 0. The species sits 5px lower again (mockup 126), which is now
  `.pdsp`'s `padding-top`.
- **The speech blob had to be re-derived** because the row above it moved.
  `.pdbubwrap` margin-top 2px → 7px puts its top on the mockup's 161.

Measured after the change: title 43/69 `#1E0C00`; chevron x14 black op .4; ⋯ 14
from the right edge, op .4; species x43 `#050505` op .4; chip 14 from the right,
y 120.8, 72.4×33, `#CCF5B0`, pad `5px 14px 8px`, `drop-shadow(... 0 0 15px)`.

The chevron sits at y 82.9 against the mockup's 85. It is centred on the title
row; the mockup nudges it 2px below centre. 2px, left as centred.

## 2 — Timeline edge fade

Both ends are now **always drawn**. That is the point of the treatment: "seed"
and "now" are permanent anchors that state the range, and their opacity says
whether you are standing on one. The stop you are actually on is still drawn
when it is neither end, so the middle of the rail stays legible.

**The fade level is a flat opacity, not a gradient.** The brief said "soft
gradient dissolve", but the file is unambiguous: `441:576` ("now", thumb on it)
is white at full opacity and `441:577` ("sprout", thumb away) is the same white
at `opacity-60`. The brief also said to match the mockup exactly, and at 16px a
masked dissolve would read as a visibly different effect. The mockup won.

`transition:opacity 180ms ease` on the span makes it live — the value changes
as `idx` crosses a stop mid-drag, and the edge labels are never unmounted so the
transition always has something to animate. The old `text-shadow` came off with
the scrim; the mockup's labels carry none.

Verified by driving the thumb: at now `[seed .6, now 1]`; mid-rail
`[seed .6, established 1, now .6]`; at seed `[seed 1, now .6]`; and back.

**Observed but deliberately not done:** the mockup has also redrawn the rail
itself — four 10px dots plus a distinct 16px thumb (`441:578`–`441:583`), where
the code still has six equal dots. That is a different change from the edge
fade, it would alter what MILESTONES means, and it was not in the four numbered
items. Left for a future pass.

## 3 — Progressive blur under the cards

**The mockup shows no blur, and that is the finding that shaped the
implementation.** Aligning `428:1782`'s film against the raw mp4 frame in the
film's own coordinates (x -27, y -184, 457×995) and comparing sharpness band by
band gives ratios of **0.93 / 0.98 / 0.99** — the mockup's film is not blurred
anywhere. It is drawn *at rest*, which is precisely the moment before the cards
"scroll up and overlap the video".

So the blur is a **scroll state**, and it has two independent axes:

- **Position** tracks the cards live: `.pdblur`'s `top` is written every scroll
  frame as the cards' measured top edge minus `PD_BLUR_LEAD`.
- **Strength** ramps with scroll progress, from nothing at rest to full at the
  bottom. That is the only reading that satisfies the brief and the mockup at
  once, and it makes the effect something you watch arrive rather than a static
  soft-focus.

Four stacked `backdrop-filter` layers, each masked in over its own band, radii
6 / 10 / 18 / 34px at offsets 0 / 30 / 120 / 250 with ramps 80 / 90 / 140 / 170.
Sibling backdrop-filters compose, so the radii add as `√Σr²` and four discrete
steps read as one curve: **≈7px at the cards' own edge, ≈16px 200px under,
≈37px 400px under.**

Decisions inside that:

- **It lives in `.pdbg`, under the films, not in the scroller.** The backdrop it
  samples is then unambiguously the film — and the timeline text, which rides in
  `.pdscroll` above it, stays perfectly sharp while the film behind it softens.
  Confirmed in the A/B render.
- **`PD_BLUR_LEAD = 72px`,** i.e. the ramp starts above the cards rather than at
  them. The cards are opaque `#FFF`: a ramp beginning exactly at their edge
  would be both invisible *and* a discontinuity on the boundary, which is the
  one thing "no hard blur edge" rules out. 72px puts the first layer ~90% on
  where the cards arrive.
- **Opacity goes on each layer, never on the wrapper.** An element with
  `opacity < 1` forms a Backdrop Root for its descendants, so fading the parent
  would have left the children with an empty backdrop and blurred nothing. On
  the filtered element itself it is exactly the cross-fade wanted.
- The first tuning (2/4/9/18 at lead 56) measured real but was too weak to read
  — an A/B render at full scroll showed almost no difference. Retuned upward
  until the pot rim and soil visibly dissolve into the card stack.

Verified: `blurTop` is `cardsTop − 72` at every scroll position (612/684,
502/574, 395/467) and strength runs 0.00 → 0.50 → 1.00.

## 4 — Scroll bounce restricted to the cards

The header was a `position:sticky` block **inside** the scroller. Sticky holds a
rest position only within the scroll range, and a rubber-band overscroll is
outside it — which is exactly why the whole interface travelled with the bounce.
No amount of tuning fixes that; the header has to leave the scroller.

`.pdhead` is now its own absolutely-positioned layer in `.pdscreen`, holding the
nav row, the species + chip row, the speech blob and the ⋯ menu. `.pdscroll`
carries only the spacer and the timeline + cards.

- 8px/16px on `.pdhead` reproduce the scroller's own padding, so nothing moved.
- `pointer-events:none` on the layer (re-enabled on the chevron, the ⋯ and the
  menu) hands the empty header area back to the scroller underneath, so a drag
  anywhere on the film still scrolls the cards.
- `.pdspacer` went from `calc(100% - 411px)` to `calc(100% - 190px)`: the 221px
  the header used to occupy came straight back out of the spacer, so total
  content height and the scroll range (218px) are unchanged.
- `overscroll-behavior-y:contain` keeps the bounce from chaining out to the page.
- The menu is now static too, which it was not before.

**The timeline stays with the cards** rather than with the header. It is not
named in the brief's "must not move" list, it has sat 19px above the bond card
at every scroll position since the layout was built, and separating them would
break that relationship.

Measured at rest, at half scroll and at full scroll: title 69, species 120.8,
chip 120.8, blob 160.8, chevron 82.9, film −184 — **identical at all three**.

## Also changed

`.pdbg`'s under-film fill. Rev 24 sampled the mp4's bottom row by hand and used
a horizontal `#D4D0C8 → #CDC9BD`. The mockup now states it: `Rectangle 60`
(`441:683`) is a **vertical** `#C8C8BC → #EDEEE9` over 88px, starting at the
film's own bottom edge (y 810; the film ends at 811) and resolving into `--bg`.
Re-expressed against this box: 94.25% → 104.58%. Taking the stated value over a
sampled guess, in the exact area §3 is about.

`.pdscrimb` was removed as well. The brief only named the top gradient, but the
mockup has no bottom scrim either and the pixels prove it: at x=2, y=780 the
mockup reads `(212,208,200)` — **identical** to the film's own bottom-left
colour, where a 20% black wash would have given ~`(170,166,160)`. It existed to
make white cards read against the film; the progressive blur now does that job.

## Verification

Headless Chrome over CDP, real `Input.dispatchMouseEvent`. **No exceptions**
(the one console error is the pre-existing missing favicon).

Rev 24 regression re-run and intact: default −184 / growth hidden; pos 0.50 →
both films at 0 with growth at **t 2.50**; home → −184, growth hidden.

## Shipped

Commit `676f49f` → `origin/main`, Vercel production, aliased to
**https://bloomling-wireframes.vercel.app** (URL unchanged).

Re-verified against the live site: all four items measure identically to local —
scrims 0, header at 43/69 in `#1E0C00`, chip 14-from-right at 72.4×33, the edge
labels stepping `.6/1` correctly at every thumb position, and `blurTop` tracking
`cardsTop − 72` while strength runs 0.00 → 0.50 → 1.00 with the header and film
frozen at 69 and −184 throughout.

One testing note: the first production run reported the growth film at `t 0`
mid-scrub. That was a cold CDN fetch, not a regression — the state machine had
run correctly (both films at 0, growth faded in) but the mp4 was not yet
buffered enough to seek. Waiting for `readyState 4` **and** `buffered.end > 4.9`
before scrubbing gives `t 2.50` at pos 0.50 and `t 4.90` at pos 0.02, exactly as
locally. Worth knowing alongside the rev 24 note about Range requests: with
video, an unbuffered film fails the same way a non-seekable one does.

# Revision 26 — Plant Detail: the reworked rail, the drag indicator, the greeting's one state

Scope is the Plant Detail screen only. Source of truth is the reworked mockup,
section `437:93`, and its three reference states:

| node | what it is |
|---|---|
| `428:1782` | at **now** — thumb at the right end, greeting on screen, cards down |
| `441:633` | **mid-span** — thumb at 333, between stops, no word lit, greeting gone |
| `444:18` | **on a stop** — thumb at 287 with "thriving" bright above it |

## 1 — The rail, rebuilt

Every number is measured off the mockup's own 402px frame, not inferred:

- **words** 16px white; the stop you are standing on at **100%**, every other at
  **60%**. `444:71` "thriving" (no opacity) against `444:21` "now"
  (`opacity-60`), and the same pair the other way round in state 1 —
  `441:576` "now" at full, `441:577` "seed" at 60%.
- **label box** 684 → 704, then a **5px** gap to the rail band's top at 709.
- **rail band** 16px — the thumb's own height. Line centre 717.
- **line** 2px, **pure white** (`444:23` comes back `bg-white`, no opacity), and
  it is **broken**. A pixel probe across `441:633` at y 716 reads
  `white … 2px gap … 16px thumb … 2px gap … white`, and the rect coordinates say
  the same: segments stop **5px** from a dot's centre and **10px** from the
  thumb's, i.e. 2px clear of both edges.
- **dots** 6px white, centres at 17 and 385 → a **17px inset**.
- **thumb** 16px white.

**The track/fill pair is gone.** The old rail was a translucent track with a
bright fill up to the thumb; the new one is a single white line whose *gaps*,
not a tint, mark the stops. `.pdtltrack` / `.pdtlfill` were deleted.

**Segments are drawn as the space between holes.** One hole per surviving dot
(radius 5px) plus a wider one for the thumb (radius 10px), sorted, and every
consecutive pair becomes a segment. Written that way there are no special
cases: the line ends at the outermost stop because that stop is the outermost
hole, it re-splits itself as the thumb travels, and when the thumb lands on a
stop the two holes become one because the dot has already been dropped.
Percent for the stop, pixels for the hole — `calc()` carries both, `max()` keeps
a segment from going negative.

**Decisions taken here**

- **Five milestones, not six.** The rail draws four dots and a thumb, and the
  state that names a middle stop puts "thriving" at the *fourth of five*
  positions (`444:71` centred on 287, stops at 17·107·197·287·385). Dropping
  **"Established"** is what lands the list on the mockup exactly. Safe to do:
  `MILESTONES` is read in exactly one place in the whole file — this rail.
- **`PD_TL_ACTIVE = 0.28` gaps** decides when a word lights up. It has to be a
  judgement because the rail is continuous and the mockup is three stills; a
  quarter-gap is generous enough that the word arrives as you approach, and
  strict enough that the middle of a span shows nothing — which is the mockup's
  own centred state (`441:633`: thumb at 333, not a word lit).
- **`PD_TL_MERGE = 10/368`** decides when the thumb has swallowed a dot. Not a
  taste call: it is the thumb's own reach, and it is why `444:18` has no dot
  under its thumb.
- **A segment under 4px is dropped** (`PD_TL_MINSEG`). As the thumb closes on a
  neighbouring dot the piece between them shrinks to a two-pixel speck that
  reads as dirt rather than as rail. The dot stays; only the stub goes.
- **Every word stays mounted** — the ends resting at .6, the middle ones at 0 —
  so arriving at a stop is a 200ms fade rather than a mount.

## 2 — Drag along the whole length, and the indicator

The press was already taken on `.pdtimeline` rather than on the dots, but the
dots and the words were live targets sitting on top of it and a press that
started on one behaved differently from a press on bare line. **Every child is
now `pointer-events:none`** and the wrapper takes the lot: a press anywhere —
on a dot, on the line, on a word, on the empty band between them — picks the
thumb up and scrubs from there. The fraction still comes from the *rail's* own
rect, so the thumb lands under the finger rather than jumping by the wrapper's
padding.

Release is also backstopped at the window: pointer capture normally guarantees
the matching `pointerup`, but a capture can be lost (a system gesture, a
cancelled touch) and the indicator would be stranded on screen.

**The drag indicator is a call, and it is logged as one.** The brief points at
"the centred state on the reference", but there is no indicator node there —
`441:633` has nothing under its thumb, and a pixel probe of the 34px around the
thumb centre confirms it: background on every side. So the design language had
to supply the answer. It is a **soft white halo that blooms under the thumb on
press and dissolves on release** (34px, radial white .5 → 0, 200ms ease-out,
scaling .5 → 1). Reasons: the rail's own bottom (725) sits **15px** above the
bond card, so nothing with a box fits under it; a glow is already this file's
"active" (the mood chip); and it is drawn *before* the thumb, so the thumb keeps
its hard 16px edge and only the surround softens.

## 3 — The greeting has exactly one state

`greeting = atTop && pos > 0.9994`. The blob is the *live* plant talking, so it
is on screen only with the film at "now" and the sheet still down; scrub away
from now, or pull the cards up over the film, and it withdraws.

`atTop` is published by `syncBlur()` — the scroll measurement it already takes
each frame answers "are the cards still down?" for free, with 8px of slack for
a rubber-band overshoot. React bails out of the re-render on the frames where
the boolean has not flipped.

The transition is **230ms ease-out on opacity and transform**, an 8px rise and a
4% shrink, played in both directions off one class. `transform-origin` is the
**top** centre, so the shrink reads as the blob withdrawing towards the plant
rather than collapsing into itself. Opacity, not `display` — the box keeps its
size, so nothing else in `.pdhead` can move.

## 4 — The blur, higher and harder

Two knobs, both turned:

- **`PD_BLUR_LEAD` 72 → 150.** That is the reach: the ramp now begins 150px
  above the cards' own top edge.
- **The four radii 6/10/18/34 → 8/14/26/48**, and all four layer tops pulled up
  *inside* the lead (0/34/80/150, was 0/30/120/250).

The second half of that matters as much as the first: rev 25 spent two of its
four layers *below* the card edge, where the film is behind opaque white and
nobody can see the blur at all. Measured from the cards' top edge, compounding
as √Σr²:

| | rev 25 | rev 26 |
|---|---|---|
| 150px above the edge | — (ramp had not started) | 0px — the ramp's own start |
| 60px above the edge | ~2px | ~9px |
| at the edge | ~7px | **~21px** |
| deep under the cards | ~40px | **~57px** |

It still starts at zero, so there is still no hard edge. Strength is still a
scroll state — nothing at rest, full at the bottom.

## 5 — Auto-watering, reworked

`445:85`. Four rows in a 392×144 card on `.scard`'s own 24px padding, and every
inner gap is the mockup's: title box 1016→1042, the schedule line at **1043**
(a single pixel under it), the hairline at **1077**, "Customise" at **1092**,
card bottom 1136 — which makes both gaps around the rule exactly **14px**. The
rule is 344 wide, i.e. inset 24 each side, black at **10%**. The schedule is
`rgba(5,5,5,.4)`, the same muted ink as every other secondary label here.
"Customise" is **#ACBA9F** (`445:89`), added to the palette as `--sage-soft` —
lighter than `--sage-deep` and not a colour this file already had.

Two calls:

- **"Customise" is wired, not a stub.** The brief allowed either; `waterConfig`
  already exists and already edits this schedule, so pointing at it is strictly
  better than a dead target. It is also what let the ⋯ menu lose its own
  "Watering schedule" entry without losing the destination.
- **The schedule line is composed, not pasted.** `445:90` reads "Every 2nd day
  at 7am"; that is a *sentence form*, not a string. The cadence comes from the
  plant's own `schedule` and the hour from `PD_WATER_HOUR`, so the card states
  what is actually configured. Vlad therefore reads **"Every 3rd day at 7am"** —
  his real schedule in the mockup's own words. Pasting "2nd" onto a plant
  watered every 3 days would have been a wireframe that lies.
- The whole row is the tap target, not just the 77px word — invisible either
  way, and a 328px-wide target is the kind a thumb actually hits.

## 6 — The cards get out of the way

Starting a scrub with the sheet pulled up is a contradiction: the thing being
scrubbed is the film the cards are covering. So a `pointerdown` on the rail
glides them home.

Hand-tweened, **not** `scrollTo({behavior:"smooth"})` — that is the browser's
curve and the browser's duration, neither of which is this file's. 420ms on
`1-(1-t)⁴` is the shape `--soft` draws, i.e. the motion the sheet already has.
Measured: `scrollTop` 288 → 236 (60ms) → 6 (260ms) → 0 (460ms).

The rail slides down under the finger while this runs. That is correct and
needs no special handling, because the scrub fraction is read from the rail's
*live* rect on every move.

## 7 — The ⋯ popover

It was `top:104px; right:14px` against `.pdhead` — a **screen** coordinate that
merely happened to land near the ⋯ and drifted away from it whenever the header
did. The menu now lives inside `.plmorewrap`, a zero-cost positioning shell
around the icon, so its offsets read as what a popover actually is:
`top:calc(100% + 10px); right:0` — ten pixels under the icon, right edges flush.

**The shell exists for a specific reason:** `.plmore` carries `opacity:.4` on
this screen, and an element with opacity < 1 fades its descendants — nesting the
menu inside the icon would have rendered the menu at 40%. Measured after the
change: gap below the icon **10.0px**, right edges flush to **0.0px**, menu
opacity **1**.

**"Watering schedule" removed**; "Personality & Settings" is the only entry. The
schedule now has a better home — the card that shows it (§5).

## Verification

Headless Chrome over CDP, real `Input.dispatchMouseEvent`, at the live 386px
device width. **No exceptions and no console errors** beyond the pre-existing
missing favicon.

| state | measured |
|---|---|
| rest | 4 segments · 4 dots · "now" lit · greeting opacity 1 · halo 0 |
| press at 62% (mid-span) | 5 segments · 5 dots · **no word lit** · greeting 0 · halo 1 |
| drag to 75% | "thriving" lit · dot swallowed (4 dots) · growth film seeking |
| at seed (x=0) | "seed" lit · thumb at 9 |
| 8px past a dot | dot merged, no stub segment |
| 13px past a dot | dot back, stub still suppressed |
| home | 4 segments · "now" lit · greeting back to 1 · film home |

Geometry against the mockup, scaled 402 → 386: rail 17 → 369 (17px inset both
ends, mockup 17/17); "seed" at x 14 (mockup 14); "now" 14 from the right
(mockup 12); dots 6px at centres 17·105·193·281; thumb 16px; segments 5px clear
of a dot centre and 10px of the thumb's; rail band bottom to bond card **15px**
(mockup 725 → 740); labels 190 up from the bottom edge (unchanged). Auto-watering
card: schedule at y 989 in `rgba(5,5,5,.4)`, rule at 1023 (14px under), action at
1038 (14px under) in `rgb(172,186,159)`.

Collapse-on-drag and the popover anchor as measured in §6 and §7.

## Shipped

Commit `8d2c753` → `origin/main`, Vercel production
`dpl_C5H4poRK8hscARFZ8cMvJvPbhcqb`, aliased to
**https://bloomling-wireframes.vercel.app** (URL unchanged).

Re-verified against the live site — every figure identical to local:

- rest: 4 segments · 4 dots · "now" lit · greeting 1 · halo 0 · thumb at 361
- mid-span: 5 segments · 5 dots · **no word lit** · greeting 0 · halo 1
- expanded: `scrollTop` 288 · `blurTop` 246 (= cards 396 − 150)
- collapse-on-drag: back to `scrollTop` 0 within 500ms of the press
- ⋯ popover: one item, 10.0px under the icon, right edges flush, opacity 1
- auto-watering: "Every 3rd day at 7am", "Customise" at `rgb(172,186,159)`
- rail band bottom 669 to bond card 684 — the mockup's 15px

One CLI note for next time: `vercel --prod` fails with a bare **"Not
authorized"** from this directory unless `--scope wannabe-course` is passed.
The project lives under the team, the CLI session is the personal account, and
the linked `orgId` alone is not enough to resolve the scope.

# Revision 27 — Personality & Settings, rebuilt on its own film

Scope is this one screen. Source of truth is `446:93` and its four states, each
of which turns out to date itself by where its content sits against the resting
layout — which is what let the scroll behaviour be *derived* rather than guessed:

| node | content top | ⇒ scroll | what it shows |
|---|---|---|---|
| `440:330` | 596 | **0** | the film to the top of the screen, its gradient at 662→750, "Personality" already on the film |
| `438:96` | 442 | **200** (see below) | film smaller and lower, the reaction bubble at 163 |
| `441:425` | 177 | **419** | film gone, `441:566`'s scrim holding the title off the settings |
| `441:497` | 213 (at "General") | **806** | settings alone under the pinned title |

## The layout is the mockup, to the pixel

Measured on the running screen against the frame's own numbers — the mockup is
402×818 and this device is 386×818, so every vertical number carries over 1:1
and only widths re-scale:

| | mockup | built |
|---|---|---|
| stage / film bottom | 662 | 662 |
| gradient | 662 → 750 | 662 → 750 |
| "Personality" | 596 | 596 |
| chip row | 662 | 662 |
| slider card | 750, h 229 | 750, h 229 |
| slider thumb rows | 799 · 866 · 933 | 799 · 866 · 933 |
| "General" | 1019 | 1019 |
| name / species | 1085 · 1164 | 1085 · 1164 |
| voice chips | 1243 | 1243 |
| "Activity" | 1383 | 1383 |
| messages row | 1449 | 1449 |
| quiet-hours card | 1528, h 146 | 1528, h 146 |
| its rule / action | 1614 / 1630 | 1614 / 1630 |
| **total scroll height** | **1746** | **1746** |

Chip widths land on the mockup's too — Friendly 123, Grump 117, Calm 99,
Cheerful 128 — which is the useful confirmation that the 24px padding and the
22px Riccione are right rather than merely close.

Values, all from the file: chips and voice pills radius 40 on `#C9D6BC` when
chosen and white when not (`440:356`, `440:373`); slider track `#EDEEE9`, fill
and 22px thumb `#C9D6BC` (`440:338/341/344`); slider labels 16px `#050505` with
the value at 40% (`440:347`); section headers 44.712px `#1E0C00`; the
quiet-hours rule black at 10% and "Customise" `#ACBA9F`, i.e. the Auto-watering
card's own pattern, with the mockup's 15px either side of the rule rather than
that card's 14.

## The film shrinks — that is the behaviour, not decoration

The two states that show the film do not show it at the same size, and reading
that difference as a rule is what made the scroll work:

- `440:330` draws it **744×856 at (−171,−194)** with 662 of stage visible.
- `438:96` draws it **402×462 at (0,0)** with 462 of stage visible.

Cover width for a visible stage of V is `V × 0.86788` (the film is 1340×1544).
The rest state runs `744 / (662 × 0.86788)` = **1.295×** cover; the scrolled one
runs `402 / (462 × 0.86788)` = **1.002×**, i.e. exactly cover. So: **the film
covers whatever is left of the stage, times a zoom that eases 1.295 → 1 over the
first 200px**, bottom-anchored throughout. Built, it measures

- scroll 0 → 744×857 at y −195 (mockup 744×856 at −194)
- scroll 200 → 401×462 at y 0 (mockup 402×462 at 0)

Past 445px of visible stage cover would take the film *narrower than the
screen*, so the width floors there and the film crops from the top the rest of
the way — which is what the two deep states show, a film already gone.

This also settles a legibility problem the naive reading created. Holding the
rest framing and merely scrolling it drags the canopy straight up through the
pinned serif title, which is unreadable; the zoom pulls the whole plant down and
out of the way before that can happen. The designer's two crops were the answer.

**The film is bottom-anchored, never top-anchored.** Its bottom edge is the seam
with the gradient and that seam must not move; the 386-vs-402 width difference
is spent on the top crop instead, where nothing meets anything. Verified: stage
bottom and gradient top are the same number at every scroll position tested.

**One inconsistency, flagged.** `438:96`'s film says scroll 200 while its own
content top (442 against a resting 596) says 154. Hand-placed frames disagree by
46px. The film's reading is taken, because it is the one that reproduces two
independent numbers (size *and* position) exactly.

## The scrim is a scroll state

`441:566` is `#EDEEE9 → transparent` over 120px at y 176 — solid page colour
behind the title and a 120px dissolve under it, 296 in all. It is **absent** at
rest (`440:330` runs the film to the top of the screen behind the title), so its
opacity is a scroll state, and both ends of the ramp are read rather than
chosen: `438:96` at 200 has no scrim and nothing under the title; `441:425` at
419 has it solid and the film gone.

**200 → 419**, and it lands where it must: the settings' own top edge (596)
reaches the title's bottom (176) at scroll **420**. The scrim finishes arriving
on the frame the content needs it.

## The reaction bubble

`438:175/176` — a 193×89 pebble at 11px from the right edge, top 163, 12px
centred text. Same blob path as the detail screen's, drawn smaller, so it is
recognisably the same object; same 230ms fade-and-settle off one class, so the
two screens read as one behaviour.

Decisions:

- **The line is generated, not static.** The brief allowed a sample string, but
  `sampleLine(preset, tune)` already exists and builds an in-character sentence
  *from these very slider values* — so the bubble genuinely answers what was
  just changed rather than miming it. Picking "Drama queen" gets "Call the
  papers — this is my FINAL act!"; moving Warmth to the top appends its warm tag.
- **12px is deliberate and off the locked type scale.** The scale's smallest
  step is 13 and this sentence does not fit an 89px blob at 13. The mockup says
  12; taken.
- **It shows only while shaping the character** — a preset tap or a slider move
  raises it, 2.6s of quiet lowers it (the brief's 2–3s band), and scrolling or
  touching anything in General/Activity lowers it at once.
- **And only while there is a plant under it.** It is the plant answering, so it
  is gated on scroll < 410 — the point at which the film has left the bubble's
  own box (163 + 89). Past that the bubble would be a voice from nowhere.

## Calls worth naming

- **Six presets, not eight.** The brief says "the canonical 8"; the canonical set
  in this file is six and `440:356` draws exactly those six. Followed the file
  and the mockup, which agree with each other.
- **"Drama queen", not "Drama Queen".** The mockup capitalises it; the data does
  not, and `PLANTS` stores `persona:"Drama queen"` while onboarding renders the
  same list. Changing the casing would have reached two other screens.
- **Male / Female / Prefer not to choose is a NEW list** (`PS_VOICES`), local to
  this screen. `VOICES` is timbre — Warm, Bright, Soft, Playful — and onboarding
  still asks that question; they are different questions, so the shared constant
  was left alone rather than repurposed.
- **The pencils work.** Tapping a name or species row swaps the serif line for an
  input in the same box; blur or Enter commits. Drawing an edit affordance that
  does nothing when pressed is the one thing worth avoiding on a settings screen.
- **"Customise" on quiet hours opens the window picker inline** rather than being
  a dead target or a new screen. The card the mockup draws is the resting state;
  the three windows the previous screen carried fold in underneath, so the
  feature survives the rebuild without appearing where the mockup shows nothing.
- **"Remove plant" is gone, and nothing else offers it.** `446:93` has no
  destructive action anywhere and the brief lists the screen's contents top to
  bottom without one, so it went with the old composition — along with its
  confirm dialog. Flagging it because it means the prototype now has no path to
  removing a plant at all; putting an unmocked destructive button back would
  have been the larger liberty, but it is a product call, not a layout one.
- **`PersonalityPicker` was deleted.** It stacked a preview blob, the preset grid
  and the fine-tuning card for this screen and nothing else ever called it.
  `PresetGrid` / `FineTuning` / `LinePreview` stay — onboarding builds on them.
- **The film is Vlad's for every plant**, exactly as the detail screen's already
  are: there is one plant with footage and `PS_FILM` is the seam where per-plant
  films would land.

## Verification

Headless Chrome over CDP at the live 386px device width. **No console errors and
no exceptions** — this run the favicon 404 did not even appear.

- Every layout number in the table above, measured on the running screen.
- Film: 744×857 at −195 (scroll 0) → 401×462 at 0 (scroll 200) → floored at 386
  wide and cropping from the top (300+). Stage bottom == gradient top at 0, 100,
  200, 300, 419, 560. Restored exactly on the way back up.
- Scrim: 0 at 0 and 200, 0.46 at 300, 1 at 419 and beyond.
- Bubble: hidden at rest; opacity 1 on a preset tap with the line changing to
  that character; opacity 1 on a real mouse drag of a slider; 0 after 2.6s; 0
  after any scroll.
- Name row opens an input carrying the current name; "Customise" reveals the
  three windows with the current one lit.
- All six flow-index entries still render, and Plant Detail → ⋯ → this screen →
  back is intact in both directions.

## Shipped

Commit `49e40f1` → `origin/main`, Vercel production
`bloomling-wireframes-h1blssl19`, aliased to
**https://bloomling-wireframes.vercel.app** (URL unchanged).
`settings-vlad.mp4` (3.9 MB) is committed alongside the other two films.

Re-verified against the live site — identical to local at every point:

| scroll | film | stage bottom / gradient top | scrim |
|---|---|---|---|
| 0 | 744×857 at −195 | 662 / 662 | 0 |
| 200 | 401×462 at 0 | 462 / 462 | 0 |
| 419 | 386×445 at −202 | 243 / 243 | 1 |
| 806 | 386×445 at −589 | −144 / −144 | 1 |
| back to 0 | 744×857 at −195 | 662 / 662 | 0 |

Scroll height **1746**, the mockup frame's own. Preset tap raises the bubble and
2.7s of quiet lowers it. No console errors beyond the pre-existing missing
favicon.

One note: the film reports `paused` once it is entirely off-screen (scroll 806)
and plays again on the way back up. That is Chrome suspending an invisible
video, not a state bug — `currentTime` resumes from where it stopped.

# Revision 28 — the rail drives the film, the blob bounces, and Auto-watering gets its screen

## 1 — "Dragging moves the thumb but not the video"

It did move the video. It moved it about **a second late**, which on a
drag-and-release is indistinguishable from not moving it at all — and that is
the whole of the bug.

Two lags stacked. The hand-over costs `PD_SLIDE + PD_FADE` = 600ms before the
growth film is even on screen, which is by design and stays. On top of that,
rev 24's `shown += (want − shown) × 0.22` needed **~26 frames**, another ~440ms,
to close a long drag — so the film arrived at a position the finger had left.
Measured before the fix, mid-drag: thumb at 0.65 while the film sat at t 1.73
against the 1.75 it owed, then still catching up 500ms after the last move.

**The film now rides the finger.** `chase` is a per-scrub rate, and which rate
is in play is decided by how far the request moved rather than by which input
sent it:

- **1** — write `currentTime` straight from the thumb, frame for frame. This is
  a continuous scrub: a drag, a trackpad gesture, anything tracking.
- **PD_EASE (0.22)** — glide. This is a *jump*: a press landing far from the
  thumb, where a cut would be ugly and nobody is following a finger.

`PD_SCRUB_JUMP = 0.08` of the rail (~28px) is the line — comfortably more than
any single pointermove or wheel notch, comfortably less than a deliberate tap
somewhere else. Measured after: thumb 0.75 / 0.60 / 0.45 / 0.30 against film
t 1.24 / 1.98 / 2.73 / 3.48, i.e. exact to the 0.02 the deliberate tail-trim
(`dur = duration − 0.04`) accounts for.

**One handler, and it is now named as one.** `seek()` became `scrub()`, the
single entry every input reaches: pointer down, pointer move, and wheel all call
it and nothing else, so there is exactly one description of what a position
means — it is the thumb, it is `pos`, and it is the film's `currentTime`, always
the same three at once.

**The "now" edge still holds.** Scrubbing live does not tear the cross-fade,
because `shown` was already frozen while the growth film is off screen
(`tick()` returns early unless phase is "growth"). So the still film sits at
"now" through the whole 420ms glide however far the finger has got, and the
growth film takes up the finger's position on the first frame it exists — at
opacity 0, with the 180ms fade turning what would have been a cut into a
dissolve from "now" to wherever the drag reached. Verified home: film back to
t 0, growth faded out, still film back at −184.

## 2 — The blob arrives with a bounce

**A keyframe, not a spring curve on the transition**, and the reason is
arithmetic: the blob travels from `scale(.96)` to `scale(1)`. A back-out cubic
peaking ~10% past its target would overshoot that by **0.4%** — the spring is
real and completely invisible. `@keyframes pdbubpop` states the overshoot
instead: 300ms, `.94 → 1.035 at 55% → 1`. Caught mid-flight in the test at
**scale 1.0345**.

Exit is untouched, as asked. It falls out of how CSS picks a transition: a
transition is the one on the state being *entered*, so adding `.off` runs
`.off`'s own plain 230ms ease-out, and removing it runs the animation. Verified:
`animation-name` reads `none` in the hidden state and `pdbubpop` on the way back.

**`.psbub` on Personality & Settings was left alone** — it is the same object
and arguably wants the same bounce, but this brief scopes to Plant Detail and
the Auto-watering screen, and that is a different screen.

## 3 — Auto-watering collapses when it is switched off

With the toggle off there is no schedule to state and nothing to customise, so
the schedule line, the rule and "Customise" go together and the card becomes its
title row: **146px → 76px** over 200ms, measured.

76 rather than the 74 every other single-line card on the screen is: `.scard`'s
two 24s round a row whose height is the **28px toggle's**, not the 26px of serif
beside it.

`max-height`, not `grid-template-rows: 0fr` or `height: auto` — the block's
height is known and fixed (1 + 20 + 14 + 1 + 14 + 20 = 70), so a hard 80px cap
is honest here and works everywhere. `overflow:hidden` also gives the block its
own formatting context, which is what stops `.pdautosub`'s 1px top margin
collapsing out through the top of it.

## 4 — The Auto-watering settings screen (447:131)

Rebuilt to the mockup. Measured against the frame's own numbers (402×874 against
this 386×818 device — the title and groups are top-anchored so their y carries
over, Save is bottom-anchored at 850 of 874 = 24 from the foot):

| | mockup | built |
|---|---|---|
| title (one line, serif) | 69 | 69 |
| chevron | 85 | 82.9 |
| "Watering every" | 161 | 160.9 |
| its option row | 195, h 95 | 194.9, h 95 |
| "Amount per watering" | 330 | 329.9 |
| its option row | 364, h 95 | 363.9, h 95 |
| Save | 24 up from the foot, h 50, r 28 | 744, h 50, r 28 |

Values from the file: cards radius 40 on 24px of padding, white / `#C9D6BC`,
serif 22px over a 16px `rgba(5,5,5,.4)` second line on a 1px gap; label rows
16px `#050505` inset 9 (which lands on the device's 14); Save `#C9D6BC`, radius
28, 15px padding, and its label is `--ink` rather than the pill buttons'
`--serif-ink`. **No film**: `447:197` reads `#EDEEE9` at every corner and margin,
so the video that frames both neighbouring screens is deliberately absent.

Calls:

- **"best choice" marks the middle option permanently, not the selected one.**
  The mockup only ever shows it on a sage card, which reads two ways; the other
  reading makes "High" become the best choice the moment you pick it, which is
  not what a recommendation is.
- **`minmax(0,1fr)`, not `1fr`.** A bare `1fr` floors each track at its content
  and "best choice" is wider than "1 day", so the three cards came out
  118/131/118 against the mockup's three equal 127.
- **Vlad's cadence moved from "Every 3 days" to "Every 2 days".** The two
  mockups agree with each other and his row did not: `445:90` sets the Plant
  Detail card to "Every 2nd day at 7am" and `447:131` opens with "2 days"
  selected. He is the plant every one of these frames is drawn from. The card
  now reads exactly what `445:90` says.
- **Plants whose cadence the mockup does not offer land on the nearest** (Felix
  every 3 → 2 days, Gosha every 10 → 5 days) rather than opening with nothing
  selected.
- **Save writes back.** `PLANTS` is this prototype's only store and Plant Detail
  re-reads it on mount, so the pick survives the trip: choosing "5 days" and
  saving returns to a card reading **"Every 5th day at 7am"**. A Save that did
  nothing would teach the wrong thing about the screen.
- Navigation is unchanged — in from "Customise", back to Plant Detail, and Save
  is also a back.

## Verification

Headless Chrome over CDP, real `Input.dispatchMouseEvent`, at the live 386px
device width. **No console errors and no exceptions.**

- Live scrub: thumb 0.75/0.60/0.45/0.30 → film t 1.24/1.98/2.73/3.48. Home
  restores t 0, growth hidden, still film at −184.
- Blob: `pdbubpop`/0.3s at rest, `animation:none` + `scale(.96)` hidden,
  `scale(1.0345)` 120ms into the return, `scale(1)` settled.
- Card: inner block 70 → 0 and card 146 → 76 on toggle off, and back on.
- The new screen's whole layout table above; picking 5 days + saving lands on
  Plant Detail with the card restated.
- All six flow-index entries still render; Personality & Settings untouched and
  intact (film 744 wide at rest).

## Shipped

Commit `4ae906c` → `origin/main`, Vercel production
`dpl_CkTY1SmDntDYtCTJWXKsv24y7Bpi`, aliased to
**https://bloomling-wireframes.vercel.app** (URL unchanged).

Re-verified against the live site, **no console errors at all**:

- **Live scrub** — thumb 0.75 / 0.50 / 0.25 against film t **1.24 / 2.49 /
  3.74**, i.e. dead on the 1.25 / 2.50 / 3.75 owed, less the deliberate 0.04
  tail trim. Home returns t 0 with the still film back at −184.
- **Blob** — `pdbubpop`, 0.3s.
- **Card** — inner block 70 → 0, card 146 → 76 on toggle off, and back.
- **Auto-watering screen** — title "Auto-watering", labels at **161 / 330**,
  three option cards **122px each** (equal), "2 days · best choice" and "Low"
  lit from Vlad's own data, Save at **5 / 744 / 376 × 50**.
- **Round trip** — picking "5 days" and saving lands on Plant Detail with the
  card restated as **"Every 5th day at 7am"**.

---

# Revision 29 — Mary replaces Margot, and every plant gets its own films

## 1 — Margot the Monstera → Mary the Cannabis

**The id moved too: `margo` → `mary`.** It was tempting to leave it — an id is
invisible — but the film files that arrived this pass are named
`still-mary.mp4` / `growth-mary.mp4` / `settings-mary.mp4`, i.e. by plant id.
Keeping `margo` would have meant a second, hand-maintained id→file table beside
the one the ids already are. Renaming it makes `filmSrc()` a string join and
nothing else. The rename carries `--c-margo`/`--g-margo`, `BUBBLE`, `AVATAR`,
`SHOT`, the notification's `plant` key and the four chat `who`s with it.

**"Mary the Cannabis", not "Mary".** Every screen already renders
`name.split(" ")[0]`, so the long form is only ever read by the notifications
list, where the two other plants read "Gosha the Cactus" and "Vera the Aloe".
"Mary" alone would have made her the one entry in that list without a species.
The My Plants card and the detail header still say **Mary**, as `428:1745` does.

**The character is untouched.** Persona stays `Drama queen`, and her check-in,
her reaction, her diary note and both chat lines are word-for-word what Margot
said — none of them named her or her species, so none of them needed to change.
The species picker's `"Monstera"` became `"Cannabis"` in place.

**Her colours were already right.** Sampling `428:1745`'s Mary card gives
`#B9CBD8` at the top-left, which is exactly the first stop of the gradient that
was already `--g-margo`. So `--c-` and `--g-` were renamed and not re-valued —
the mockup confirms the palette rather than replacing it.

## 2 — The two Mary assets, and how they were measured

**Pot render** — `458:18` inside `428:1745`. Frame **262×215** at (135, 275)
against a card at 275 whose right edge is 397, so `win` is `right 0, top 0`, and
the fill runs `w-[136.64%] / h-[166.51%] / top-[-66.51%]` → a **358** square at
`0, −143`. Margot's was 265×172 with a 363 square at `0, −144`; the new frame is
43px taller and the crop 5px shallower, which is the whole difference.

**Chat avatar** — composed at 4× from `458:46`, not exported. Figma renders that
frame against the section's dark board, so an export comes back with a grey
backdrop baked in; the frame's own parts are cleaner. The recipe: white disc,
then the source square scaled into its 90px box at (−13, −5), shown where the
circle mask **or** the top band reaches. The frame is **63×70** and the circle
sits at (13, 32), so `AVATAR.mary` is `w 63, h 70, l −13, t −32` — the same
"negate the circle's position in the frame" rule the other three entries follow.

**The recipe was checked before it was trusted.** Running it on Felix's own node
(`364:1057`, frame 38×47, circle at (0, 9)) rebuilds the committed
`avatar-felix.png` at exactly 152×188 and visually identically — mean absolute
difference 9.9/255 per channel, all of it resampling. So Mary's avatar is
produced the same way the existing set was.

`pot-margot.webp` and `avatar-margot.png` are deleted; nothing references them.

## 3 — Every plant now plays its own films

`assets/video/` holds **all fifteen files** — `still`/`growth`/`settings` for
felix, mary, gosha, vera and vlad. **Nothing is missing, so the fallback never
fires in this build**; `FILM_FALLBACK = "vlad"` and `FILM_HAVE` stay in anyway,
because the honest stand-in for a plant with no footage is the plant every one
of these frames was drawn from, and a missing file should degrade rather than
render a black rectangle.

`FILM`/`PS_FILM` — two frozen strings — became `filmFor(id)` and `psFilm(id)`
over one `filmSrc(id, kind)` join. Plant Detail reads `filmFor(p.id)`,
Personality & Settings reads `psFilm(p.id)`, and both `<video>`s carry a `key`
on their src so a plant change swaps the element instead of mutating a playing
one.

**The scrub geometry needed nothing.** All ten still/growth files are
**976×2124, 5.042s** — Vlad's exact shape and length — so `PD_Y`, the
`t = (1 − pos) × duration` mapping and the 976/2124 aspect all carry over
untouched.

## 4 — The one real fork: the settings films are not all the same shape

Vlad's `settings-vlad.mp4` is **1340×1544** (0.86788). The other four are
**1292×1604** (0.80549) — taller and narrower.

`PS_AR` is not a constant of the screen, it is the input to the cover rule
(`width = visible-stage × AR × zoom`), and `.psvid` pins the same ratio in CSS.
Left as Vlad's it would have **stretched four films by 7.7%** to fit a box cut
for a fifth — which is the one thing "identical mechanics per plant" cannot
mean. So `PS_AR` became `PS_AR_BY_ID` + `psAr(id)`, written inline on the
element and read by `sync()`; the CSS value stays as the default. **The
arithmetic is unchanged — only its one input moves.**

What that buys, measured at rest: the stage is 336×662 and the film is 857 tall
and bottom-anchored on **all five**. Only the width differs — 744 for Vlad, 691
for the other four — which is exactly cover for each film's own ratio. The seam
with `.psfade` does not move for anyone.

## 5 — Left alone, deliberately

- **`428:1745` labels Vlad "Aloe"; the app says "Bonsai".** His card render and
  all three of his films are a bonsai, and the brief for this pass is Margot →
  Mary. Noted, not touched.
- **The updated avatar set (`364:1042`) also contains Vlad.** The chat casts
  Felix, Mary, Gosha and the user and no one else, so wiring a fifth avatar
  would add an unreachable entry. Only Mary's was taken.

## Verification

Headless Chrome over CDP, real `Input.dispatchMouseEvent`, DPR 2. **No console
errors and no exceptions on any screen, and no broken images anywhere.**

- **My Plants** — five cards in the mockup's order Felix / Mary / Gosha / Vera /
  Vlad. Mary reads chip **thirsty**, species **Cannabis**, name **Mary**,
  surface `linear-gradient(126.35deg, rgb(185,203,216) 38.007%, rgb(227,155,132)
  115.19%)`, render `pot-mary.webp` in a **262×215** window at `right 0 / top 0`
  with a **358×358** fill at `0 / −143` — the measured Figma values, live.
- **Chat + notifications** — both of Mary's bubbles and her avatar render; the
  digest reads "Mary the Cannabis" and "Mary's reservoir is empty — she'd love a
  refill", and tapping it still lands in the chat.
- **Plant Detail, all five** — each loads its own pair. `still-<id>.mp4` and
  `growth-<id>.mp4` for felix / mary / gosha / vera / vlad, every one reporting
  **976×2124, 5.042s**, still film parked at **−184**.
- **Live scrub, all five** — thumb 0.75 / 0.50 / 0.25 → growth film t **1.25 /
  2.50 / 3.75** on every plant, growth layer at opacity 1. Home restores t 0,
  growth hidden, still film back at −184. Identical numbers for all five.
- **Personality & Settings, all five** — `settings-<id>.mp4`, intrinsic
  1292×1604 for the four and 1340×1544 for Vlad, each drawn at its own ratio:
  **691×857** and **744×857**, both bottom-anchored in the same 336×662 stage.
  744 is the number rev 27 logged for this screen, unchanged.

## Shipped

Commit `c99d69a` → `origin/main`, Vercel production
`dpl_2LrxX9K93NkGhedXAJC9Cbwpk2UN`, aliased to
**https://bloomling-wireframes.vercel.app** (URL unchanged).

Re-verified against the live site. The only console entry on the whole run is
the browser's own automatic `GET /favicon.ico` 404 — this prototype has never
declared one, and rev 28's build 404s on it identically. **No 404 on any
resource the page actually asks for**, and no exceptions.

- **Assets** — `pot-mary.webp` and `avatar-mary.png` serve 200; all fifteen
  films serve 206; `pot-margot.webp` is **404**, i.e. genuinely gone.
- **Per-plant films** — Felix / Mary / Gosha / Vera / Vlad each load
  `still-<id>` + `growth-<id>` at **976×2124, 5.042s** with the still film at
  **−184**, and `settings-<id>` at **691×857** (1292×1604 source) or, for Vlad,
  **744×857** (1340×1544).
- **Live scrub, all five** — thumb 0.75 / 0.50 / 0.25 → film t **1.25 / 2.50 /
  3.75** on every plant, against a `growth-<that plant>.mp4`. Home returns t 0,
  growth faded out, still film back at −184. Five identical tables.

---

# Revision 30 — the resolve moves on top of the film, Profile is rebuilt, and the films get half a second lighter

## 1 — The video→page gradient, on BOTH screens

It was a band **under** the film — `#C8C8BC` at the film's own bottom row
resolving to `--bg` over 88px, sitting below it on Personality & Settings and
painted as `.pdbg`'s background on Plant Detail. `458:65` and `441:421` move it
**on top of the film**, and both carry the same fill:

```
linear-gradient(180deg, rgba(237,238,233,0) 16.837%, #EDEEE9 50%)
```

Transparent page to 16.837%, full page by 50%, solid the rest. It reads as the
page arriving over the film rather than as a strip under it — and it retires the
one seam this prototype could never make clean, because `#C8C8BC` was Vlad's
film's bottom row and the other four plants' bottom rows are not that colour.

**Plant Detail — the anchor is the film's own bottom edge, and all three states
say so.** `428:1782` has the film at −184 (bottom 811) with the rect at 723…811;
`441:633` and `444:18` have it at 0 (bottom 995) with the rect at 907…995. So
`.pdresolve` is given the **film's own box** — same `left`, same `width`, same
`aspect-ratio` — plus the same Y transform on the same 420ms curve as
`.pdvid-still`. Its bottom edge is then the film's bottom edge by construction
rather than by arithmetic, and the two glide together with no second number to
keep in sync. `.pdbg` loses its gradient and goes flat `--bg`.

**Personality & Settings — a −70px margin, and the content does not move.**
`440:330` — the one state whose film is drawn at the size the cover rule
actually produces — puts the rect at 592…680 against a film bottom of 662: 70
up into the film, 18 past it. `.psfade` gets `margin:-70px -5px 0`, so it tracks
the stage's bottom edge through every scroll position for free. The content is
then re-lapped from −154 to **−84** (`PS_LAP`), because the stage plus the
resolve's overhang now runs to 680 rather than 750 — "Personality" still opens
at 596, exactly where `440:330` opens it.

- **`438:96` draws the same 88px band at −60/+28 of its own film bottom, not
  −70/+18.** The rest state is the one measured from: it is the state the screen
  opens in and the only one whose film is drawn to the rule. 10px of
  hand-placement is not a second behaviour.
- **No `z-index` on any of the three.** `.psstage`, `.psfade` and `.pscontent`
  are all positioned, so they paint in DOM order, which is the order the mockup
  stacks them — film, resolve, settings on top of both. The first attempt gave
  `.psfade` a `z-index:1` and it half-dissolved the "Personality" header;
  `440:330` renders that header fully legible ON the film.

## 2 — The preset chips scroll the chosen one into view

The row is 774px against a 386px screen, so tapping a chip that is half off the
edge selected something you could not read. Tapping now scrolls it fully in,
smoothly, with **24px of clearance** — the chips' own side padding, so a
revealed chip sits exactly one pad from the edge rather than flush against it.
Both edges, from the same two lines: if the chip's right runs past the viewport,
scroll to put it there; if its left runs before, scroll to that.

**Measured with `getBoundingClientRect` against the row, not `offsetLeft`.**
`.pschips` is not positioned, so `offsetParent` is the content column and
`offsetLeft` would be measuring the wrong box entirely.

## 3 — The bubble stays, and one gate stays with it

The 2.6s preview timer is gone: the bubble is simply the plant now, on screen
the whole time the plant is. `sampleLine()` still builds the line from the live
slider values, so it *answers* each change instead of *appearing* for each one.
`poke`/`hush` and the two `onPointerDown` handlers that fed them went with it.

**The depth gate survives, and that is a call.** The brief asks for the bubble to
be always visible **over the video** and names exactly one thing to remove —
"the personality-editing-only show/hide logic". `PS_TALK_MAX` is not that: it
hides the bubble once the film has scrolled out from under it. The mockups back
it — `438:96` draws the bubble on the film, while `441:425` and `441:497`, the
two states scrolled past the film, do not draw it at all. Pinned at `top:115`
with nothing behind it, it lands on the "General" heading, which is the one
outcome neither reading of the brief wants. Verified: opacity 1 at rest, 0 at
scroll 600, 1 again on return.

## 4 — The Activity toggles

**The stretched switch was a selector, not a size.** `.psqtop > div{flex:1 1
auto}` was written for the card's text column — but the switch is a bare `<div>`
too, and `.psqtop > div` (0,1,1) outranks `.toggle` (0,1,0), so it handed the
switch `flex:1 1 auto` and the switch grew to fill the row. Fixed at both ends:
the text column got a class of its own (`.psqtxt`) so the child selector stops
matching the switch, and `.toggle` states `flex:0 0 50px` instead of `0 0 auto`,
so its width is its own property rather than something a parent gets a vote on.
Measured after: both switches **50×28** with **22×22** knobs, identical.

**Quiet hours collapses like Auto-watering, because it is the same card.** With
the switch off there is no window to state and nothing to customise, so the
subtitle, the hairline and "Customise" all withdraw and the card falls to title
+ switch. `.pdautomore`'s exact pattern and deliberately its exact numbers —
`max-height` over a known height, 200ms, `overflow:hidden` for the BFC. Two
blocks rather than one, because the subtitle lives inside the title column (so
the switch stays centred on what is left) while the rule and the action sit
under the whole row. Switching off also folds "Customise" away, which is what
keeps the tail's height the fixed 51px its `max-height` is cut for.

- **The subtitle gets room for two lines (60px, not 40).** On a 386px device it
  takes them: 254px of body copy into a 228px column, where the 402px frame it
  was drawn on gives 294. Body copy wrapping is fine.
- **`.psrowcard` gave up its 12px gap.** The serif TITLE wrapping is *not* fine,
  and "Plants talk to each other" lost its single line to that gap on the new
  Profile screen. Both mockups run the title box from 24 to the switch's own
  left edge at 318 with nothing between them, so 0 is also the drawn value.

## 5 — Profile, rebuilt on 460:144

Measured after, every number against the mockup's own: identity card **161**,
"Chat settings" **296**, switch card **362** (74 tall), "Message frequency"
**450**, the three options **484** (95 tall), language cards **685** and **794**
(95 each), Log out **929** (50). The rhythm is one pair of numbers — **40px**
between sections, **14px** inside one — and it holds everywhere.

**Three of the four blocks are cards this app already owns**, so the screen is
assembled rather than drawn: `.psrowcard` for the switch row, `.psqt`/`.psqsub`
for identity and the two language rows, and — the one worth naming — the
**auto-watering option card** for message frequency. `460:167` is the same 95px
pill on the same 5px gap with the same `--sage` fill, the same 22/16 pair inside
it and the same "best choice" on the middle one. It is the same control; it is
now literally the same code.

Calls:

- **`460:145` carries a still of the pot and is not built.** The frame renders
  flat `#EDEEE9` over every inch of it — a hidden layer, not a background.
- **The row descriptions are gone** ("They comment on each other, not just you",
  "Buttons, menus and screens"), and so is the `prototype · v0.3` line. The
  mockup states the current VALUE under each title instead — "English", "5
  plants" — which is the more useful thing for a settings row to say, and it
  ends at Log out.
- **Log out is `--card` at radius 28 in `#E39B84`**, the coral the palette
  already carries as Mary's second gradient stop. Not a `--sage` Save: it is the
  only destructive thing on the screen.
- **Navigation is unchanged.** In from the dashboard avatar, out via the
  chevron; "5 plants" still opens the plants list; both language rows still open
  the picker and still write back.

## 6 — The films

`ffmpeg` (9.0.1, via Homebrew) → H.264, `-preset slow`, `-pix_fmt yuv420p`,
`-an`, `-movflags +faststart`. **73MB → 9.7MB, 13.3% of what was there.**

| file | before | after | |
|---|---:|---:|---:|
| growth-felix.mp4   | 3 131 957 | 1 030 314 | 32.9% |
| growth-gosha.mp4   | 9 178 205 | 1 236 498 | 13.5% |
| growth-mary.mp4    | 10 792 430 | 1 360 530 | 12.6% |
| growth-vera.mp4    | 2 907 278 | 1 089 665 | 37.5% |
| growth-vlad.mp4    | 4 202 689 | 1 177 069 | 28.0% |
| settings-felix.mp4 | 4 860 948 | 406 709 | 8.4% |
| settings-gosha.mp4 | 3 385 027 | 246 748 | 7.3% |
| settings-mary.mp4  | 5 082 138 | 595 675 | 11.7% |
| settings-vera.mp4  | 4 112 656 | 262 508 | 6.4% |
| settings-vlad.mp4  | 4 103 578 | 340 127 | 8.3% |
| still-felix.mp4    | 3 918 223 | 298 547 | 7.6% |
| still-gosha.mp4    | 4 192 598 | 339 012 | 8.1% |
| still-mary.mp4     | 6 474 550 | 876 276 | 13.5% |
| still-vera.mp4     | 4 448 436 | 363 317 | 8.2% |
| still-vlad.mp4     | 5 237 172 | 527 081 | 10.1% |

**The scrub fix is the GOP, and it was the whole bug.** Every original growth
file carried **one keyframe for its entire 5.04 seconds** — so seeking to t=4.5
meant decoding 108 frames forward from frame 0, every time. `-g 6 -keyint_min 6
-sc_threshold 0` at 24fps puts a keyframe every **0.25s**: 21 of them per file.

Measured with `requestVideoFrameCallback` — time from writing `currentTime` to
the browser actually presenting a frame, ten seeks scattered across the film:

|  | median | mean | worst |
|---|---:|---:|---:|
| original, 1 keyframe | 116ms | 113ms | **217ms** |
| optimized, 0.25s GOP | 20ms | 19ms | 25ms |

The *shape* matters as much as the number. The original's cost tracks distance
from t=0 — 217ms at t=4.5 against 29ms at t=0.6 — which is exactly what "decode
from the one keyframe" looks like, and exactly what made scrubbing feel like it
was dragging. The optimized file is **flat**: 8.8ms at t=4.5, 24.5ms at t=1.2.
Still and settings films loop rather than seek, so they keep a normal GOP and
take the size win instead.

Calls:

- **Resolution is unchanged — 976×2124 and 1292×1604/1340×1544 — and the brief's
  own escape hatch is why.** It asks for "roughly 720px height (or the display
  size actually needed)", and the display size actually needed is larger than
  the source already: on a 386px device the detail film draws 439×955 CSS, i.e.
  **878×1910** at DPR 2, against a 976×2124 source. The settings film draws
  691×857 CSS → **1488×1714**, against a 1292×1604 source that is *already
  below* it. Downscaling to 720 would have put every film under one device
  pixel per source pixel at DPR 1. The savings came from CRF instead, and they
  were there to take: the originals ran **8.3 Mbps**.
- **CRF 26 for the loops, 24 for the growth films.** The growth films pay for
  their dense keyframes in bits, so they get the quality budget back. SSIM
  against the originals: **0.9969** (growth-vlad), **0.9933** (still-mary),
  **0.9946** (settings-mary).
- **`-an` is stated though the sources carry no audio track.** It costs nothing
  and it means the command says what the output is rather than what the input
  happened to be.
- **`preload="auto"` on the growth video was already there** (rev 24) and stays,
  now with a file a fifth of the size behind it.

## Verification

Headless Chrome over CDP, real `Input.dispatchMouseEvent`, DPR 2. **No console
errors and no exceptions on any screen; no broken images anywhere.**

- **Plant Detail resolve** — `.pdresolve` box equals `.pdvid-still` box in both
  states: `(2,−195) 382×831` at rest with `translate3d(0,−184px,0)`, `(2,−11)
  382×831` with `translate3d(0,0,0)` after the hand-over. Gradient
  `rgba(237,238,233,0) 16.837% → rgb(237,238,233) 50%`, 88px, and `.pdbg`'s own
  background-image is now `none`.
- **Settings resolve** — stage `0…662`, resolve `592…680`, content opens at
  **596**. All three the mockup's own numbers.
- **Chips** — Grump (cut off right) → row scrolls to 119, chip lands 24px clear
  of the right edge; Sassy → 440, the row's own maximum; Friendly → back to 0,
  chip at the row's 5px pad. Both edges, smooth.
- **Bubble** — `psbub` opacity **1** at rest, `psbub off` opacity **0** at
  scroll 600, **1** again on return.
- **Toggles** — `.psrowcard` switch and `.psqtop` switch both **50×28**, knobs
  both **22×22**.
- **Quiet hours** — card **166 → 76** on toggle off (76 is Auto-watering's own
  collapsed height), subtitle block 41 → 0, tail 51 → 0, the window presets
  removed; **166** again on toggle on, with "Customise" folded shut.
- **Profile** — the full geometry table above; language picker round-trip writes
  **Русский** back onto the card; "5 plants" opens the list (5 cards); two
  chevrons return to the dashboard.
- **Films, all five plants** — thumb 0.75 / 0.50 / 0.25 → film t **1.25 / 2.50 /
  3.75**, growth layer at opacity 1, home restoring t 0 and the still film at
  −184. Unchanged by the re-encode.

## Shipped

Commit `fc0c36e` → `origin/main`, Vercel production
`dpl_Gj7TU7NcN3NuzYYW3WYH7CJW2W9o`, aliased to
**https://bloomling-wireframes.vercel.app** (URL unchanged).

Re-verified against the live site, **no console errors and no exceptions**:

- **Films served at their new weight** — `growth-mary.mp4` **1 360 530**,
  `still-mary.mp4` **876 276**, `settings-vlad.mp4` **340 127** bytes, i.e. the
  optimized encodes and not a stale cache.
- **Live scrub, all five plants** — thumb 0.75 / 0.50 / 0.25 → film t **1.25 /
  2.50 / 3.75** against each plant's own `growth-<id>.mp4`; home returns t 0 with
  the still film back at −184.
- **Settings resolve** — `rgba(237,238,233,0) 16.837% → rgb(237,238,233) 50%`,
  the stage at 0…662 and the resolve at 592…680.
- **Bubble** — `psbub`, opacity **1**, at rest.
- **Toggles** — both **50×28** with **22×22** knobs; the Quiet-hours card **166**
  open.
- **Profile** — identity **161**, "Chat settings" **296**, switch card **362**,
  "Message frequency" **450**, options **484**, language cards **685** / **794**,
  Log out **929**. Every number the mockup's.

---

# Revision 31 — one settings design for two screens, no Saves, and the Add Plant flow rebuilt

## 1 — Quiet hours gets the auto-watering screen's design

Not "the same look" — **the same code**. The labelled three-up that both screens
are made of came out of `WaterConfig` into `AwGroup`, so "these two screens match"
is not a promise two components have to keep in step by hand. The only
difference between them is the words.

**Two groups, not one, and that is the fork.** The card that opens this screen
used to offer three fixed windows inline (`10pm–8am`, `11pm–7am`, `9pm–7am`).
`447:131` draws two labelled three-ups, and the window splits into two the same
way watering does — "silent from 10pm till 8am" and "every 2nd day at 7am" are
the same kind of sentence, assembled from two independent picks. All three of
the old windows are still reachable, and six more with them.

**The window moved onto the plant.** `p.quietFrom` / `p.quietTo`, defaulted in
`quietOf()` rather than written into all five `PLANTS` rows — stating the same
default five times to say nothing new is worse than defaulting once. Personality
& Settings re-reads it on mount, which is the round trip Auto-watering already
makes.

Measured: both screens put their first label at the same y, their first
three-up at the same y, their second label at the same y and their second
three-up at the same y, with the same 105px option cards. Identical, because
it is one component.

## 2 — No Save on either screen

`447:131` draws no button of any kind, and it is right not to: every choice here
is a single tap on a card that lights up, so there is nothing to batch and
nothing to confirm. Each pick now writes straight through to the plant, so Back
is simply Back — and the card that sent you here is already stating what you
chose by the time you land on it. Verified both ways round: picking "5 days"
lands on **"Every 5th day at 7am"**; picking 11pm and 9am lands on **"Vlad stays
silent from 11pm till 9am"**.

`.awsave` is gone with them. The wide sage action it drew lives on as `.actbtn`,
which the Add Plant flow needs and which is the same button.

## 3 — The Add Plant flow

### The final screen list

`465:312` draws five frames and they are all one shape — chevron, centred serif
line, optional muted line, actions — so they are **one component with a state
machine**, not five. `ApStep` is that shape, and every screen in the flow is an
instance of it, including the four the mockups stop short of.

| screen | states | from |
|---|---|---|
| `pair` | idle → searching → found → connecting → paired | the five mocked frames |
| `capture` | frame → reading | designed |
| `identify` | — | designed |
| `species` | — | designed (rebuilt) |
| `create` | — | designed |
| `meeting` | — | designed → Dashboard |

**Gone: `matches` and `speciesDetail`.** Three ranked guesses, then a screen with
a reference-photo carousel, an identification table and a look-alikes list. That
is a species encyclopedia hung off a pairing flow, and nothing in these mockups
has more than one idea on a screen. The read comes back as **one** species to
confirm or override, and the manual list survives as the escape hatch. This is
the "adjust granularity if the mockups imply a different split" the brief allows,
and the mockups imply it loudly.

**`capture` is two states on the pairing screen's own rhythm** — an action, then
a progress line with nothing else on it, then the result on its own screen. The
app has no camera, so the viewfinder draws the frame it would fill rather than
pretending to a picture it does not have.

### The film

`bt.mp4` — the pot with its Bluetooth LED breathing — replaces `462:262` on the
three frames that draw it (found, connecting, paired) and stays off the two that
have nothing to show yet. Looping, muted, `playsInline`, no controls.

**There is no window to place it in.** The mockup's pot occupies x 65…337 /
y 372…685 of the frame; the film's occupies x 158…819 / y 904…1681 of 976×2124;
and 272/661 scales 976 to **401.6** and 2124 to **874.0**. Drawn at the mockup
frame's own size from its top-left corner, the film's pot lands exactly on the
582px pot Figma draws. The "image slot" is just where the pot falls in a
full-bleed film.

- **Sized by HEIGHT — a fixed 874 — and that is the one thing worth arguing.**
  The device is 336×818 against a 402×874 frame, i.e. proportionally taller. A
  film scaled to the WIDTH rides 60px up the screen while the type, top-anchored
  at the mockup's own y like everything else here, stays put — and the headline
  lands on the pot. Pinning the film's height to the frame's keeps every
  vertical the mockup drew; the 33px it then overhangs either side is clipped by
  `.apbg`, exactly as the detail screen's 457px film is clipped to 402.
- **Optimized on the same terms as the rest**: H.264, `-preset slow`, CRF 26,
  `-an`, `+faststart`. **2 091 115 → 170 052 bytes (8.1%)**, SSIM **0.9969** —
  and it arrived with an audio track, which is now stripped.

### The designed steps

Everything below the mockups is assembled from cards this app already owns, so
what you set up on day one is visibly the thing you edit later:

- **`identify`** — the read handed back as one species over the app's own render
  of it, with a confirm and an override.
- **`species`** — a search field over `.psrowcard`, the same 74px card the
  settings screens list everything else in.
- **`create`** — `.psfield` for the name, the `.pschips` row (with rev 30's
  scroll-the-chosen-one-into-view, reused verbatim) for personality, and the
  `.psvoice` grid for Male / Female / Prefer not to choose.
- **`meeting`** — the plant's own in-character line in the app's own pebble,
  over the film of what it is.

Calls:

- **Six presets, not eight.** The brief says eight; `PERSONALITIES.presets` — the
  single source of truth both this screen and Personality & Settings read — holds
  six, and `440:356` draws six. Reusing the source beats hard-coding a number.
- **No sliders on `create`.** Warmth / drama / chattiness belong to Personality &
  Settings, where you tune a character you have already met. Day one asks three
  questions and gets out of the way.
- **The flow actually adds the plant.** `PLANTS` and `CHAT_INIT` are this
  prototype's whole store, and a "done" screen that changed nothing would teach
  the wrong thing about the flow. "Start caring" pushes the plant and its first
  chat line, then lands on the Dashboard.
- **`look`, the borrowed visual identity.** A new plant has no render, no avatar
  and no film of its own, so it carries the id of the plant whose art stands in
  for its species (Ficus → felix, Cannabis → mary, Cactus → gosha, Aloe → vera,
  Bonsai → vlad). One helper, `lookOf()`, and everything that reaches for ART
  goes through it while everything that reaches for DATA still goes by the real
  id. Known limit, accepted: the borrowed film carries the donor's face, so a
  cheerful new cactus greets you over Gosha's scowl. The alternative is a plant
  with no picture.
- **Entry point unchanged** — "Add" on My Plants, and the flow index's "Add new
  plant". Back works at every step; inside the pairing machine it returns to
  idle rather than leaving the flow, because on four of those five frames the
  thing behind you is the previous state, not the previous screen.

## Verification

Headless Chrome over CDP, real `Input.dispatchMouseEvent`, DPR 2. **No console
errors and no exceptions on any screen; no broken images anywhere.**

- **Both settings screens, side by side** — label / three-up / label / three-up
  at the same four y positions on each, 105px option cards on each, `.awsave`
  **absent** on each. Auto-watering reads "Watering every" + "Amount per
  watering"; Quiet hours reads "Silent from" + "Until", opening on **10pm** and
  **8am** as best choices.
- **Write-through** — "5 days" → the Plant Detail card restates **"Every 5th day
  at 7am"**; 11pm + 9am → the Quiet hours card restates **"Vlad stays silent
  from 11pm till 9am"**.
- **The pairing machine** — idle → "Searching for devices…" → "Pot found nearby"
  (film `assets/video/bt.mp4`) → "Connecting…" → "Your pot is paired and online",
  each frame carrying exactly the mockup's copy and actions.
- **The rest of the flow** — "Take a photo" → viewfinder → "Reading your photo…"
  → **"We think it's a Ficus"** → either "Yes, that's my plant" or the manual
  list (16 species, searchable) → `create` → `meeting`.
- **`create`** — typing "Basil", tapping the fifth chip scrolls the row to 355
  and leaves **Cheerful** lit and fully clear of the edge; the voice grid selects.
- **`meeting`** — "Meet Basil" over `still-gosha.mp4` for a Cactus, carrying the
  Cheerful preset's own line.
- **The landing** — the new plant is in the chat (its line, wearing its species'
  avatar), in My Plants as a **sixth** card with that species' render and
  gradient, and its own detail screen plays that species' film.

## Shipped

Commit `015e18e` → `origin/main`, Vercel production
`dpl_AW5EayKrNtNCSwPMbpQ6hoq9Ssv5`, aliased to
**https://bloomling-wireframes.vercel.app** (URL unchanged).

Re-verified against the live site. `performance.getEntriesByType('resource')`
reports **no 4xx on anything the page asks for**; the only console line on a
cold load is the browser's own automatic `GET /favicon.ico`, which this
prototype has never declared.

- **`bt.mp4`** serves 200 at **170 052** bytes, decodes 976×2124, and is drawn
  **402×874 from y 0** inside an 818-tall stage — the mockup frame's own
  geometry, with the 33px either side clipped.
- **Both settings screens** — label 150 / three-up 184 / label 319 / three-up
  353 and 105px option cards on **each**, `.awsave` absent on **each**. "5 days"
  lands on "Every 5th day at 7am"; 11pm + 9am lands on "Vlad stays silent from
  11pm till 9am".
- **The whole flow** walks: idle → searching → found (film) → connecting →
  paired → viewfinder → reading → "We think it's a Ficus", every frame with the
  mockup's own copy and actions.

---

# Revision 32 — the pairing film becomes the wait, the camera arrives, and the detail screen loses its band

## 1 — The working ellipsis

One dot, two, three, then all three go and it starts again: 450ms a step, 1.8s a
cycle, on `ease-in-out` so each one arrives rather than snaps.

**Opacity, not content.** The three dots are always in the line, so its width
never changes and the centred serif headline never jumps as the animation runs —
which is the whole reason not to do this by rewriting `textContent`. Three
keyframe sets rather than one plus `animation-delay`, because each dot appears
at its own moment but they all leave together, and a single set cannot say that.

**Applied to all three progress lines**, not just the one asked for: "Searching
for devices", "Connecting" and "Recognising your plant" are the same sentence in
three places, and a loader that animates on one of them and not the others reads
as a bug.

## 2 — The pairing film IS the connecting animation

It used to loop as decoration beside a 1.6s timer. Now it holds on its **first
frame** while the pot is merely found, plays **once** when Connect is pressed,
and its own `ended` is what advances the screen to "Your pot is paired and
online", where it stays frozen on its **last frame**. The wait is the pot's own
five seconds of LED rather than a number someone chose, and this component has
one fewer constant than it did.

One element across all three film frames — same `src`, same `key`, so React
keeps it — which is what makes "press Connect on the film that is already
showing you its first frame" true rather than approximately true.

- **"Connecting" keeps its own frame**, which the brief does not require. The
  text has to say something true while those five seconds run, and `465:286`
  already decided what.
- **Back rewinds.** Leaving the machine pauses the film and returns it to 0, so
  coming back in finds the first frame again rather than wherever you left.

Measured: held at **t 0, paused** on arrival; **t 1.71 → 3.71** while
connecting; **t 5.04, paused, ended** with the title changed.

## 3 — The camera (417:1591)

A full-bleed viewfinder and a shutter, and nothing else — the mockup draws no
chrome at all. `add-plant.mp4` plays it, muted, `playsInline`, no controls,
**sized by HEIGHT to the mockup frame's own 874** and centred: 874 × 1176/1756 =
585.4, which is `472:319`'s own width to half a pixel. Same rule the pairing film
follows, same reason — it keeps every vertical the mockup drew on a device that
is proportionally taller.

- **The viewfinder needs its own clipping box.** `472:319` starts at the frame's
  y 0, i.e. behind the status bar, so `.cambg` is that box (`top:-48`,
  `overflow:hidden`) and `.camscreen` must NOT clip, exactly as `.pdbg` is one
  for the detail screen's film. The first pass clipped at the screen and the
  film simply started 48px too low.
- **The shutter is bottom-anchored**, at the 65px the mockup leaves under it
  (874 − 809), not at an absolute 809. The device is 818 against an 874 frame,
  so a top-anchored 809 hangs it off the bottom edge — which is exactly what the
  first pass did.
- **The chevron is added back.** The mockup draws none; a flow you cannot back
  out of is not a flow. White with a drop shadow, because what is behind it is a
  photograph.

## 4 — Shutter → freeze → recogniser

One gesture, three effects: kill the loop (or the freeze wraps to 0), pause, and
seek to `duration − 0.04`. The recogniser raises over the frozen frame for 1.8s.
It is the app's own `.spin` geometry in white on a 45% scrim, because what it
sits on is a photograph and a sage ring on a page tint would vanish into it.
Measured after the tap: **t 2.918 of 2.958, paused, loop off**.

## 5 — The result

"Looks like it's a Ficus", and the grey helper line is gone — the two buttons
under it already say what the choices are.

**The photo is `add-plant.mp4`'s last frame exported once, not a live `<video>`
seeked to its end.** A still is what this screen actually shows; it survives the
screen change with no playback state to carry across, it cannot end up a frame
off the one the shutter froze, and it is 72KB against a second video element.
The box becomes 240×320 and `object-fit:cover`, because a photograph has no
transparent margin to sit politely inside one.

The photo only exists for the species the recogniser returns by default. A
manual override lands on `create` directly, so the mismatch never appears.

## 6 — The personality step is the personality section

Not "the same design" — the same component. `PersonalitySection` owns the chips
row, the scroll-the-chosen-one-into-view, and the three sliders, and both
Personality & Settings and the Add Plant flow render it.

**This reverses rev 31**, which shipped the flow with chips and no sliders on the
reasoning that fine-tuning belongs to a character you have already met. The brief
says otherwise, and one shared component is the only way to be sure the two
screens cannot drift apart again — which was rev 31's own argument for the chips,
applied properly.

Measured on both screens: the same six chips, the same `.pscard`, the same three
sliders reading **Warmth / Drama / Chattiness**, the same defaults (84 / 50 / 84
for Friendly), and the reveal scrolling the row identically.

*Six presets, not eight — the brief's number again. `PERSONALITIES.presets` is
the single source of truth both screens read, and `440:356` draws six.*

## 7 — Plant Detail loses the band

Rev 30 laid 88px of transparent-into-page over the film's bottom edge, welded to
it by the film's own box and transform. It is **removed**. As the cards climb the
film it reads as a second, competing edge over the one the progressive blur is
already softening, and two treatments doing one job is one too many. The blur is
the whole of it now — four masked `backdrop-filter` layers at 8 / 14 / 26 / 48px,
untouched.

**Personality & Settings keeps its resolve.** That screen has no blur, so there
the band IS the treatment rather than a rival to one — and `441:421` is still the
mockup for it.

## The film

`add-plant.mp4` on the same terms as everything else in `assets/video`: H.264,
`-preset slow`, CRF 26, `-an`, `+faststart`. **4 603 040 → 855 388 bytes
(18.6%)**, SSIM **0.9869**, and it arrived with an audio track, now stripped.

- **0.5s GOP (`-g 12` at 24fps).** The clip is seeked to its own end exactly
  once, when the shutter freezes it, and a keyframe near there makes that
  instant. Cheaper than the growth films' 0.25s because one seek is not a scrub.
- **Resolution unchanged at 1176×1756.** Drawn 585×874, that is 1170×1748 at
  DPR 2 — the source, near enough exactly.
- **SSIM 0.9869 is lower than the pot films' 0.993–0.997 and that is expected**:
  this is a real photographic scene with window detail and bokeh, where those
  are a product render on a seamless background. Inspected at full size, no
  visible artefacts.
- `photo-addplant.webp`, the exported last frame: 720×1075, **72 366 bytes**.

## Verification

Headless Chrome over CDP, real `Input.dispatchMouseEvent`, DPR 2. **No console
errors and no exceptions on any screen; no broken images anywhere.**

- **Dots** — three `<i>`, three distinct keyframe names, 1.8s each.
- **Connect** — t 0 paused on arrival and still 0 a beat later; 1.71 → 3.71
  playing; 5.04 paused and `ended`, title "Your pot is paired and online".
- **Camera** — screen 336×770, film **585×874** at x −125 (centred) and y −48
  (behind the status bar), shutter **84×84** at (126, 663) i.e. its box 23px off
  the foot and its centre **65px** off it, which is `472:315`'s own clearance
  (874 − 809). Chevron at (14, 37).
- **Shutter** — t **2.918** of 2.958, paused, loop off, overlay up with
  "Recognising your plant…".
- **Result** — "Looks like it's a Ficus", **no** `.apsub`, the photo at 240×320
  from a 720×1075 source.
- **Personality** — identical chips, card, slider labels and values on the
  settings screen (`.pssec`) and in the flow (`.apsec`); tapping the sixth chip
  scrolls the row to 440 and re-tunes the sliders to that preset.
- **Plant Detail** — `.pdresolve` **absent**, `.pdbg` background-image **none**,
  the four blur layers present and ramping 8 / 14 / 26 / 48px.
- **End to end** — pair → camera → shutter → result → name + preset → meeting →
  Dashboard, with the new plant in the chat and a sixth card in My Plants. Back
  steps out of the machine to idle, then out of the flow to My Plants.

## Shipped

Commit `00cdaa0` → `origin/main`, Vercel production
`dpl_B5vDmV3eCwvQjcKAGNuvqCvMCaSo`, aliased to
**https://bloomling-wireframes.vercel.app** (URL unchanged).

Re-verified against the live site. The only console line is the browser's own
automatic `GET /favicon.ico`, which this prototype has never declared.

- **Assets** — `add-plant.mp4` **855 388** bytes, `photo-addplant.webp`
  **72 366**, both 200.
- **Dots** — three `<i>`, `apdot1` / `apdot2` / `apdot3`, 1.8s each.
- **Connect** — held at **t 0 paused**, then 1.71 → 3.71 playing, then **5.04
  paused and `ended`** with the title on "Your pot is paired and online".
- **Camera** — film 585×874 at (−125, −48), shutter 84×84 at (126, 663),
  chevron at (14, 37); the shutter freezes the film at **t 2.918 of 2.958**,
  paused with the loop off, and raises "Recognising your plant…".
- **Result** — "Looks like it's a Ficus", no grey helper line, the frozen photo
  at 240×320 from a 720×1075 source.
- **Plant Detail** — `.pdresolve` absent, `.pdbg` background-image `none`, the
  four blur layers ramping 8 / 14 / 26 / 48px.

---

# Revision 33 — the flow's films find the top edge, and its character step becomes the real screen

## 1 — The pairing film, 40px up

`.apvid.up40`, applied to **all three frames that show it** — found, connecting
and paired — not the two the brief names. They carry one continuous take across
one persistent `<video>` element, so shifting two of the three would make the pot
jump on `ended`, which is the one moment of that screen anybody watches.

## 2 — "Take a photo of your plant"

The button on the paired frame. It is the only button on the screen and it now
says what it takes a photo *of*.

## 3 — The character step IS Personality & Settings

Not "the same design" — the same construction. `PsShell` came out of
`PlantSettings` and holds everything on that screen which is not a settings
**section**: the film stage and the cover rule that sizes it, the resolve, the
scrim, the pinned nav, the plant's bubble, and the one rAF-coalesced scroll read
that drives all four. Both screens render it, so there is no copy of any of it
left to drift.

What the flow gets, for free and by construction: the pot breathing behind the
chips, the line coming off it as you move a slider, the film shrinking under the
cover rule as you scroll, the scrim arriving as the content reaches the title,
and the bubble withdrawing when the film has gone.

**The film is Felix's**, per the brief — `settings-felix.mp4`, the app's
reference pot, because the plant being set up has no footage of its own and the
flow has not yet decided whose it borrows. It routes through
`psFilm()`/`filmId()`, so the standing fallback rule still applies.

**Which sections survive, and this is the call the brief asked to be logged:**

- **Personality** — in full, as asked: chips, scroll-to-selected, sliders card.
- **General — trimmed to name + voice, not dropped.** Dropping it whole would
  leave the flow with no way to name the plant, and the very next screen greets
  it by name ("Meet John") and the app files it under one. "Not needed here"
  cannot have meant that. The **species row is** dropped — the screen before this
  one just decided it, and offering to re-edit it here would undo that step.
- **Activity — dropped whole.** Message toggles and quiet hours are about living
  with a plant you already have; there is nothing to be quiet about yet.

The mockups stop at the photo step, so there is nothing in them to follow here
either way.

The title is **"Name and nature"** rather than "Personality & Settings": the
shell is the same, but the pinned line should say which step of the flow you are
on, and this one sets exactly those two things.

## 4 — The final screen's film reaches the top edge

It was not reaching it, and the cause was one word. `.apscreen` carried
`overflow:hidden`, so `.apbg`'s `top:-48px` was simply cut off and **every** film
in this flow started a status bar's height below the top of the device, with a
band of flat page above it. `.apscreen` is now `overflow:visible` and `.apbg`
does the clipping — the detail screen's exact arrangement, and the same fix rev
32 made for `.camscreen` after the same mistake.

Measured, sampling the column just inside the phone's left edge at the device's
first rows: **before** `rgb(237,238,233)` — flat `--bg` — for the first 16px,
then the film; **after** the film's own `rgb(208,202,191)` from the very first
row. Applied to the whole flow rather than the one screen, because it is one
shell and the mockups draw every film from the frame's own y 0.

## Verification

Headless Chrome over CDP, real `Input.dispatchMouseEvent`, DPR 2. **No console
errors and no exceptions; no broken images anywhere.**

- **Pairing film** — `apvid up40` at **y −40** relative to the device top on
  found, connecting and paired alike; the button reads **"Take a photo of your
  plant"**.
- **Character step** — `.psscreen` shell, `settings-felix.mp4`, stage
  **336×662**, film **691×857** at rest (Felix's own 1292×1604 ratio through the
  cover rule), the resolve gradient present, the bubble live at opacity 1 and
  following the preset (tapping "Drama queen" swaps the line and re-tunes the
  sliders to 50/84/84), sections **Personality** and **General**, six chips,
  three sliders, the three voice options.
- **Personality & Settings, unchanged by the extraction** — Vlad's own
  `settings-vlad.mp4` at **744×857**, all three sections, resolve at top 592
  height 88, content opening at **596**; scrolled to 200 the film is 401 wide,
  at 600 the scrim is solid and the bubble is at opacity 0, and home restores
  744 and 1. "Customise" still opens Quiet hours.
- **Final screen** — film top flush with the device top (0), title "Meet John".
- **End to end** — pair → camera → shutter → result → name + preset + voice →
  meeting → Dashboard, with "John" in the chat and a sixth card in My Plants.
  Back steps out of the machine to idle, then out of the flow to My Plants.
