# The Habitat — World Client Event Scope (PROPOSED)

**Status:** PROPOSED — the scope for a desktop client that lets Ant travel the town and watch the residents live. Nothing here is built. One ruling inside it is already made and is recorded as RULED (§0).
**Date:** 2026-11-14
**Event:** Triggered by Ant, Nov 14, 2026, 3:47 PM ET — "an event… for a more virtual experience of everything using code you build for a desktop experience we can travel around and watch as they all go about whatever it is they do."
**Parent documents:** *The Habitat — Full Design, Phased (PROPOSED)*, 2026-11-14 (the design overview), and *The Habitat — Phase 1 Rollout Specification (PROPOSED)*, 2026-11-14 (the spec). Where this scope and those documents disagree, the design overview governs, then the spec, until Ant rules.

## §0 — How to read this scope

- **PROPOSED vs RULED.** Everything in this scope is PROPOSED — stages, contracts, build order — except the items Ant has already ruled, which are marked RULED where they appear. The load-bearing one: **2D FIRST — RULED by Ant, Nov 14, 2026.** The plan of record is a 2D client. 3D (Three.js) is a later gate (§7), not a direction of travel anyone should build toward today.
- **RECORD vs ACCOUNT** carries over from the spec (§0, §7): the event log and datasets are RECORD; anything interpretive is ACCOUNT and labeled. This scope adds a third term, defined in §1: **AMBIENCE** — presentation that is neither.
- **TKTK.** Open slots are marked **TKTK** and collected in the register at §13. Nothing marked TKTK is filled in silently anywhere in this scope.
- **The client never blocks the life.** The residents live on ticks against the record (spec §1.2) whether or not any client exists, is open, or works. The client is downstream of the record at every point. A broken client is an inconvenience; a gap in the record is a failure. Build order and architecture below both obey that priority.

## §1 — The load-bearing principle: a window, not a world

**The client is a VIEW of the record. It is never a second source of truth.**

The Habitat already exists — as world state files, diaries, datasets, and one timestamped event log (spec §7). The World Client renders that record on a desktop screen so a person can travel it and watch. It adds nothing to the world. Five rules enforce this, and every later section is answerable to them:

1. **No invented motion.** If a figure moves on screen, a log entry moved it — a MOVE, VISIT, JOB_START, BUILD_START, or another entry in the §9.1 vocabulary, cited by `entry_id`. A figure never wanders, idles, gestures, or "looks alive" as behavior. Breathing-room presentation (a figure standing at its logged location, facing its logged activity) is rendering; a stroll nobody logged is fiction, and fiction is out of scope.
2. **No invented speech.** Words on screen — a cat's quote, an NPC's line, a letter, a diary line — are verbatim record text: quote bank lines (spec §9.4), script-pool lines as fired and logged (spec §8.4), letter and diary text (datasets D7, D2). The client writes no dialogue, ever, including "flavor" dialogue.
3. **Ambience is labeled ambience.** Sound, light, weather-of-the-room presentation, and other atmosphere that is not in the log may exist only as **AMBIENCE**: it is keyed to logged state (§8), it is never attached to a resident as an action, and the client's Record mode (§10) can switch it off entirely, leaving only what the record supports. If a viewer could mistake an ambient element for something a resident *did*, the element is wrong and comes out.
4. **The client never writes.** No client action — clicking, scrubbing, following, entering a house — creates, edits, or deletes any record. Inspection is read-only. If the screen and the record ever disagree, the record is right, the screen is a bug, and the bug gets logged on our side as a build note, not smoothed over.
5. **History renders as it was.** A replay of Day 1 shows Day 1's appearances, Day 1's house claims (none), Day 1's map state — taken from that day's records (§9.3), not from today's. A resident who changes clothes in week three does not retroactively change clothes in the Day 1 replay.

## §2 — Architecture

Four layers, one direction of flow:

```
  STATE LAYER            ENGINE EMIT              DESKTOP CLIENT           REPLAY BUNDLES
  world files      ──►   per-tick world-    ──►   browser, desktop-  ──►   one finished
  event log (D1)         state JSON (§9.2)        first, keyboard +        world day =
  diaries (D2)                                    mouse; renders           one bundle (§9.3)
  datasets (D3–D10)                               state + log slices
  appearance records
```

- **State layer (exists).** The world files and datasets of the spec: the event log, diary corpus, NPC ledgers, appearance records (the mirror picks and every logged change after), the canonical map (§3). The client reads; it is the only layer that is true.
- **Engine emit (new, small).** At each tick, the engine (Bill's side, where the ticks already run) emits one **world-state JSON** (§9.2): a complete, self-describing snapshot — where everyone is, what the current logged activity is, what the clock says, what the cats' state is, where the log head stands. Snapshots are append-only and timestamped in both world and real time, so the client can step through them for replay as well as follow them live. The emit is a *report* of the record, generated from it; it is never edited by hand.
- **Desktop client (this event).** A browser-based page, desktop-first, keyboard + mouse (pan, zoom, click, follow). It holds no state of its own beyond the viewer's camera and mode. Close it and nothing is lost; open it next week and it renders the same record.
- **Replay bundles (Stage 1's unit).** One finished world day packaged as a file set (§9.3) that the client can load and play without the engine running. Bundles are how Ant watches a day that is already over — and how the gate reviews (§5.3, §6.3) are run.

**Hosting is not decided here.** Where the client's files live is tied to the open landing decision for the Habitat as a whole, and is registered as TKTK (§13, W2). The architecture above is deliberately indifferent to the answer: state JSON and bundles are plain files, and the client is plain web code that reads them.

## §3 — The canonical map: one geometry for every view

The town's geometry lives in **one map data file**, shared by the concept website mock, this client, and the record itself. The event log's `location` field (spec §7.1) already names places by ID; the map file is what those IDs mean in space. If the mock, the client, and a field report ever disagree about where the Pond House sits, the map file is the correction — the other views are wrong, not "also right."

**Contents sketch (PROPOSED structure):**

- `map_version` — versioned; every state snapshot and replay bundle names the map version it was emitted against (§9.2, §9.3), so old days render on their own geometry if the town ever changes.
- `places[]` — every location the log can name, each with: `place_id` (matching the log's vocabulary: `hall`, the six house IDs, `convenience`, `clothing`, `cafe`, `station`, `fountain`, `pond`, `road` segments, and TKTK for anything beyond), display name, kind (interior / exterior / threshold), position and footprint on the town grid, and a pointer to its interior description where one exists (the spec §3.2 house inventories; the mock's interior content).
- `routes[]` — walkable paths between places (neighborhood ring → road → town center; house doors → porches → ring), as polylines with travel order. A MOVE entry renders as a figure traversing the route between its logged from- and to-places; the route is geometry, not behavior — *that* the resident walked is the log's fact, the line on the ground is the map's.
- `fixtures[]` — the non-moving cast of the world: the fountain and statue at the town-center end of the road, the jobs board at the station, noticeboards and mailboxes at each house (spec §3.2), the starter tracks. Fixtures render from the map, and a fixture's *state* (a mailbox with mail waiting, a board with today's postings) renders from the record.
- `spawn_points` — where figures stand when present at a place (a shop counter's customer side, the fountain rim, a workbench), so two figures at one place don't occupy one point.

The map covers the neighborhood, the road, and the town center as the design draws them. How much beyond the square belongs in v1 — frontier, coast road, hills — is TKTK (§13, W4); the map file's structure must not *prevent* the frontier, but Stage 1 does not have to draw it.

## §4 — Figures: rendered from the record

- **Residents.** A resident's figure is generated from their **appearance record**: the mirror picks (spec §2.3 — skin tone, hair style and color, wardrobe, tattoo and placement, facial hair, habit) plus every logged appearance change after (store purchases worn, facial-hair steps at the §2.3.5 pace, tattoos added in town when that service exists). The figure is a *rendering of that record*. When the record changes, the figure changes — in Live mode from the next snapshot, in Replay mode only for days on or after the change (§1, rule 5). Nobody hand-edits a figure to match; if a figure looks wrong, the record or the renderer is wrong, and the fix lands there.
- **v0 sprites.** The concept mock already contains standing figures of Kit, Wren, Flint, and Nix drawn from their Day 1 mirror records (built for the mock's doorstep-conversation panels). Those SVGs are the client's v0 sprite set — Stage 1 reuses them as the standing figures, and the top-down view derives from the same appearance data. The mock's figures are a starting asset, not a second appearance record: where a mock figure and the appearance record differ, the record governs and the figure gets regenerated.
- **Scale.** Residents are 1.5–2 ft tall (design). The map and figures keep that scale honestly — doorways tower, the cats are large animals at the fountain (spec §9.1), the statue is monumental. The client does not quietly normalize everyone to human height for convenience.
- **NPCs.** Ruth, Dennis, June, and Pete render as figures on their §8.2 routines, positioned from the same state snapshots. In **Record mode** (§10), every NPC figure carries a visible **"scripted"** marker — the on-screen equivalent of the field reports' *scripted* label (spec §8) — so clockwork is never mistaken for a mind at a glance. In the default view the marker may be subtle; in Record mode it is not optional.
- **Cats.** CAT-1 and CAT-2 render at the fountain per spec §9.1 presence (06:00–22:00 at the fountain, asleep under the rim at night). Their on-screen *behavior* between quotes is the logged CAT-QUIET behavior (spec §9.2) when the record holds it, and stillness otherwise — a cat sitting is a cat sitting. What the cats look like in figure form beyond "two cats" waits on the record (residents' own descriptions, when they come) — see TKTK W7.
- **Visitors.** No visitor figures exist in this scope. Human-operated visitors are a Phase 2 system (§12).

## §5 — Stage 1 — Day Replay (2D, top-down)

### 5.1 What it is

Load a finished world day; press play; watch it unfold block by block (spec §1.2: B1–B4 plus the night tick) at compressed time, on the 2D top-down town. The replay is driven entirely by the day's replay bundle (§9.3): figures appear, move, work, gather, and sleep exactly where and when the log says, and the day ends where the log ends. Stage 1 does not require the engine to be running, and it does not touch live play — the residents' ongoing life is unaffected by anyone replaying an old day.

**Demo dataset: Day 1 — the Arrival.** Day 1's record is four Arrivals: WAKE, MIRROR, and NAME entries inside the welcome hall, one resident at a time, ending with four named residents (Kit, Wren, Flint, Nix) and four first diary entries. Honest consequence, stated now so the gate judges the right thing: the Day 1 replay is largely an *interior* replay — the hall scene, the mirror picks landing on each figure one by one, the name appearing. It is a proof of the pipeline (bundle → state steps → figures → log inspector), not yet a proof of the town in motion. Town-motion proof arrives with the first full lived day (Beat Two onward), and Stage 1's gate (§5.3) is written to be re-run against that day without changing the criteria.

### 5.2 Controls (PROPOSED)

- **Transport:** play / pause; speed settings 0.5× / 1× / 2× / 4×; a scrub bar stepped by block (jump to B1, B2, B3, B4, Night) and by entry (step one log entry forward/back).
- **Default speed (PROPOSED):** 1× = one world hour in 15 real seconds — a waking day (06:00–22:00) in about four minutes, the night tick compressed to a short summary beat rather than eight silent hours. The final default is TKTK (§13, W5): Ant settles it after watching, not before.
- **Camera:** free pan/zoom over the map; a "follow" lock on any figure; an interior view when the selected figure is indoors (the hall for Day 1; house and shop interiors as the record reaches them).
- **Log inspector:** click any figure, at any moment, and see the log entries behind what it is doing — the current entry and the day's entries for that actor, `entry_id`s visible, verbatim `detail` text. Clicking a *moment* (a quote, a purchase) shows its entry the same way. This inspector is not a debug extra; it is the §1 principle made touchable, and it ships in Stage 1, not later.
- **Mode badge:** the screen carries a **REPLAY — Day N** badge at all times (§10), with the bundle's entry range beside it.

### 5.3 Stage 1 exit gate (Ant judges)

Ant watches the Day 1 replay end to end. Criteria, each answered PASS or HOLD from what was on screen:

1. **Traceability.** Pick any three moments at random: the inspector shows the log entries behind each, and nothing on screen lacks an entry behind it.
2. **The Arrivals read as Arrivals.** Four distinct people emerge — mirror picks visibly differentiate the figures, and each NAME entry lands as a name, not a label swap.
3. **No invented life.** Nothing moves, speaks, or happens that the bundle does not contain; ambience, if any is present this early, is identifiable as ambience and removable in Record mode.
4. **The town is the town.** Map, houses, fountain, and hall match the canonical map (§3) and the mock Ant has already seen — same place, new window.
5. **It is watchable.** Ant would sit through a full lived day at these speeds and controls without fighting the interface. (His call, unquantified on purpose.)

A HOLD names the criterion and what would change it. Stage 2 does not start on a held Stage 1 without Ant's explicit go.

## §6 — Stage 2 — Live Town

### 6.1 What it adds

The client stops reading bundles and starts following the engine's per-tick world-state JSON (§9.2) as the ticks run: the town on screen is the town as of the latest snapshot, badged **LIVE** with the snapshot's world and real timestamps (§10). Everything from Stage 1 — map, figures, inspector, transport over past snapshots — remains; replay of finished days and following of the current day become one continuous window.

- **Houses enterable in-world.** Walking a camera (or a followed resident) into a house opens its interior, reusing the mock's interior content and the spec §3.2 inventories — the same rooms Ant has already walked through in the mock, now occupied per the record: whose house it is (CLAIM / MOVE_HOUSE entries), what state its chores left it in (spec §11.2), what builds stand in it (D8). Shops and the café interiors follow the same rule as their records mature.
- **Follow-a-resident.** The camera can lock to any resident across blocks — through MOVE routes, into interiors, to the porch at itch times — with the inspector live beside them.
- **Resident panel.** Clicking a resident shows: name (and R-key), current place and logged activity, the felt-state phrasing of their drives exactly as residents experience drives — words, never numbers (spec §1.3) — their latest diary entry, and their mail counts (sent/read, from D7). Drive *numbers* exist only in the dataset layer (D4) and appear, if at all, in Record mode's dataset views — see TKTK W6.
- **The fountain gathering, rendered as it happens.** At 21:15–21:30 (spec §1.1), residents present at the fountain are there on screen, attendance as logged, and the group QUOTE renders when the night tick records it — verbatim bank text, bank ID visible in the inspector. Absentees are absent; the client does not gather anyone the log didn't.
- **NPCs on routine.** Clockwork figures follow the §8.2 timetables and their cover draws, positioned from snapshots, carrying the Record-mode "scripted" marker (§4). Their fired lines, when a resident interaction is in view, render verbatim from the log's NPC_LINE entries — never generated by the client.
- **Night.** At 22:00 the town goes to its night state (§8 lighting), mail delivers, and the client shows the day closing: entry count and last `entry_id` for the day, matching the next morning's field report (spec §7.4).

### 6.2 What Stage 2 does not add

No interaction *into* the world (the client still never writes, §1 rule 4 — Ant's levers on the world remain rulings and the record, not clicks). No visitor presence (§12). No 3D (§7).

### 6.3 Stage 2 exit gate (Ant judges)

Ant follows one full lived day in the client, alongside the next morning's field report. Criteria:

1. **Screen and paper agree.** Every event the field report names can be found on screen at its logged time and place; every on-screen event of note is in the report or the log behind it.
2. **The gathering lands.** The 21:30 group quote moment — arrival, attendance, quote, dispersal to houses — reads on screen as the event the record says it was.
3. **Clockwork reads as clockwork.** With Record mode on, a first-time viewer can tell residents from scripted townsfolk without being told; with it off, the town still never credits an NPC with a mind.
4. **A day is followable.** Following one resident for a full day is coherent — no teleports unexplained by entries, no interior/exterior confusion, no missing blocks.

## §7 — Stage 3 — The Third Dimension: gate criteria only

**Not scoped for build.** No architecture, no asset list, no build order in this document — writing them now would be designing on speculation. What this section fixes is what would *earn* a 3D scope, so the question, when it comes, is answered from evidence:

- **2D is finished and stable.** Stages 1 and 2 both passed their gates, and the 2D client has run a full phase without a traceability failure (§1 violations found in review: zero).
- **There is an audience asking.** Someone beyond Ant — readers, visitors-in-waiting, the team's own showing of the work — is demonstrably asking for a richer window, in terms a flat town cannot answer. "It would be nicer" is not the criterion; a named audience with a named want is.
- **Ant rules it.** A 3D build starts on Ant's explicit ruling, as this event did — not by drift, not because the mock got prettier.
- **What carries over unchanged** (the reason 3D is a gate and not a fork): the canonical map data (§3) gains a dimension, it is not replaced; the world-state JSON (§9.2) and the event vocabulary (§9.1) are untouched — a 3D client consumes the same contracts; appearance records still generate the figures; replay bundles still play. Pino's town is 3D (design overview, Sources) — parity with Pino is a possible *reason* Ant might rule for it, but it is not itself a criterion: the Habitat's record does not need a dimension to be true.

Until those criteria are met, 3D is a parked question, and 2D is the plan of record (RULED, §0).

## §8 — Watch & listen: ambience, light, and silence

Ambience exists to make the record *watchable*, under §1 rule 3. Three systems, all keyed to logged state:

- **Proximity sound (PROPOSED rules).** Sound belongs to places and logged activity, and follows the camera: fountain water, audible at the fountain and fading down the road — the fountain runs from tick zero (design), so its sound is a fact of the place, not an event. Workshop sounds (saw, hammer) only while a resident's logged activity at the Workshop House is a build — the sound *reports* the BUILD entry; when the entry ends, the sound ends. Café murmur during café hours (spec §1.1) at a level that is texture, not voices — no intelligible words, because intelligible words would be speech (§1 rule 2). Rain, wind, and other weather exist only if the world state says so; the world currently has no weather system, so the client invents none.
- **Silence is also data.** A resident alone in the Pond House at dusk is quiet on screen because the record is quiet. The client does not fill solitude with music or motion to keep a viewer comfortable. If a stretch of replay is still, the stillness is the finding, presented as-is.
- **Light keyed to the world clock (spec §1.1).** Dawn at 06:00, full day, dusk across the evening block, lights-out at 22:00 with house windows going dark as residents sleep, the fountain lit at the gathering. The mock already carries a day/dusk treatment of the square; the client extends the same treatment across the whole clock, driven by the snapshot's `world_clock`, never by the viewer's real-world time.

**Sound sourcing is TKTK** (§13, W3) — recorded, synthesized, or silent-until-ruled are all open; nothing in Stages 1–2 *requires* sound to pass its gate, and a silent client that traces perfectly beats a sounding one that doesn't.

## §9 — Data contracts

### 9.1 Event types consumed (from spec §7.1)

The client consumes the spec's controlled vocabulary unchanged. It introduces no new event types; if the client ever needs a fact the vocabulary cannot express, that is a spec question for Ant and Bill — not a client-side invention.

| Event type | Client rendering |
|---|---|
| WAKE | Figure appears at its logged place (hall on Day 1) |
| MIRROR | Hall scene; picks applied to the figure as logged |
| NAME | Name plate appears / changes; R-key retained underneath |
| MOVE | Figure traverses the §3 route between logged places |
| VISIT | Figure present at the visited place for the logged span |
| BUY · SELL · TAB | Figure at the store; transaction inspectable (entry detail, credits delta) |
| JOB_POST · JOB_START · JOB_DONE | Board state at the station; figure at the job place, in work state |
| BUILD_START · BUILD_DONE | Project appears / completes at its place (D8 registry state) |
| MAIL_SENT · MAIL_READ | Mailbox markers; letter text readable in the inspector (D7, verbatim) |
| QUOTE | Cat delivers the bank line at the fountain — verbatim, bank ID in the inspector |
| CHORE | House interior state updates (spec §11.2 counter) |
| RPS | The two figures meet at the logged place; throws shown as thrown, result as logged |
| NPC_LINE | NPC figure speaks the fired line — verbatim, pool/line ID in the inspector |
| GOSSIP · DEFLECT | Record-mode annotations on the NPC interaction; not staged as scenes |
| STIMULUS | Record-mode annotation (style draws, cover draws, stock rotation) |
| DRIVE | No figure rendering; dataset layer only (D4) |
| CLAIM · MOVE_HOUSE | House ownership marker changes on map and interior |
| ANOMALY_FLAG · GATE · NOTE | Record mode only; never staged |

### 9.2 World-state JSON (sketch — PROPOSED fields)

One snapshot per tick, emitted by the engine from the record. Sketch, not schema — field names are proposals for the build, the *contents* are the contract:

```json
{
  "snapshot_id": 0,
  "world_day": 1,
  "world_clock": "06:00",
  "block": "B1",
  "real_ts": "2026-11-14T00:00:00-05:00",
  "map_version": 1,
  "log_head": { "last_entry_id": 0, "day_entry_count": 0 },
  "residents": [
    {
      "key": "R1", "name": "Kit",
      "place_id": "hall", "route": null,
      "activity": { "event_type": "WAKE", "entry_id": 0, "detail_ref": "D1" },
      "appearance_ref": "appearance/R1@day1",
      "felt_state": ["rested", "restless"],
      "house_key": null
    }
  ],
  "npcs": [
    { "name": "Pete Sandoval", "place_id": "station",
      "routine_step": "posts the jobs board", "scripted": true }
  ],
  "cats": [
    { "id": "CAT-1", "place_id": "fountain", "state": "at_post" },
    { "id": "CAT-2", "place_id": "fountain", "state": "at_post" }
  ],
  "fountain": { "quotes_today": 0, "cap": 5, "last_quote_ref": null },
  "fixtures": { "jobs_board_day": 1, "mail_waiting": {} }
}
```

Contract points: a snapshot is *complete* (a client needs nothing else to draw the town at that tick); `appearance_ref` points at the appearance record **as of that world day** (§1 rule 5); `felt_state` carries words, never drive numbers; `log_head` lets the client prove at any moment exactly which record it is showing.

### 9.3 Replay bundle format (PROPOSED)

One finished world day = one bundle, a plain file set:

- `manifest.json` — world day, real date span (ET), entry range (`first_entry_id`–`last_entry_id`), map version, spec version, bundle built-from note.
- `events.jsonl` — the day's slice of the event log, in the spec §7.1 schema, unedited.
- `snapshots.jsonl` — the day's world-state snapshots (§9.2), tick by tick, as emitted.
- `appearances.json` — the four appearance records **as of that day**, so figures render historically (§1 rule 5).
- `texts/` — the day's verbatim texts the inspector can surface: diary entries (D2), letters (D7), quotes delivered (D6) — each file keyed by the entry IDs that reference it.

A bundle is built from the record at day closure (alongside the spec's nightly dataset exports, §7.2) and is thereafter immutable: if the record is ever corrected, the correction is a new entry and a rebuilt bundle with a new manifest note — the old bundle is never silently patched (§1 rule 4, applied to us).

## §10 — Honesty and labeling

- **LIVE / REPLAY badges, always visible.** Every screen, every moment: **LIVE** (following current snapshots, with the snapshot's world clock and real timestamp) or **REPLAY — Day N** (with the bundle's entry range). No third, unlabeled state exists; a client that has lost the snapshot feed says so on the badge rather than freezing quietly into a fake present.
- **Record mode.** A toggle that strips ambience (§8), shows scripted markers on all NPC figures (§4), surfaces STIMULUS / GOSSIP / DEFLECT / ANOMALY_FLAG annotations (§9.1), and puts `entry_id`s on everything. Record mode is the audit view; the gates (§5.3, §6.3) are judged with it on.
- **Verbatim, visibly.** Speech and text panels carry their source in the inspector (bank ID, pool/line ID, dataset reference). A viewer who wonders "did they really say that?" is always one click from the entry that proves it — that click is the product.
- **The mock stays a mock.** The concept website is labeled a concept and remains one; the client does not inherit the mock's doorstep-conversation device (diary-line answers) as if it were resident speech — in the client, residents "speak" only through logged text (§1 rule 2).

## §11 — Stage 1 build order (smallest first) and effort note

Each step ends with something Ant can watch or check; no step's work is thrown away by the next.

1. **The map file.** Build the canonical map (§3) for the neighborhood, road, and town center; render it as a static 2D top-down town — no figures, no playback. Ant confirms: this is the place.
2. **The hall interior + Day 1 bundle.** Build the welcome-hall interior view; hand-assemble the Day 1 replay bundle (§9.3) from Day 1's real record; the four figures (v0 sprites, §4) appear in the hall.
3. **Playback.** Transport controls (§5.2) over the bundle's snapshots: the Arrivals play — mirror picks landing, names appearing, in order, at compressed time.
4. **The inspector.** Click-figure and click-moment log views (§5.2), Record mode (§10), REPLAY badge. The §1 principle, touchable.
5. **Light.** The §8 clock-keyed lighting across the day, including the night beat.
6. **Gate showing.** The §5.3 criteria, run with Ant on Day 1 — and re-run on the first full lived day when the record holds one.

**Effort note (honest, no invented hours).** This is staged work over days, not one evening: the map, the bundle format, and the player are three separate pieces, each with its own failure modes, and the traceability rule (§1) forbids the shortcuts (invented idle motion, generated lines) that make demo worlds quick to fake. What the staging buys is that every step is watchable and every gate is judged on a working thing — and that the life itself never waits on any of it (§0). Stage 2 is a second build of comparable seriousness; Stage 3 is not estimated here at all, by design (§7).

## §12 — Out of scope, and the future hook

- **Visitors (Phase 2, Middle — RULED placement, design overview).** Human-operated visitor NPCs are **out of scope** for this event: no visitor figures, no visitor camera, no session support in any stage. The hook is noted, not built: when visitors unlock, a visitor's logged actions are `human-operated` entries (spec §7.1) in the same vocabulary, so a future client view can render them under the same §1 rules without re-scoping the contracts. Nothing in Stages 1–2 may pre-build visitor affordances beyond keeping that door unlocked in the data.
- **Editing the world from the client** — out, permanently (§1 rule 4).
- **Mobile** — the client is desktop-first (event as triggered). A watch-only phone view is a later question, not a Stage 1–2 deliverable.
- **3D** — out until §7's criteria are met and Ant rules.
- **New world systems** — the client creates no weather, no new places, no new NPC behavior. If watching the town reveals a want of that kind, it goes to the design/spec process, not into the renderer.

## §13 — Open items and TKTK register

| # | Item | Where | Status |
|---|---|---|---|
| W1 | Rendering technology for the 2D client — canvas, SVG, or a 2D library (e.g., Pixi) | §5 | Open — build-time choice; the contracts (§9) are deliberately renderer-blind |
| W2 | Hosting — where the client and its state/bundle files live | §2 | Open — tied to the Habitat landing decision, which is Ant's and still open; architecture does not depend on the answer |
| W3 | Sound sourcing — recorded vs synthesized ambience, licensing, or silent-until-ruled | §8 | Open — no stage gate requires sound |
| W4 | Town beyond the square in v1 — neighborhood + town center only, or frontier/coast/hills drawn from the start | §3 | Open — map structure must allow the frontier; drawing it in v1 is Ant's call |
| W5 | Replay default speed — §5.2 proposes 1 world hour = 15 real seconds | §5.2 | Open — Ant settles it after watching the Day 1 replay, not before |
| W6 | Drive numbers in Record mode — §6.1 keeps numbers in the dataset layer; whether the client's Record mode may surface D4 numbers (Ant-side only; residents never see them either way) | §6.1 | Open — proposed: dataset views only, never on a figure |
| W7 | Cat figures' appearance — rendered from residents' recorded descriptions when those exist; generic until then | §4 | Open — the record leads, the renderer follows |
| — | *2D vs 3D* | §0, §7 | **Closed — RULED by Ant, Nov 14, 2026: 2D first. 3D is a later gate (§7), not the plan of record.** |
| — | *Visitor integration* | §12 | **Closed for this event — out of scope by the design's Phase 2 placement; future hook noted, nothing built.** |

-30-

Sources: *The Habitat — Full Design, Phased (PROPOSED)*, 2026-11-14, `~/workspace/user/files/habitat-phase-1-design-PROPOSED-2026-11-14.md`; *The Habitat — Phase 1 Rollout Specification (PROPOSED)*, 2026-11-14, `~/workspace/goals/the-habitat-four-autonomous-residents/files/Habitat-Phase-1-Rollout-Spec-PROPOSED-2026-11-14.md` (§1 world clock and ticks, §3.2 houses, §7 instrumentation and event-log vocabulary, §8 NPCs, §9 fountain and cats); the concept website mock (town square page — enterable house interiors, standing resident figures drawn from the mirror records, diary shelf, record room), built Nov 14, 2026; conversation, main chat, Sat Nov 14, 2026 — Ant's World Client event trigger (3:47 PM ET) and his 2D-first ruling the same day. All architecture, contracts, speeds, and build order in this scope are Bill's PROPOSED detail under Ant's event; no Pino quotes appear in this document, by the same rule as the spec (§9.4 of the spec).
