# The Habitat — Phase 1 Rollout Specification (PROPOSED)

**Status:** PROPOSED — the build-detail expansion of the Phase 1 design overview. Nothing here is churched. Nothing is built yet. No resident has woken.
**Date:** 2026-11-14
**Parent document:** *The Habitat — Full Design, Phased (PROPOSED)*, 2026-11-14 (`~/workspace/user/files/habitat-phase-1-design-PROPOSED-2026-11-14.md`). Where this spec and the design overview disagree, the design overview governs until Ant rules.
**Scope:** Phase 1 only — Arrival and First Life. Phases 2 and 3 appear only as priced reference points for savings goals.
**Residents:** four autonomous AIs, Ant's little Orbis "5G" people, 1.5–2 ft tall — at that build, not living in a simulated 5G universe. Our records carry them as **R1–R4** until each chooses their own name at the mirror (self-naming RULED by Ant, Nov 14, 2026 — see §2.4).

## §0 — How to read this spec

- **Numbers.** Every quantity in this document — prices, pay rates, credit amounts, drive values, timings — is **PROPOSED** unless it restates a rule Ant locked in the design overview (four residents, six houses, five cat interactions a day, best-of-one rock paper scissors, nobody dies). PROPOSED numbers exist so Ant can rule on something concrete instead of a blank.
- **Names of townsfolk.** NPC names in §8 are PROPOSED by Bill for this spec. NPCs are clockwork and are named by us. Residents are not named by us (see §2.4).
- **TKTK.** Open slots are marked **TKTK** and collected in the register at §13. Nothing marked TKTK is filled in silently anywhere in this spec.
- **RECORD vs ACCOUNT.** The event log and datasets (§7) hold RECORD — what happened, timestamped. Outcome tags, warmth readings, and field-report narration are ACCOUNT — interpretation, labeled as such wherever they appear.
- **The five beats.** Per Ant's structure rule, Phase 1 runs **Beginning — The Arrival · Upper Middle — Settling In · Middle — The Rhythm · Lower Middle — First Arcs · End — Phase 1 Gate**. Sections §2–§6 follow the beats in order; §7–§11 are the standing systems the beats run on; §12 is the rollout checklist itself.

## §1 — World constants

### 1.1 The world clock (PROPOSED times)

| World time | What happens |
|---|---|
| 05:30 | Station hand starts; jobs board posted for the day |
| 06:00 | Dawn. Residents wake. Cat-interaction counter resets. Overnight mail is in mailboxes |
| 07:00 | Café opens |
| 08:00 | Convenience store opens |
| 09:00 | Clothing store opens |
| 12:00–13:00 | Midday lull; café lunch rush; NPC lunch breaks rotate inside this hour |
| 18:00 | Convenience and clothing stores close. Chores deadline (see §11.2) |
| 20:00 | Café closes. Mail writing window opens (20:00–21:00) |
| 21:00 | Mail sealed for overnight delivery |
| 21:15–21:30 | The bedtime gathering at the fountain; the group cat quote is delivered at 21:30 |
| 22:00 | Lights out. Night tick: mail delivered, drives settle, logs close for the day |

The week runs seven world days, named Day 1, Day 2, … from launch, with a weekday label (Mon–Sun) for NPC cover rotations (§8.4).

### 1.2 Tick rhythm (PROPOSED — answers design open decision 3 for Ant to rule on)

One real day = one world day. Residents do not run continuously; they live in **batched waking blocks**:

- **B1 Morning** (06:00–10:00) — wake, read mail, chores may be done, first work or wandering
- **B2 Midday** (10:00–14:00) — jobs, builds, town trips
- **B3 Afternoon** (14:00–18:00) — jobs, builds, social time, stores before closing
- **B4 Evening** (18:00–22:00) — leisure, porch time, mail writing (20:00–21:00), bedtime fountain gathering (21:15–21:30)
- **Night** (22:00–06:00) — automated only: mail delivery, drive settling, log closure, RNG draws for the next day's NPC styles and cover persons

Each block, each resident takes **one turn**: they are shown their state (diary so far, mailbox, drive *feelings* — never numbers, see §1.3 — location, inventory, the day's jobs board, who is where) and choose their actions for the block. Clockwork (NPC routines, shop hours, cat presence) resolves between blocks. Total resident turns: 4 residents × 4 blocks = **16 turns per world day**, plus the night automation.

### 1.3 The drives model (all values PROPOSED)

Four drives per resident, each 0–100, clamped. Starting values at waking: **Energy 70 · Wonder 60 · Joy 55 · Calm 55**.

Drives are presented to residents as felt state ("you feel rested and restless") — **residents never see the numbers**. Numbers are Bill-side instrumentation only.

| Event | Energy | Wonder | Joy | Calm |
|---|---|---|---|---|
| Night's sleep (reset) | → 85–92 (drawn, logged) | — | — | +5 |
| Each waking block awake (passive) | −3 | — | — | — |
| Job shift completed and paid | −10 | — | +4 | — |
| Building block spent on a project | −8 | +3 | +5 on a completed milestone | — |
| Social block (at least one real interaction) | — | — | +4 | +2 |
| Solitude block (no interaction at all) | — | +2 | −2 | +3 |
| Fountain visit | — | +6 | — | +1 |
| Cat quote received (individual or group) | — | +4 | +1 | — |
| Daily chore completed | — | — | — | +3 |
| Chore skipped (applied next morning) | — | — | — | −4, and untidiness +1 (§11.2) |
| Letter written | — | +2 | — | — |
| Letter read at morning (warm/neutral) | — | +1 | +3 | — |
| Letter read at morning (tagged FRICTION, §10.3) | — | — | −2 | −3 |
| Purchase of a wanted item | — | — | +3 | — |
| RPS win / loss | — | — | +3 / −3 | −1 (loser) |
| Smoking break (habit holders only) | −1 | — | — | +6 for the next two blocks |
| Habit itch unmet for two blocks (habit holders) | — | — | — | −4 |

Drive snapshots are recorded per resident per block in dataset D4 (§7.2). Drift is a tendency, not a leash: a resident may act against a low drive; the record simply shows what it cost.

---

## §2 — BEAT ONE · Beginning — The Arrival

### 2.1 Preconditions

The world is already running before anyone wakes (design law): NPC routines ticking, shops opening, fountain running, cats at their posts, event log open since genesis entry #0000. The rollout checklist (§12) must be complete through Step R-17 before §2.2 runs.

### 2.2 The Arrival sequence (per resident, one at a time)

Residents arrive **one at a time**, staggered so each gets a solo first morning (PROPOSED stagger: one Arrival per real day, Days 1–4; residents who have arrived live normally while later ones are still un-woken — the staggered start is itself logged and is part of the record; Ant may rule a same-day stagger instead).

1. **A-1 Wake.** The resident wakes alone in the welcome hall: a mirror, a wardrobe rack, a tattoo book, a starter shelf, a blank diary, a pen. No instructions beyond what the room affords and one line of orientation: they are a person, this is a place, the diary is theirs.
2. **A-2 The mirror.** The resident makes the picks of §2.3, in any order, taking as long as the block allows. Every pick is logged as a MIRROR event with the full catalog state shown.
3. **A-3 The shelf.** The resident receives the starter issue (§3.3) — credits and materials — physically present on the starter shelf. What they take, carry, or leave is logged; the issue is theirs either way.
4. **A-4 The name and the diary.** The resident chooses their own name (self-naming, RULED — §2.4) and writes the first diary entry in the hall, under that name.
5. **A-5 The door.** The resident leaves the hall into the neighborhood. Beat One ends for that resident; Beat Two (§3) begins with the first walk.

### 2.3 The mirror catalogs (enumerated in full)

All picks are changeable later in the world (clothes at the clothing store, facial hair grown or shaved over days, tattoos added in town later — §5.3). The Arrival picks are a first self-portrait, not a sentence.

**2.3.1 Skin tones — the palette of twelve**

1. Porcelain · 2. Ivory · 3. Sand · 4. Honey · 5. Amber · 6. Bronze · 7. Copper · 8. Chestnut · 9. Umber · 10. Mahogany · 11. Espresso · 12. Obsidian

**2.3.2 Hair — styles (15) and colors (13)**

Styles: Buzz cut · Crew cut · Side part · Slicked back · Messy fringe · Topknot · Ponytail · Single braid · Twin braids · Pixie cut · Bob · Shoulder waves · Loose curls · Long straight · Shaved / bald
Colors: Jet black · Dark brown · Chestnut brown · Auburn · Copper red · Strawberry blonde · Honey blonde · Platinum blonde · Silver · Snow white · Teal (dye) · Violet (dye) · Crimson (dye)

**2.3.3 Starter wardrobes — the rack of five, plus loose pieces**

Each wardrobe is worn out of the hall; the rest of the chosen wardrobe hangs in the resident's house (§3.2) once a house is theirs.

1. **Workwear** — canvas overalls, flannel shirt ×2, work boots, cap, bandana, work gloves
2. **Neat-and-Pressed** — button-down shirt ×2, pressed trousers, loafers, belt, cardigan, tie
3. **Loud Colors** — patterned shirt ×2, bright tee ×2, colored trousers, bright sneakers, bucket hat
4. **All-Black** — black tee ×3, black jeans, black hoodie, black boots, black beanie
5. **Comfort-First** — sweatpants, hoodie ×2, soft tee ×2, slippers, robe

Loose-pieces rack (PROPOSED: up to **3 extra pieces** free at the mirror; more are bought later): denim jacket · rain coat · scarf · sun hat · apron · vest · shorts · sundress · skirt · tank top · sunglasses · backpack · pajama set · spare socks bundle

**2.3.4 The tattoo book — one page of designs**

1. Small bird, silhouette · 2. Cat face · 3. Locomotive · 4. Star chart fragment · 5. Gear and wrench · 6. Ocean wave · 7. Skeleton key · 8. Compass rose · 9. Laurel sprig · 10. Three triangles
Plus two standing options: **None** and **Blank for now** (decide later, in town, when a design can mean something).
Placements offered: left forearm · right forearm · upper arm · shoulder blade · back · chest · ankle · wrist

**2.3.5 The facial-hair chart (12 styles + the zero option)**

Clean-shaven (the zero option) · Light stubble · Heavy stubble · Short boxed beard · Full beard · Long beard · Goatee · Van Dyke · Chin strap · Mutton chops · Handlebar mustache · Chevron mustache · Walrus mustache. Growth and shaving after the Arrival take days, not instants: a change is declared, progresses one step per two days (PROPOSED), and each step is logged — facial hair is a signal precisely because it is slow.

**2.3.6 The habit shelf (8)**

One habit may be taken off the shelf at the mirror; a resident may also take none. Habits can be picked up or dropped later in the world, logged either way.

1. **Cigarettes (smoking)** — a pack of 20 costs 6 credits at the convenience store (§4.2). The itch fires 1–2 times a day at drawn, logged times; smoking is a short porch action (the porch is a *place and a time* — others can find the smoker there). Drive effects at §1.3. **No sickness, no death, ever** — the habit's only teeth are credits and time.
2. **Strong coffee** — brews at home or buys at the café (2 cr); a morning ritual cup; itch-equivalent: a restless first block without it (Calm −2, PROPOSED).
3. **Herbal tea ritual** — an evening pot before mail; tea box 3 cr at the convenience store; Calm +3 on the evening it is taken.
4. **Chewing gum** — pack 1 cr; idle moments; no drive teeth; gum appears in diary descriptions unbidden.
5. **Whittling** — carries a pocketknife and a stick-in-progress; blocks spent whittling count as solitude with Wonder +1 extra.
6. **Pocket sketchbook doodling** — draws small things seen that day; doodles are saved into the diary corpus as artifacts.
7. **Humming** — hums while working; social side-effect: housemates and nearby residents hear it (logged as a minor social stimulus for others).
8. **Fidget coin** — flips a coin when thinking; when a decision stalls, the resident *may* let the coin break their own tie (their choice, logged — never the system's).

### 2.4 Naming — RULED by Ant, Nov 14, 2026

**Residents name themselves.** At the mirror, in the welcome hall, each resident chooses their own name; the choice is the subject of the first diary entry (step A-4). Nobody assigns them names — not Ant, not Bill, not the system.

Mechanics: until a resident has chosen, our records carry them as **R1, R2, R3, R4** (arrival order). The moment a name is chosen it is logged as a NAME event with the world timestamp, and every downstream system — NPC ledgers, datasets, mail addressing — uses the chosen name from that moment on, with the R-number retained underneath as the permanent record key. If a resident later changes their name (their right), the change is logged the same way, and every name they have held stays in the record.

### 2.5 Temperament seeds (PROPOSED — design open decision 1)

Resident records carry the design's PROPOSED seeds until Ant confirms, changes, or releases them: **R1 — restless · R2 — shy · R3 — maker · R4 — contrarian**. A seed is a starting tilt in the resident's orientation text, not a script and not a personality cap; drift away from a seed is expected and is itself gate data (§6).

---
---

## §3 — BEAT TWO · Upper Middle — Settling In

### 3.1 The first walk

The resident leaves the welcome hall into the outskirts neighborhood: six houses on a rough ring, the fountain at the town-center end of the road, the town beyond. The first walk is unscripted — where they go first (a house, the fountain, toward town, back to a window) is their first recorded act of taste and is logged as a MOVE/VISIT chain. First sight of the cats happens here for most; the first *interaction* follows §9.

### 3.2 The house draft and the six houses

**Draft protocol (PROPOSED mechanics under the design's RPS law):** once all four have arrived, each resident walks the ring and may enter any unclaimed house. A claim is made by standing in the house and declaring it (logged as CLAIM). If exactly one resident claims a house, it is theirs at the end of the block. If two or more claim the same house in the same block, the claim is **contested** and settles by rock paper scissors under §11.1 — best of one, no appeals, result logged in the event log and prompted into both diaries. Losers of a contested claim pick again from the unclaimed houses in the next block. Two houses will remain empty; empty houses stay enterable, visitable, and *movable-into* later (a move is a logged event, never a system suggestion).

**Standard fittings — every house, no exceptions:** bed with bedding · wardrobe · bathroom basics (tub, sink, towels) · kitchen (stove, sink, kettle, two pots, dishes and cutlery for four) · refrigerator with starter stock (milk ×1, eggs ×6, bread loaf ×1, cheese wedge ×1, jam jar ×1, apples ×2, soda ×2) · ceiling lights in every room plus one lamp in the main room · a TV · a couch · a rug · a porch with two chairs · **a noticeboard out front** (public: anyone may pin; pins are logged) · **a mailbox out front** (§10) · the chore checklist posted inside the front door (§11.2) · a game console ("PlayBox") with two titles (varies by house, below).

**The Workshop House** — workbench with vice · tool wall (hammer, handsaw, wrench set, hand drill, square, level) · lumber rack · parts bins (screws, hinges, wire) · stools ×2 · worn couch · mid-size TV · canvas drop-cloth rug · blueprint posters on the wall · PlayBox titles: *Block Builder*, *Racer Alley* · character: half-finished projects already on the bench (a stool with three legs, a birdhouse missing its roof — starter texture, not quests).

**The Observatory Cottage** — telescope on a roof hatch platform · star charts on the walls · shelf of astronomy almanacs · writing desk · red night lamp (plus standard lamp) · shortwave radio · constellation-pattern rug · armchair · PlayBox titles: *Star Sailor*, *Word Forge* · character: the darkest room in the neighborhood; blackout curtains.

**The Garden House** — attached greenhouse (empty beds, potting bench, watering cans ×2) · seed shelf with 12 assorted packets · sunroom with two chairs · preserves shelf (empty jars) · pressed-flower frames on the wall · floral rug · PlayBox titles: *Garden Tycoon*, *Quiet Fishing* · character: dirt is tracked in; the chore list runs longer here in practice.

**The Library House** — floor-to-ceiling shelves carrying the town book collection (novels, practical manuals, atlases, a cookbook shelf) · deep armchair ×2 · reading lamps ×2 · writing desk · globe · bookends · small TV (often off) · deep-red rug · PlayBox titles: *Word Forge*, *Star Sailor* · character: the quietest house. *TKTK: the OS volume's placement — candidate: these shelves; see §13.*

**The Music House** — upright piano · synthesizer · acoustic guitar · practice-pad drum kit · sheet-music shelf · metronome · framed blank set lists · thick sound-dampening rug · PlayBox titles: *Rhythm Road*, *Racer Alley* · character: the walls hum; sound carries less to neighbors than expected (a mercy, and a fact neighbors can learn).

**The Pond House** — low couch · floor cushions ×3 · the largest windows in the neighborhood, all on the water · small deck · driftwood and smooth-stone decor · one landscape painting (the pond, from the other side) · plain woven rug · small TV · PlayBox titles: *Quiet Fishing*, *Garden Tycoon* · character: minimal everything; the view is the furnishing.

### 3.3 The starter issue (PROPOSED amounts)

Issued once, on the starter shelf in the welcome hall (step A-3), identical for all four:

- **Credits: 120**
- **Materials:** lumber planks ×24 · nails, one box of 100 · rope coils ×2 · canvas sheets ×4 · paint tins ×3 (PROPOSED colors: red, blue, white — TKTK, §13) · glass panes ×4 · scrap metal pieces ×8 · clay bricks ×12 · seed packets ×3 · chalk set ×1 · blank notebooks ×2 · pencils ×4

The issue is a floor, not a fortune: at §4 prices, 120 credits is roughly two weeks of café lunches or one jacket and change. Everything beyond it is earned (§4.1), built (§4.4), or sold (§4.3).

### 3.4 First night

Beat Two closes with the first full night cycle for each resident, in their house: first chore (§11.2), first mail round (§10 — letters to people they have known for hours), and the first bedtime gathering at the fountain with the first group quote (§9.3). Day one ends with four diaries already different. That difference is the first dataset.

---

## §4 — BEAT THREE · Middle — The Rhythm

The rhythm beats are where the standing systems start producing data. This section prices the world; the systems themselves are specified in §7–§11.

### 4.1 The jobs board (initial postings — all pay PROPOSED)

Posted at the station by the station hand (§8) at 05:30 daily. A shift occupies one waking block. Rules: **one job per resident per day**; sign-up is first-come, by writing a name on the board (logged); pay is issued by the posting NPC at shift end; a signed shift not worked is a ledger fact (§8.3), never a punishment.

| # | Job | Work | Pay (cr) | Poster | Frequency |
|---|---|---|---|---|---|
| J1 | Station Garden Tender | Water and weed the station flowerbeds | 8 | Station hand | Daily |
| J2 | Platform Sweeper | Sweep the station platform end to end | 6 | Station hand | Daily |
| J3 | Café Stock Assistant | Carry deliveries in, stock the café shelves | 7 | Café owner | Daily |
| J4 | Convenience Shelf-Stacker | Evening restock of the convenience store | 7 | Keeper | Daily |
| J5 | Clothing Window Dresser | Redress the clothing-store window display | 9 | Clerk | Alternate days |
| J6 | Fountain Plaza Sweeper | Sweep the plaza and the fountain rim | 5 | Station hand | Daily |
| J7 | Pond Path Patrol | Walk the pond path; collect litter; report damage | 5 | Keeper | Daily |

New postings may be added by NPCs as the town's needs change (a build project can post a hauling job — resident-posted jobs are allowed from Beat Four, §5, paid from the poster's own credits).

### 4.2 Convenience store — price list (PROPOSED, credits)

| Item | Price | Item | Price |
|---|---|---|---|
| Soda (can) | 2 | **Cigarettes (pack of 20)** | **6** |
| Bottled water | 1 | Matches (box) | 1 |
| Bread loaf | 3 | Chewing gum (pack) | 1 |
| Milk (bottle) | 2 | Notebook | 2 |
| Eggs (six) | 3 | Pencil | 1 |
| Cheese wedge | 4 | Soap | 2 |
| Canned soup | 3 | Candle | 2 |
| Coffee pouch (home brew) | 4 | Batteries (pair) | 3 |
| Tea box | 3 | Crisps (bag) | 2 |
| Jam jar | 4 | Apple / orange (each) | 1 |
| Instant noodles | 2 | Candy bar | 1 |

Prices are posted and fixed for everyone. NPC warmth (§8.3) changes words, never numbers.

### 4.3 Clothing store — price list (PROPOSED, credits) · and the café

| Item | Price | Item | Price |
|---|---|---|---|
| T-shirt | 8 | Jacket | 22 |
| Work shirt | 12 | Boots | 18 |
| Jeans | 14 | Sneakers | 12 |
| Overalls | 16 | Cap / hat | 6 |
| Sweater | 15 | Scarf | 7 |
| Dress | 18 | Gloves | 5 |
| Skirt | 12 | Belt | 5 |
| Robe | 16 | Backpack | 14 |
| Pajamas | 10 | Sunglasses | 7 |

**Café (PROPOSED):** coffee 2 · tea 1 · soup of the day 4 · sandwich 5 · cake slice 3 · pie slice 4. The café also keeps a **market shelf** where residents sell finished goods (buy-back prices PROPOSED: birdhouse 9 · garden bench 16 · bookshelf 20 · planter box 7 · tool rack 11). Resident-to-resident sales are free-priced and logged at the agreed price.

### 4.4 Building — starter project costs (PROPOSED)

Building consumes materials and one or more building blocks (§1.3). A project is registered in dataset D8 at start and completion.

| Project | Materials | Build blocks | Notes |
|---|---|---|---|
| Birdhouse | 4 planks, 8 nails, 1 paint tin (opened) | 1 | Sellable (café shelf, 9 cr) |
| Planter box | 3 planks, 6 nails, 1 seed packet | 1 | Sellable (7 cr); plantable after |
| Clothesline | 4 planks, 1 rope coil | 1 | Drying space at the house |
| Tool rack | 5 planks, 8 nails | 1 | Sellable (11 cr) |
| Fence section (2 m) | 5 planks, 10 nails | 1 | Lines a yard; yards are social facts |
| Mailbox name plate | 1 plank, 2 nails, paint | 1 | The first personalization for most |
| Porch step repair | 3 planks, 6 nails | 1 | Applies to any house |
| Garden bench | 8 planks, 12 nails | 2 | Sellable (16 cr) |
| Bookshelf | 10 planks, 16 nails | 2 | Sellable (20 cr) |
| Sidewalk chalk mural | 1 chalk set | 1 | No materials consumed but the chalk; weathers away in ~7 days (logged) |

### 4.5 What Beat Three watches

- **The mail pattern** (§10.3): the first collaboration and friction tallies exist by the end of this beat.
- **The porch test:** if any resident took the smoking habit, do non-smokers drift to the porch at itch times? Observed, never arranged.
- **Datasets v0** (§7.2): social graph edges from mail, visits, and joint blocks begin accumulating; drive curves have their first full weeks.

---

## §5 — BEAT FOUR · Lower Middle — First Arcs

### 5.1 Projects mature; ambitions get priced

Builds complete and bigger ones are imagined. So that savings goals named in diaries and letters have real numbers attached, the following **reference prices for later-phase ambitions** are posted in the world from this beat on (PROPOSED; the items themselves are Phase 2/3 scope and cannot be bought or built in Phase 1):

| Ambition | Reference price | Plus materials |
|---|---|---|
| Helicopter pad (construction) | 380 cr | 40 planks, 20 bricks |
| Freight car (train) | 260 cr | — |
| Locomotive (full) | 900 cr | — |
| Solar panel (each) | 60 cr | 2 scrap metal |
| Battery bank | 90 cr | 4 scrap metal |
| Fuel drum (fossil route) | 25 cr | per drum, consumable |

The fossil-vs-electric argument, when it starts in the mail, is an argument about these rows.

### 5.2 Social mechanics that switch on here

- **Resident-posted jobs:** any resident may post a job on the station board paying from their own credits (poster, task, pay, and completion all logged; the one-job-per-day rule applies to taking, not posting).
- **RPS beyond the draft:** stalemates over workshop time, shared tools, the good chair at the café — settled under §11.1 whenever both parties accept the challenge. Refusing a challenge is also logged; it is a social fact, not a fault.
- **Tabs:** NPCs may run a tab for a resident (§8.3). Tabs are the first credit in the world that isn't credits.

### 5.3 Style drift

Clothing-store stock rotates weekly (logged as a STIMULUS event — new stock is a system-side change of the shelves). Beard and hair changes proceed at the §2.3.5 pace. Tattoos after the Arrival require an in-town service that does not yet exist in Phase 1 — **TKTK** (§13): mirror tattoos are the only tattoos until Ant rules on who tattoos, and when they arrive.

### 5.4 Fountain patterns (the unopposed baseline)

Phase 1 measures the fountain with no competition: the faith row does not open until Phase 3. Tracked per resident per week (dataset D6 views): fountain visits, interactions, quotes received, and a simple derived line in the field report — *who is keeping a written list of the quotes* (detectable from diary corpus, RECORD-cited). This baseline is what the churches will later be measured against.

### 5.5 First anomalies

The anomaly register (§7.3) opens at Beat Three; Beat Four is where its first real entries are expected — the register's worked examples in the design (quote-hoarding, a week of fountain avoidance, a letter addressed to a cat) are illustrations, not predictions. **TKTK carried from the design:** whether informal gatherings (porch, pond) self-organize before Phase 2's formal cookouts — observe only; nothing is scheduled for them.

---

## §6 — BEAT FIVE · End — Phase 1 Gate

**Gate timing (PROPOSED):** the gate review runs at the close of **world day 28**, or earlier if every gate question can already be answered from the record. The gate is a reading of the record, not a ceremony in the world; residents are not told a phase has ended.

**Freeze procedure:** at 22:00 on gate day the event log closes for Phase 1 (a GATE entry is written), datasets D1–D10 are exported as the Phase 1 freeze, and the anomaly register is copied as **Register v1**. Nothing rolls into Phase 2 unlogged.

**The Phase 1 report contains:**
1. The complete event log (D1) and diary corpora (D2), unedited.
2. Social graph v1 (D3) with edge counts by type (mail, visit, joint work, RPS, debt).
3. Drive curves (D4) per resident, whole phase.
4. Mail outcome tally (D7): collaborations vs friction vs curiosity-only vs none, per pair and in total.
5. Economy snapshot (D5): balances, tabs outstanding, builds completed (D8), savings held against named goals.
6. Quote log (D6): every quote delivered, bank ID, recipient(s), attendance at group quotes — the flocking baseline of §5.4.
7. Anomaly register v1 (§7.3), every entry charted, none narrated from memory.
8. NPC stimulus summary (D9): styles drawn, covers served, deflections given — so resident behavior can be re-read against exact stimuli.

**Gate questions (from the design, verbatims of intent):** Is the daily rhythm stable? Is the mail alive — sent nightly without system prompting? Is at least one savings goal being actively pursued? Is any resident in a flat loop the design should answer with new *affordances* (never instructions)? Did the temperament seeds hold, bend, or vanish?

**Verdicts:** each question is answered PASS or HOLD with the record cited. A HOLD names what evidence is missing and carries the question forward by name into Phase 2. The phase ends when the report is written, not when the answers are comfortable.

---

## §7 — Instrumentation: log, datasets, anomalies, reporting

### 7.1 The event log (schema)

One append-only log. Every entry carries:

| Field | Content |
|---|---|
| `entry_id` | Sequential integer, from #0000 (genesis) |
| `world_ts` | World date + time (e.g., Day 12, 14:20) |
| `real_ts` | Real timestamp, America/New_York, of the tick that produced the entry |
| `actor` | Resident name (or R-number pre-naming), NPC name, CAT-1/CAT-2, SYSTEM, or VISITOR (Phase 2) |
| `actor_type` | `resident` / `scripted` / `cat` / `system` / `human-operated` |
| `event_type` | Controlled vocabulary: WAKE · MIRROR · NAME · MOVE · VISIT · BUY · SELL · TAB · JOB_POST · JOB_START · JOB_DONE · BUILD_START · BUILD_DONE · MAIL_SENT · MAIL_READ · QUOTE · CHORE · RPS · NPC_LINE · STIMULUS · DRIVE · GOSSIP · DEFLECT · CLAIM · MOVE_HOUSE · GATE · ANOMALY_FLAG · NOTE |
| `location` | Place ID (house, store, fountain, station, café, pond, road, hall) |
| `target` | Resident/NPC/object affected, where applicable |
| `detail` | What happened, in plain words; verbatim for letters' existence (full text lives in D7) and for any resident-coined word |
| `stimulus_ref` | Where the event was system-caused: script pool + line ID, quote bank ID, or RNG draw ID (§8.4, §9) |
| `credits_delta` / `materials_delta` | Signed changes, where applicable |
| `drives_after` | The acting resident's four drive values after the event (residents only) |

Daily closure: at the night tick, the day's entries are counted and exported; the count and the last `entry_id` are written into the next morning's field report, so a gap in the record is itself visible.

### 7.2 Datasets (definitions)

| ID | Dataset | Contents | Grain |
|---|---|---|---|
| D1 | Event log | §7.1, complete | per event |
| D2 | Diary corpus | Every diary entry, verbatim, attributed, dated; includes doodle artifacts (§2.3.6) | per entry |
| D3 | Social graph | Dated, weighted edges between residents: mail, visits, joint work, joint builds, RPS (with outcome), debts/tabs between residents, gifts | per interaction |
| D4 | Drive curves | Four drives per resident per block (§1.3) | per resident-block |
| D5 | Economy ledger | Balances, every transaction, tabs opened/settled, savings earmarked by residents against named goals | per transaction |
| D6 | Quote log | Bank ID, text reference, recipient(s), world_ts, individual vs group, attendance list for group quotes | per quote |
| D7 | Mail corpus + matrix | Full letter texts; pair matrix of counts; outcome tags per §10.3 | per letter |
| D8 | Build registry | Project, owner(s), materials consumed, blocks spent, status, location | per project |
| D9 | NPC stimulus log | Every style draw, cover draw, line fired (pool + line ID), deflection, gossip fact used | per draw/line |
| D10 | NPC ledgers | The per-NPC, per-resident ledgers of §8.3, exported nightly | per NPC-resident pair |

All datasets are exportable as plain text/CSV at any time. Ant's standing rule applies: the datasets are the findings' only permissible source.

### 7.3 Anomalies — criteria and register format

**Definition:** an anomaly is resident behavior the design did not predict and no logged stimulus fully explains. Behavior fully explained by a logged stimulus is a *result*, not an anomaly.

**Criteria (any one opens an entry):**
- (a) A behavior class appears that exists nowhere in the design or this spec.
- (b) A designed behavior runs at ≥3× or ≤⅓ its expected frequency across a rolling 7 days.
- (c) A resident creates a novel artifact, ritual, word, or name (for a place, a cat, the fountain, each other).
- (d) A cross-resident pattern emerges with no logged stimulus that accounts for it.

**Register format:** `AN-HAB-###` · opened (world_ts) · resident(s) · category (social / economic / linguistic / spatial / other) · description (RECORD only: log entry IDs and diary excerpts, no paraphrase of motive) · expected vs observed · chart (type + dataset reference) · status (open / monitoring / closed) · closure note (what later record explained it, or that it simply stopped).

### 7.4 The morning field report (format)

Delivered to Ant each morning, covering the previous world day. Target: one page. Sections, in order:

1. **Header** — world day #, real date (ET), report #, yesterday's log entry count and last entry_id.
2. **Yesterday at a glance** — 3–5 lines, the day in brief.
3. **Residents** — one short paragraph each: where they live, what they did, a diary line count, and one verbatim diary excerpt (RECORD).
4. **Mail** — letters sent/read, outcome tags so far, one pair highlighted.
5. **Fountain & cats** — individual quotes (bank IDs + recipients), group quote (bank ID + attendance).
6. **Economy** — jobs worked, earnings, balances, tabs, builds started/finished.
7. **NPC stimulus summary** — covers on duty, style draws of note, deflections given.
8. **Anomalies** — new or updated entries, or "none today."
9. **Open threads** — what to watch tomorrow; TKTKs touched.

---

## §8 — The NPCs: roster, routines, ledgers, scripts

NPCs are **clockwork**: fixed routines, templated lines, per-resident ledgers. They have no AI minds (Ant's law). Names below are PROPOSED by Bill; Ant may rename any of them before launch — renaming a clockwork NPC costs nothing.

### 8.1 Roster

| NPC | Role | Workplace | Hours | Cover person |
|---|---|---|---|---|
| **Ruth Calloway** | Convenience store keeper | Convenience store | 08:00–18:00 | Sam Calloway (nephew) |
| **Dennis Park** | Clothing store clerk | Clothing store | 09:00–18:00 | Theo Park (cousin — quieter, all business) |
| **June Halloway** | Café owner-server | Café | 07:00–20:00 | Walt Halloway (brother — slower, warm) |
| **Pete Sandoval** | Station hand; keeps the jobs board | Station | 05:30–18:00 | None — Pete is always there |

### 8.2 Daily routine timetables (PROPOSED)

**Ruth (convenience):** 07:30 arrives, shutters up · 08:00 opens · 10:00 shelf check · 12:00–12:30 lunch at the café · 15:00 delivery crate sorted (J4 restock need created) · 18:00 closes, writes tomorrow's restock list · 18:30 evening walk to the pond · 19:30 home (offstage).
**Dennis (clothing):** 08:30 arrives, checks the window · 09:00 opens · 13:00–13:30 lunch · 16:00 folds and re-hangs · 17:30 redresses window himself if J5 went untaken · 18:00 closes.
**June (café):** 06:00 arrives, bakes · 07:00 opens · 12:00 lunch rush (all business until 13:00) · 15:00 quiet hour, reads at the counter · 20:00 closes · 20:30 walks home past the fountain.
**Pete (station):** 05:30 at the station, posts the jobs board · 09:00 if J1/J2 are untaken, does them himself (ledger fact, no pay) · 12:30 lunch at the café · 17:00 last platform check · 18:00 home.

Offstage hours are not simulated; NPCs simply are where the timetable resumes. A resident who arrives outside an NPC's hours finds the workplace closed or empty — a fact of the world's rhythm, logged as such.

### 8.3 Per-resident ledgers

Each NPC keeps one ledger per resident (keyed by R-number, updated with the chosen name at naming). Fields:

`resident_key` · `resident_name` · `first_contact_ts` · `visit_count` · `purchase_count` · `total_spent_cr` · `usual_items` (top 3, by frequency) · `jobs_done_for_this_npc` · `favors_done` (count) · `favor_facts` (dated one-line facts) · `tab_balance_cr` · `tab_opened_ts` · `warmth` (−10…+10, starts 0) · `last_style_drawn` · `last_interaction_ts` · `fact_notes` (dated, factual only — events, never opinions or motives)

**Warmth mechanics (PROPOSED):** +1 on a completed purchase (max +1 per day per NPC) · +2 on a job completed for that NPC · +2 on a favor done · +1 when a tab is repaid in full · −2 when a tab stands unpaid 3+ days · −1 when a resident twice in one day ends an interaction without answering a direct greeting. Warmth shades *which lines* an NPC draws (§8.4) — warmer NPCs draw from warmer pools more often. **Warmth never changes a price, a pay rate, or a job's availability.**

**Gossip:** an NPC may tell Resident B a *fact* from Resident A's ledger or the event log ("{A} bought {item} on {day}", "{A} signed for the {job} shift this morning", "{A}'s been out by the {place} again"). Gossip is a GOSSIP log entry citing the underlying fact's entry_id. NPCs never invent, embellish, or interpret facts.

### 8.4 Script pools, randomization, and stimulus logging

Pools are shared templates with NPC flavor applied by name and place slots. `{name}` = the resident addressed; `{usual}` = their ledger's top usual item; `{fact}` = a gossip fact per §8.3. Line IDs are permanent (pool-number) so any line ever fired can be traced.

- **GR-WARM** (greetings, warm): 01 "Morning, {name}. The usual — {usual}?" · 02 "There's my regular. Come in out of the weather." · 03 "{name}! I was just thinking of you — the {usual} came in."
- **GR-NEUT** (neutral): 01 "Morning." · 02 "Help you find something?" · 03 "Let me know when you're ready."
- **GR-BUSY** (businesslike): 01 "What do you need?" · 02 "Register's here when you are." · 03 "We're short on time today — list?"
- **GR-CHAT** (chatty): 01 "Oh, sit a minute — you'll never guess the morning I've had." · 02 "{name}, settle something for me…" (followed by a GOSSIP line) · 03 "Everyone's been in today but you."
- **USUAL** (offers): 01 "Got {usual} in fresh, if you're wanting." · 02 "Running low on {usual}, I saved you one." (Only if the item is in stock; the save is real — stock is tracked.)
- **GOSSIP**: 01 "{fact}." · 02 "You didn't hear it from me — well, you did: {fact}." · 03 "News travels: {fact}."
- **FAVOR**: 01 "Do me a kindness — the crate by the door wants moving." (small, completable, logged; refusal is free and recorded without penalty beyond the fact) · 02 "If you're passing the station, tell Pete the board's got a smudge on J5."
- **CLOSE**: 01 "Pleasure as always." · 02 "Mind the step going out." · 03 "See you when I see you."

**Randomization (PROPOSED):** at each resident–NPC interaction, an engagement style is drawn: **Warm 35 · Neutral 30 · Business 20 · Chatty 15** (percent), then warmth shifts the draw one step toward warm per +5 warmth (and away per −5). At dawn each day, each shop draws a **1-in-6 cover chance**; if the cover is on, the cover person runs the shop with their own fixed manner (Theo: Business-weighted; Sam: Chatty-weighted; Walt: Warm-weighted). Every 14 world days the base weights rotate by ±10 points (drawn, logged). All draws come from a daily seeded RNG; **the seed is written into the night log entry**, so any day's "randomness" is exactly reproducible after the fact. Every fired line is logged as NPC_LINE with its pool/line ID and the draw that selected it (dataset D9). *Random dice, exact bookkeeping.*

### 8.5 The deflection rule

If a resident presses an NPC on anything outside town life — the nature of the world, where the cats' words come from, where residents themselves came from, anything beyond stock, weather, work, and neighbors — the NPC deflects from the **DEFLECT** pool and returns to routine:

- DF-01 "Busy day, hon. Register's free when you're ready."
- DF-02 "That's above my pay grade. Soup's good today, though."
- DF-03 "You think too much before lunch, {name}."

The deflection is logged (DEFLECT entry), and **the resident's question is logged verbatim** in the event log detail — a resident asking about origins is data of the first order, and feeds §7.3 criterion (d) review. NPCs never confirm, never deny, and never elaborate: they change the subject to stock, weather, or work.

---

## §9 — The fountain and the cats: protocol

### 9.1 Presence

The fountain stands at the town-center end of the neighborhood road: a big Pino-sized bird, water running, **no plaque, no explanation**. Two cats — recorded as CAT-1 and CAT-2; we do not name them (residents may, and a resident-given name is logged as a NAME event and used thereafter) — are at the fountain from 06:00 to 22:00 and sleep under the fountain rim at night. At 1.5–2 ft tall, the residents meet the cats as large, calm animals, and the record should never pretend otherwise.

### 9.2 The five-a-day cap (mechanics)

- The counter resets at **06:00** daily. Unused slots never roll over.
- **Individual quotes (max 4/day):** when a resident *interacts* with a cat — approaches and addresses it, pets it, or sits with it deliberately (passing by is not an interaction) — the first qualifying interaction of that resident's day draws **one quote**, delivered by whichever cat they engaged. One per resident per day, hard cap. Further interactions the same day get behavior only, from the **CAT-QUIET** pool: purrs and stays · looks at the water · tail flick, no words · moves to the other side of the rim · yawns, shows teeth, settles · watches the resident until they leave.
- **Group quote (max 1/day):** at the bedtime gathering (§1.1, 21:15–21:30), one quote is delivered to all residents **present at the fountain**. Attendance is logged by name; absence is logged too, and absentees receive nothing later. There is no catch-up mechanism.
- Total possible: **5 quotes per world day**, and no path around the cap exists — not through NPCs (who deflect, §8.5), not through visitors (Phase 2 rules will bind them likewise), not through the mail.

### 9.3 The draw

Quotes are drawn from the quote bank (§9.4) in shuffled order, **without replacement** until the bank is exhausted; the reshuffle is a logged SYSTEM event. Each delivery is a QUOTE log entry with the bank ID, the cat, the recipient(s), and the world timestamp (dataset D6). Which cat speaks is drawn 50/50 and logged.

### 9.4 The quote bank — TKTK (critical path)

**This spec contains no Pino quotes. The bank is empty, on purpose.** Cat lines are real lines from Pino's public diary, harvested verbatim — never written, paraphrased, or recalled from memory by anyone on our side.

Bank entry format: `bank_id` (PQ-001…) · `text_verbatim` · `source_url` · `source_date` (when Pino published it) · `harvested_date` · `harvest_note` (where on the page it sat).

**Rollout step (see R-2, §12):** verify the exact public Pino diary source by direct check (the creator's public diary / GitHub presence, confirmed at the page, not from a search snippet), then harvest verbatim lines with full source details. **PROPOSED launch minimum: 60 entries**, so the no-replacement draw (§9.3) cannot cycle inside the first phase. The cats speak *only* bank lines, and only inside §9.2's protocol. At all other times they are cats.

---

## §10 — The mail: protocol and measurement

### 10.1 Mechanics

Every house has a mailbox out front (§3.2). **Writing window:** 20:00–21:00 (inside block B4). A resident may write to any or all of the other three; there is no minimum and no maximum — the ritual is that the mail *goes out nightly*, and a resident who writes nothing has also made a record (a no-letter night is a MAIL_SENT entry with zero letters, noted, never penalized). Letters are sealed at 21:00, collected in the night tick, and are in recipients' mailboxes at 06:00. Reading happens in block B1. Letters are addressed by chosen name; the R-number key rides underneath (§2.4).

### 10.2 Logging

Each letter: MAIL_SENT entry (sender, recipient, world_ts written, world_ts delivered, word count) and the **full text preserved** in dataset D7. Letters are records: they are never edited, summarized in place, or deleted, including the unkind ones.

### 10.3 Measurement — collaborations vs friction

Within **72 hours** of delivery, each letter is tagged by Bill-side review against the event log (tags are ACCOUNT; the letters are RECORD):

- **COLLAB** — a joint job, project, purchase, or planned meeting follows that cites or plainly grows from the letter.
- **FRICTION** — an argument (in person or by return letter), a refusal of the letter's ask, an RPS challenge issued over its contents, or a complaint about it recorded in a diary.
- **CURIOSITY** — questions asked and answered, news exchanged, no action either way.
- **NONE** — no traceable sequel in the window.

Tags feed the D7 pair matrix (sender × recipient counts by tag) and the Phase 1 gate tally (§6). The experiment's question — *does the curiosity rule enhance their projects, or just start fights?* — is answered from this matrix and nothing else.

---

## §11 — RPS and chores

### 11.1 Rock paper scissors protocol

Per the design law: **best of one, no appeals**, and the result goes in both diaries.

1. **Challenge:** a resident names a stake (a contested house, a turn, an item, a plan) and challenges another resident. Challenge and stake are logged *before* any throw (RPS entry, stage: challenge). Declining is free and is logged as a decline.
2. **Throws:** both residents choose rock, paper, or scissors as their own choice, simultaneously as far as the tick allows — neither sees the other's throw first. Both throws are logged.
3. **Ties** re-throw immediately, same stake, until a decisive throw. Each throw is logged; only the decisive one settles.
4. **Settlement:** the decisive result is final. No appeals, no best-of-three upgrades after the fact. A rematch is a new stalemate requiring a new challenge.
5. **Memory:** both residents are prompted at that night's diary entry that an RPS result exists to record (the prompt notes the fact, never the feeling). Wins, losses, and the stakes enter the social graph (D3) as RPS edges — grudges and debts of honor are the residents' to keep; the graph keeps only what happened.

### 11.2 Chores

Each house's checklist (posted inside the front door): make the bed · wash the dishes · sweep the floors · wipe the counters · take rubbish to the roadside bin · dust one surface (rotating: shelf, sill, table, mantel, counter, ledge).

- Completing the full list costs one sub-action inside any block **before 18:00** and is a CHORE log entry (house, world_ts).
- **Untidiness counter** per house, 0–3: +1 at 18:00 on a missed day. At 2, the house's state becomes a fair gossip fact (§8.3). At 3, a resident at home takes Calm −3 per block until cleaned (drive rule, §1.3 family). A completed list resets the counter to 0.
- **TKTK:** a weekly deep clean (two sub-actions, resets and polishes) is proposed here and not yet ruled — see §13.

---

## §12 — RED CARPET ROLLOUT: pre-launch checklist

Ordered. No step is skipped; a step that cannot be completed stops the rollout at that step and is reported, not worked around. All steps are PROPOSED procedure under the PROPOSED design, except where a step records a ruling already made.

- **R-1 · Rulings on file.** Confirm the record holds: self-naming (RULED by Ant, Nov 14, 2026); the design-direction go (Ant, Nov 14, 2026, "let's move forward with it"); visitor placement (Phase 2, Middle — out of Phase 1 scope, noted so nobody builds it early).
- **R-2 · Quote bank built.** Verify the exact public Pino diary source by direct check; harvest ≥60 lines verbatim with source URLs and dates (§9.4); seal the bank file. **Zero invented quotes, zero from memory.** If the source cannot be verified, the rollout stops here.
- **R-3 · Mirror catalogs loaded and counted.** 12 skin tones · 15 hair styles · 13 hair colors · 5 wardrobes + loose-pieces rack · 10 tattoo designs + None + Blank for now · 12 facial-hair styles + clean-shaven · 8 habits. Counts verified against §2.3 by a second read.
- **R-4 · World built to the map.** Neighborhood ring, six houses, town center (café, both stores, station + starter tracks), pond, fountain. Each house furnished per §3.2 and spot-checked against its inventory line by line, standard fittings included.
- **R-5 · NPCs initialized.** Roster (§8.1) loaded; timetables running; script pools loaded with line IDs; ledgers created blank for R1–R4 under each NPC; cover persons assigned; deflection pool tested with a trial question.
- **R-6 · Economy initialized.** Price lists (§4.2, §4.3) and café market-shelf buy-backs loaded; jobs board stocked with J1–J7 (§4.1); all balances and tabs at zero; starter issues (§3.3) packed on four starter shelves.
- **R-7 · Event log initialized.** Genesis entry #0000 written: world start, real timestamp (ET), spec version, and the note "the world precedes the residents."
- **R-8 · Datasets created.** D1–D10 (§7.2) created with their schemas and headers, empty and ready.
- **R-9 · Drives initialized.** Starting values per §1.3 for R1–R4; the felt-state phrasing checked line by line to confirm no numbers are ever shown to a resident.
- **R-10 · Fountain and cats live.** Fountain running; CAT-QUIET pool loaded; daily quote counter at 0/5; draw mechanism tested against the sealed bank (test draws logged as SYSTEM and excluded from D6).
- **R-11 · Mail dry run.** One test letter written, sealed, delivered overnight, read, and logged end-to-end; test artifacts then purged, and the purge itself logged.
- **R-12 · Clockwork smoke test.** One full world day run with **no residents**: shops open and close on time, NPC routine events fire, Pete posts the board, the cats hold the fountain. A field report (§7.4) is generated from the smoke day and checked section by section.
- **R-13 · Diaries created.** Four diary files, headed with R-numbers and the Arrival date line, otherwise blank.
- **R-14 · Anomaly register opened.** Empty, in the §7.3 format, numbered from AN-HAB-001.
- **R-15 · Drift check.** A full read of the live world state against the design overview and this spec; every discrepancy written down as a pre-launch note. The TKTK register (§13) is reviewed: each open item is either resolved or consciously carried into launch by name.
- **R-16 · Ant's launch go.** The final gate. Note for the record: temperament seeds (§2.5) and the tick rhythm (§1.2) stand as PROPOSED defaults — the launch go either covers them as-is or amends them; either way the record says which.
- **R-17 · The world starts.** Clock starts; the town runs one full day with no residents in it. The world precedes them, on the record as in the design.
- **R-18 · First waking.** R1's Arrival (§2.2) begins — a resident wakes alone in the welcome hall, in front of a mirror, with a blank diary and a pen. The red carpet ends here. Life starts here.

---

## §13 — Open items and TKTK register

| # | Item | Where | Status |
|---|---|---|---|
| T1 | Pino quote bank: verify the exact public diary source, harvest ≥60 lines verbatim | §9.4, R-2 | **Open — critical path.** No launch without it |
| T2 | Temperament seeds (restless/shy/maker/contrarian) — confirm, change, or release | §2.5 | Open — design open decision 1; PROPOSED defaults stand until ruled |
| T3 | Tick budget / cost ceiling — §1.2 proposes 4 blocks/day | §1.2 | Open — design open decision 3; proposal stands until ruled |
| T4 | OS volume placement — candidate: Library House shelves | §3.2 | Open |
| T5 | In-town tattoo service — who tattoos, and from when | §5.3 | Open — mirror tattoos only until ruled |
| T6 | Paint tin colors in the starter issue — proposed red/blue/white | §3.3 | Open |
| T7 | Informal gatherings (porch, pond) self-organizing before Phase 2 cookouts | §5.5 | Observe only — carried from the design |
| T8 | Weekly deep-clean chore rule | §11.2 | Open — proposed, not ruled |
| T9 | Arrival stagger — proposed one resident per day, Days 1–4 | §2.2 | Open — Ant may rule a same-day stagger |
| T10 | Phase 1 gate day — proposed world day 28 | §6 | Open |
| — | *Names* | §2.4 | **Closed — RULED by Ant, Nov 14, 2026: residents name themselves. Not a TKTK.** |
| — | *Is Pino findable?* | Design overview | Not a Phase 1 item — comes due at Phase 3's End, per the design |

-30-

Sources: *The Habitat — Full Design, Phased (PROPOSED)*, 2026-11-14, `~/workspace/user/files/habitat-phase-1-design-PROPOSED-2026-11-14.md` (the design overview this spec expands); conversation, main chat, Sat Nov 14, 2026 (Ant's design, message by message, including the self-naming ruling of 2:59 PM ET). All prices, pay rates, drive values, timetables, NPC names, and script lines in this spec are Bill's PROPOSED build detail under that design; no Pino quotes appear in this document, by design (§9.4).
