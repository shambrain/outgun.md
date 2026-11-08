# outgun.md
complete framework game maker project directive
# OUTGUN — OpenCode Master Mission Directive

**Document:** `OUTGUN-OpenCode-Master-Directive.md`  
**Project:** OUTGUN  
**Target workspace:** `C:\Users\Administrator\GameMakerProjects\Outgun_Framework`  
**Engine baseline:** GameMaker LTS 2026.0, GMS2 Runtime 2026.0.0.23 (verify installed version before use)  
**Prepared:** 3 November 2026  
**Directive revision:** 1.0  
**Primary objective:** Build a polished, extensible, fully playable vertical-shmup framework that the artist/game creator can finish by replacing clearly identified placeholder art, animation and audio assets and tuning documented data—not by having to rewrite the engine.

# OUTGUN — Master Directive v2.0 Creative and Gameplay Addendum

**Document:** `OUTGUN-OpenCode-Master-Directive-v2-Addendum.md`  
**Applies to:** `OUTGUN-OpenCode-Master-Directive.md`  
**Project:** OUTGUN  
**Project root:** `C:\Users\Administrator\GameMakerProjects\Outgun_Framework`  
**Asset source:** `C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Assets for Use`  
**Original soundtrack:** `C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_ost`  
**Prepared:** 6 November 2026  
**Revision:** 2.0 — additive clarification; original directive is retained.

---

# IMPORTANT — HOW THIS ADDENDUM APPLIES

This is an **additive amendment** to the existing `OUTGUN-OpenCode-Master-Directive.md`. Insert this entire addendum immediately after the original document's title/metadata and before `# 0. EXECUTION ORDER`, or provide it to OpenCode alongside the original master directive and explicitly instruct OpenCode to read this addendum first.

**Do not delete, shorten, silently omit or replace any non-conflicting requirement from the original master directive.** The original sections, technical requirements, quality rules, code standards, documentation obligations, six-boss ambition, art requirements, audio requirements, save requirements, co-op requirements and implementation milestones remain in force.

Where the original master directive conflicts with the new, more specific creative decisions below, this v2.0 addendum is authoritative. OpenCode must update the affected original sections in place, preserve all unrelated content, and record the reconciliation in `docs/DECISIONS.md`. Do not leave contradictory requirements active in the final documentation or code.

The key authoritative changes are:

1. The opening playable sequence is a **one-time, non-replayable Prologue starring the real Diamond Ace**, ending in a scripted, unwinnable encounter with OMEN. It is not a tutorial or training stage.
2. There are **six Story missions and ten Remix/Arcade missions**. The Prologue is separate and is not counted as one of the six Story missions.
3. Story and Remix encounters use **authored, repeatable, memorisable spawn and attack choreography**. Adaptive/dynamic encounter generation is isolated into a separately selected mode and never changes the normal Story or Remix patterns.
4. Spawners and altitude changes are driven by **authored vertical world progression and level markers**, not merely elapsed time.
5. Campaign backgrounds are **long, authored, finite, non-looping level paths**. Parallax layers move at different rates but must not restart the entire background when they reach an edge.
6. The altitude display has three clear states: **LOW, MEDIUM and HIGH**, with explicit gameplay and spawn eligibility rules.
7. There is **exactly one named/visible Pitstop per Story mission** and no Pitstops in Remix. Additional automatic safe-save anchors are not Pitstops.
8. Story power-ups are authored and predictable; Remix may use seeded random temporary power-ups. Permanent aircraft upgrades remain a separate, persistent Hangar system.
9. The first Story mission follows the specific beach-to-cloud-to-missile-defusal sequence described below. The existing Oasis soundtrack track remains the first soundtrack track as requested, even though the newly specified opening visuals are coastal.
10. Import and correctly assign the existing placeholder/source assets from `Assets for Use`, and preserve and correctly import the original soundtrack from `Outgun_ost`.

---

# A. PROJECT PATHS AND SOURCE-ASSET RULES — AUTHORITATIVE

Use these exact paths unless inspection proves the workspace has been moved. Do not substitute an older `C:\Users\shambrain\...` path or another user's profile path.

```text
C:\Users\Administrator\GameMakerProjects\Outgun_Framework\
    Assets for Use\       # Existing art, vectors, text material, sprites and backgrounds
    Outgun_ost\           # Original OUTGUN soundtrack; preserve source files
```

## A.1 Inspect and correctly assign all supplied assets

The user states that placeholder/source materials have already been added. Do not generate replacements before inspecting these folders.

Inventory every file under `Assets for Use` recursively. Identify, where possible:

- Player/Diamond Ace aircraft artwork.
- Enemy aircraft and ground-unit artwork.
- Boss concepts and boss-part artwork.
- Backgrounds and long-form level artwork.
- Tiles, tile sheets and environment components.
- Vector artwork and icon material.
- Text files, design notes, labels, reference sheets and animation notes.
- Sprite sheets, animation frames and cutscene material.
- HUD, menu, pickup, projectile and effect artwork.

For each asset, record the relative path, file type, dimensions, transparency, animation/frame information where detectable, intended role, source classification and GameMaker resource to which it is assigned. Create or update `docs/ASSET-ASSIGNMENT-MAP.md` and `assets/manifests/ASSET-MANIFEST.csv` (or an equivalent machine-readable manifest).

**Correct assignment and scaling are mandatory.** Do not simply import every image at arbitrary dimensions or assign every image to a generic sprite. Inspect image aspect ratios and transparency; set origins deliberately; preserve intended pixel density; avoid stretching; set appropriate filtering/compression; and distinguish backgrounds, sprites, UI, tiles, vector exports and sequence frames. Where a supplied vector format is not supported directly by the chosen runtime, create a documented raster export at appropriate resolutions while preserving the original vector source unchanged.

For every imported sprite, deliberately configure:

- Resource name and stable ID.
- Frame order and playback speed where animated.
- Origin/anchor appropriate to the object's behaviour.
- Bounding box and collision mask independent from the visual artwork where necessary.
- Texture page/atlas grouping where practical.
- Filtering and interpolation appropriate to pixel art or smooth illustration.
- Scale in world units based on source dimensions and intended on-screen size.
- Animation events through semantic events, not hardcoded frame-number gameplay logic.

For backgrounds, record their intended world width/height, visible playfield fit, scroll direction, art scale, parallax depth and whether each piece is finite or deliberately repeatable. **Do not loop a long unique stage background.**

Create contact sheets for supplied assets and a visual audit showing the imported resource beside its source. Report any unidentified, duplicated, corrupted or unsupported assets. Never silently discard an asset because its purpose is unclear.

## A.2 Original soundtrack — preserve and map it

Recursively inspect `Outgun_ost`. Preserve all original audio files byte-for-byte. Do not overwrite, destructively normalise, re-encode over, rename or move the source files. Import/copy into GameMaker's managed resource structure only as needed, keeping the source folder intact.

The **Oasis track is the first soundtrack track**. Identify the actual file and map it to the first intended soundtrack slot in `data/audio/music_manifest` (or the project's equivalent). Do not guess a filename. Do not rename the source file to make the mapping convenient. Record the GameMaker sound resource name, source path, format, duration, loop metadata and any verified BPM/cue data.

The first Story mission's newly specified opening environment is a beach/coastal sequence. This is a creative distinction from the Oasis track name. Keep the Oasis track first as requested; do not force an Oasis visual biome into the beach sequence simply because of the music filename. Preserve all Oasis artwork for later authored use, Remix missions, or a deliberately designed transition where it fits.

Create `docs/AUDIO-ASSET-MAP.md` linking every original track to its GameMaker sound resource, mission/mode usage, loop/cue markers and import settings. BPM and beat offsets must be measured from the actual files or left unknown; never invent them.

---

# B. CANONICAL GAME MODES AND CONTENT COUNTS

OUTGUN has three distinct gameplay categories. Keep their rules, records, progression and encounter systems clearly separated.

## B.1 One-time Story Prologue — Diamond Ace

The Prologue is the opening story sequence, not a training/tutorial level.

- The player initially controls the **real Diamond Ace**, the original legendary pilot.
- The player gets to experience the aircraft and basic shooting naturally through a short playable combat sequence. Do not stop the action for a tutorial curriculum, instructional pop-ups or training-room objectives.
- OMEN appears as the overwhelming threat.
- The player fights OMEN in a deliberately **unwinnable, canonically scripted encounter**. The player may shoot, dodge and demonstrate skill, but the encounter inevitably reaches its authored defeat/cutscene outcome.
- This outcome must not be presented as a normal skill failure. Do not make the player repeat an impossible boss fight, lose campaign progress or believe they failed to execute a hidden mechanic. The game must communicate through presentation that this is a story event.
- The sequence transitions into the established premise: the real Diamond Ace is dead; Rookie continues the mission in the same aircraft, and enemies subsequently mistake Rookie for Diamond Ace.
- The Prologue plays once on a fresh campaign/save. It is not listed as a mission, is not available through mission select, has no boss-practice entry, and cannot be replayed from the menu or archive. A deliberate full campaign reset may create a new campaign and replay its opening once; do not provide a separate Prologue replay button.
- Save a durable `prologue_completed`/`prologue_seen` flag so returning to the title or loading a save does not replay it unexpectedly.
- Use the existing OMEN boss architecture and art placeholders. The Prologue version may use a limited early form/moveset, while the final Story encounter uses the complete final boss. Both must be recognisably the same entity.

There is **no separate Training mode or Training room**. Remove/reconcile references to a replayable tutorial sortie, advanced training curriculum, tutorial selection or training completion as a campaign requirement. This does not prohibit a normal controls/settings screen or concise in-context HUD labels when they are genuinely needed.

The original directive's replay and learning philosophy remains: players improve by replaying authored missions, recognising patterns, refining routes and attempting cleaner runs. Do not replace that loop with a mandatory tutorial.

## B.2 Six Story missions

The Prologue is separate. The main narrative consists of exactly **six Story missions**, unlocked through the Hangar/mission board. Story missions carry the plot, briefings, radio conversations, mission objectives, persistent aircraft progression and one Pitstop each.

The canonical order is:

| Story mission | Primary environment/identity | Core encounter |
|---|---|---|
| 01 | Shallow Beach → Beach → Cloudline pursuit | Dune Regent launch-platform appearance, missile chase and missile-defusal climax |
| 02 | Snow | WHITEOUT |
| 03 | Hills | IRON HART |
| 04 | Ground Army | WAR MAUL |
| 05 | Sea | ABYSSAL CROWN / LEVIATHAN with animated tentacles |
| 06 | Enemy fortress / black-site / final approach | OMEN, full final boss encounter |

These are six narrative missions, not six training stages plus six missions. Mission IDs should be stable and data-driven (`story_01` through `story_06`) regardless of display-name changes.

Preserve the previous DUNE REGENT concept and art direction. In Story Mission 01, Dune Regent is the enormous enemy platform/commander encountered over the beach. It is **not fought in that opening encounter**; it launches the missile and withdraws/escapes according to the authored sequence. Preserve Dune Regent as an enemy/boss asset and behaviour concept so it can return in a later Remix mission or additional encounter if the content design calls for it. Do not delete its resource or concept merely because it is not the first conventional boss battle.

For the six principal Story climaxes, treat the missile-defusal encounter as the first boss-equivalent finale/set piece, followed by WHITEOUT, IRON HART, WAR MAUL, LEVIATHAN and OMEN as the five conventional major boss battles. Do not count the Prologue's scripted OMEN encounter as an additional campaign mission or as a seventh final boss.

## B.3 Ten Remix / Arcade missions

Create **ten separate Remix missions** (`remix_01` through `remix_10`). These are side missions, not story chapters.

- No narrative campaign dependency, no new mandatory story scenes and no story dialogue required. Short non-story callouts are allowed only when they serve gameplay.
- Reuse and recombine the six Story missions' environments, enemy art, bosses/components, tilesets, effects and parallax assets. Reuse should be intentional: new route composition, enemy formations, altitude sequences, projectile choreography, boss combinations and visual arrangement—not merely the same level copied ten times.
- Use alternate/remixed soundtrack arrangements or suitable alternate tracks where available. Preserve the original soundtrack files; do not invent or overwrite music. If alternate OST material is not present yet, provide clearly named configurable music slots and mark them as waiting for the creator's audio.
- Target roughly **3–5 minutes per Remix mission**, with **five minutes as the hard authored target ceiling** before results. Do not pad missions with empty scrolling. Shorter, purposeful stages are preferable.
- Remix is the pure arcade challenge: demanding, tightly authored, pattern-focused and replayable. Every mission ends with a meaningful boss or boss-like climax. A Remix boss may be a returning Story boss, Dune Regent, a component variant or a newly configured composite assembled from existing assets.
- **No Pitstops and no Story checkpoint/continue anchors** in Remix missions. A failed run restarts the selected Remix mission according to its arcade rules. Preserve the score/record history, not an in-progress checkpoint.
- Remix enemy waves and attack choreography remain fixed and learnable. Random temporary power-ups may be used as described in Section F; do not randomise enemy encounters.
- Maintain separate score records, medals and leaderboard rules from Story mode.

## B.4 Separate Dynamic Operations mode

Dynamic/adaptive encounter generation is not part of the normal OUTGUN shmup campaign or standard arcade Remix mode.

If dynamic encounter logic is implemented, it must be a **separate, explicitly selected mode** (working label: `Dynamic Operations`). It must have its own menu entry, ruleset ID, records and documentation. Its adaptive spawners may vary encounter composition within authored safety limits, but they must never alter Story or Remix wave order, spawn locations, pattern choreography, altitude schedule, boss phase timing or fixed level triggers.

Do not put adaptive difficulty, reactive wave replacement, performance-based enemy spawning or hidden encounter changes behind the normal Story/Remix director. The player should be able to memorise the standard mode and pursue a repeatable perfect run.

If resource/time limits prevent a full Dynamic Operations mode, implement only a clearly isolated future extension point and mark the mode as not implemented. Do not activate a fake dynamic system in Story or Remix to claim completion.

---

# C. FIRST STORY MISSION — EXACT OPENING SEQUENCE

Story Mission 01 must implement this sequence as an authored, vertical-progress-driven set piece. This is a core OUTGUN identity feature, not a generic placeholder mission.

## C.1 Shoreline opening

1. Start low over a **shallow beach**. The player sees the sea/shoreline, shallows, sand and coastal features moving at controlled parallax rates.
2. The level progresses into the main **beach** environment. Use the supplied backgrounds and art where suitable; do not loop a short image to fake a long level.
3. The player encounters the enormous Dune Regent/launch platform. The platform is revealed with a memorable entrance, radio reaction and environmental scale.
4. Do not turn this into a conventional boss fight. Dune Regent launches a missile aimed at specified city coordinates, then escapes or becomes unavailable to attack according to the authored story beat.
5. The missile launch, city target and urgency must be visually and narratively clear. Show the target/destination through an appropriate HUD/radar cue and Operator radio dialogue.

## C.2 Cloudline pursuit

6. Rookie climbs above the clouds to pursue/intercept the missile. This is an authored altitude transition from LOW/MEDIUM to HIGH.
7. The background changes from beach/low-altitude scenery to clouds and high-altitude sky layers. Preserve the finite stage path and smooth parallax continuity; do not reset or loop the beach background.
8. Spawn enemy aircraft in fixed formations along the vertical course while Rookie catches up to the missile. These waves are triggered by stage progress markers, not by a free-running timer.
9. The missile remains ahead of the player until the authored interception point. Use a distance/progress indicator if it improves readability. The player fights through the waves but cannot accidentally bypass the mission event by scrolling past its trigger.

## C.3 Missile defusal — first boss-equivalent climax

10. Once Rookie catches up, enter the first mission's boss-equivalent encounter: the missile is the central mission-critical target. This is a focused objective encounter, not an ordinary enemy that can be destroyed by shooting.
11. Operator explains over the radio that she can hack/defuse the missile, but Rookie must remain close enough to establish the link.
12. Display a **medium-sized circular defusal radius** around the missile. The circle must be visually readable and scaled in world-space so its interior is large enough for precise dodging while still requiring the player to stay close.
13. The player must remain inside the circle for the defusal bar to fill. While inside, the bar advances at a clearly communicated rate. When the player leaves the radius, progress pauses (do not silently reset the whole bar). Re-entering continues the existing progress.
14. Taking a hit must not erase all defusal progress. Apply the normal player hit/invulnerability rules; the defusal action may pause briefly during a hit/respawn state only if the reason is clearly communicated.
15. Enemy aircraft attack in an authored **circular/orbital formation around the missile**. Use the emitter/pattern system to produce rotating formations, aimed attacks and controlled gaps. Their orbit and firing sequence must be fixed and repeatable so the player can learn it. Never rotate/spawn enemies randomly into the player's position.
16. Provide clear HUD feedback for the defusal bar, link status, distance/radius and any missile impact deadline. The Operator's dialogue must explain the objective once, then remain concise during combat.
17. Completion of the defusal bar triggers a validated, one-shot `MISSILE_DEFUSED` event, stops/neutralises the missile, resolves remaining hostile attacks safely and plays a short cinematic/radio payoff. Do not award the mission reward twice if the event is replayed or a save is loaded.
18. If the missile deadline is used, make the time pressure understandable and fair. Define the failure/retry path. A failed attempt must not create a softlock or erase unrelated permanent progression. Prefer a clear authored retry from the nearest valid Story recovery point.

The missile encounter must be implemented as a reusable objective/boss-event component so other missions can reuse hold-radius, escort, disable, intercept or multi-terminal mechanics without copying the entire mission script.

---

# D. AUTHORED VERTICAL PROGRESSION, FINITE LEVELS AND MANUAL TRIGGERS

## D.1 The level is a finite authored path

The main Story and Remix backgrounds are long, unique, vertically progressing level environments. They are **not automatically looping backgrounds**.

- Define a world-space level length and a monotonically increasing stage-progress value.
- The camera/scroll controller advances through the authored level path.
- Each parallax layer has its own offset and speed multiplier but is bounded by its authored content length or explicit end behaviour.
- At the final world coordinate, execute the mission's exit/boss/result event; never wrap the stage to its beginning.
- Do not repeat a finite background texture merely to hide the end of the artwork.
- Individual effects, small water textures, clouds or decorative strips may repeat only when the asset configuration explicitly marks them as repeatable. This must not make the overall stage path loop.
- Preserve tilemaps for authored terrain and ground surfaces where they are useful. Tiling does not mean the mission itself loops.

## D.2 Vertical progress is the primary encounter clock

The primary mission director must use **vertical level progression/distance** as its trigger domain. Time may still be used inside a projectile pattern, animation, cooldown, laser charge or dialogue delay, but it must not be the sole clock for deciding when a wave appears along a finite level.

Support both editor-placed trigger instances/markers and validated data-defined trigger records. The creator should be able to visually place a trigger at a world-space position in the Room Editor or change its progress coordinate in a documented mission data file.

Each marker must have:

- Unique stable trigger ID.
- Mission ID and world progress coordinate.
- Trigger type: spawn formation, altitude change, dialogue, music cue, camera event, background/layer change, objective, miniboss, boss entrance, Pitstop, autosave checkpoint or other allowlisted event.
- Spawn/formation/pattern ID where applicable.
- Altitude requirement/filter.
- Optional prerequisites and one-shot/repeat policy (default one-shot per attempt).
- Debug label and editor-visible marker/category.

When the camera crosses a trigger coordinate, process every crossed trigger in deterministic coordinate/priority/ID order. Large frame-time spikes must not skip a marker. Restarting or resuming must not fire an already completed one-shot trigger twice. Save the trigger ledger/progress so the mission resumes correctly.

Do not use a dynamic encounter director to move triggers or replace the authored wave schedule in Story or Remix. The same inputs and mode configuration should lead to the same encounter order.

## D.3 Spawner and formation library

Build a reusable authorable spawner system. It must support formation templates including at least:

- Single entry.
- Straight horizontal line.
- Straight vertical column.
- Diagonal line (left-to-right and right-to-left).
- V formation and inverted V.
- Wedge/arrow formation.
- Staggered/offset rows.
- Double-column formation.
- Diamond/box formation.
- Arc/curved formation.
- Circular/orbital formation.
- Split formation that opens into two lanes.
- Custom waypoint formation.

Each formation is configurable for enemy type, count, spacing, spawn edge/anchor, direction, movement profile, entry speed, delay between members, attack delay, altitude filter and optional emitter pattern. Store reusable presets in clearly named folders and provide a `docs/SPAWNER-101-GUIDE.md` with visual diagrams/contact sheets showing what each pattern looks like and which parameters the brother can change.

Provide an in-game/debug preview or test room that lets the designer select a formation, enemy type, altitude state and movement profile and see the result without editing shared engine code.

## D.4 Emitter configuration belongs to every ship

The **player ship's guns must use configurable emitters**, and enemies/bosses must use the same reusable emitter architecture where applicable.

An emitter configuration should define muzzle/anchor offset, projectile type, cadence, burst size, spread, angle, speed, acceleration, damage, lifetime, visual/trail/effect, sound, cooldown, simultaneous emitter group and weapon-level modifications. Support multiple emitters per ship (for example left/right guns, central gun, wing pods, missile launcher or boss weapon ports).

The player weapon resource must not hardcode a single projectile pattern into the player Step event. The brother must be able to modify the ship's emitter list and each enemy's emitter list through data/configuration. Keep player/enemy ownership, collision masks and damage rules explicit.

---

# E. ALTITUDE SYSTEM — LOW / MEDIUM / HIGH

Altitude is a readable gameplay/world-state system. The player does not need to freely fly into a different depth plane using a 3D vertical axis. The authored mission path defines the aircraft's altitude state through manual vertical-progress trigger markers.

The HUD meter must display exactly the understandable bands **LOW**, **MEDIUM** and **HIGH** (visual styling may include a simple icon/bar, but do not require a numeric height readout).

## E.1 Altitude rules

- **LOW:** the aircraft is close to the ground. Ground forces are eligible and expected: turrets, tanks, APCs, SAM sites, artillery, convoys and ground-boss components. Aerial enemies may also appear.
- **MEDIUM:** transitional/mixed altitude. Use a designed mixture of ground and flight encounters according to the mission's authored trigger schedule. Ground units remain eligible where the art/terrain supports them.
- **HIGH:** the aircraft is well above the ground/cloud layer. Ground combat is locked out: do not spawn ground-unit encounters in HIGH zones. Use flight-only encounters. High-altitude aircraft should be more challenging, with more unique, faster, stronger or more advanced ship archetypes and attack patterns.

High altitude must not simply mean all existing enemy bullets become unfairly fast. Introduce advanced archetypes and more complex but readable combinations. The difficulty increase is fixed and authored, not adaptive to player performance.

## E.2 Manual altitude triggers

Create editor-placeable `ALTITUDE_ZONE` markers at vertical world-progress coordinates. A marker changes the altitude state and may reference a short transition sequence, parallax profile, cloud/fog effect, music cue or allowed enemy roster.

The altitude transition must be visible and smooth: change background depth, ground visibility, cloud layers and HUD state with a short controlled transition. The gameplay state changes at a deterministic trigger point. Do not spawn ground units in advance of a HIGH state and then delete them abruptly; filter the spawn catalogue before spawning.

Every mission should have a deliberate altitude schedule. Story Mission 01 must specifically transition from beach/LOW through the ascent to cloudline/HIGH. Other missions may move between LOW/MEDIUM/HIGH as appropriate to their environment. Remix schedules may reuse and rearrange these zones, but remain authored and fixed.

Create `docs/ALTITUDE-SYSTEM-101.md` explaining the three bands, how the author places a trigger, which enemy categories are permitted, and how to preview the transition.

---

# F. PERFECT-RUN DESIGN, FIXED ENCOUNTERS AND POWER-UP ECONOMY

## F.1 Story and Remix must be repeatable

A core shmup pleasure is learning an encounter, remembering formations and bullet patterns, then improving the route toward a cleaner or perfect run. Respect that genre contract.

In **Story and Remix**:

- Enemy waves, spawn locations, formation types, altitude changes, boss phases and attack schedules are authored and repeatable.
- Do not change the next wave because the player scored highly, took damage, killed an enemy quickly or lingered in a lane.
- Do not silently tune bullet speed, number, angle, enemy HP or spawn position during a run.
- Do not use hidden dynamic difficulty/adaptive encounter replacement.
- Any aimed shot may calculate its direction from the player's position at the exact firing/commit moment, as a normal authored shmup attack, but it must then travel according to its defined trajectory. Do not continuously home an ordinary bullet at the player unless it is a clearly signposted homing projectile with bounded turn rate.
- Seeded variation is allowed only where explicitly specified, recorded and reproducible. Do not randomise the core pattern choreography.

The goal is not that every run looks identical in every incidental detail; the goal is that the player can learn the level's structure, timings, threat roles and safe routes through repeated play. Maintain deterministic trigger ordering and stable attack patterns.

## F.2 Separate Story power-ups from permanent upgrades

Use three distinct concepts:

1. **Permanent Hangar upgrades:** persist across Story sorties and are purchased/unlocked through the Calibration Bay. They modify the aircraft within balanced caps and are documented in the save schema.
2. **Story mission power-ups:** placed at authored vertical-progress positions and/or delivered by specifically authored enemy drops. These are predictable, curated and repeatable. Do not use random spawn locations or hidden probability rolls in Story mode.
3. **Remix temporary power-ups:** randomised/weighted arcade-style temporary pickups may be used in Remix. Their spawn opportunities must be authored (the pickup can vary, but an unannounced object must not appear in an unsafe location). Use a controlled seed and record it with the run so the exact pickup sequence can be reproduced for testing/replay. Temporary Remix power-ups reset according to the Remix run rules and do not silently become permanent aircraft upgrades.

## F.3 Permanent upgrade balance and arcade fairness

Permanent upgrades should provide meaningful but bounded choices. Do not create huge multiplicative damage/health bonuses that make the Story campaign or leaderboard difficulty collapse.

For **ranked/standard Remix**, use a standardised aircraft baseline so permanent Hangar upgrades cannot create an unfair score advantage. The selected aircraft appearance may carry over, but ranked gameplay statistics must be normalised. If a future unranked Remix variant allows a fully upgraded Hangar loadout, label it clearly and keep its records separate.

Story missions use the player's permanent aircraft configuration plus their mission's authored temporary power-up route. The two systems must be visible in the UI and saved separately.

## F.4 Perfect-run scoring and records

Track clean clears, continues, deaths, bombs used, damage received, score, combo, enemy route efficiency, boss part destruction and mission time. Define a clear perfect/no-continue/low-damage medal policy. Never call a run perfect if assists/continues or a different ruleset invalidate that award. Preserve the encounter and score ruleset version with the record so later balance changes do not misrepresent old results.

---

# G. PITSTOPS AND STORY RECOVERY CHECKPOINTS

The terms **Pitstop** and **automatic save checkpoint** must be distinct in code, UI and documentation.

## G.1 Exactly one Pitstop per Story mission

There is **one and only one named, visible Pitstop per Story mission**. It is a deliberately authored safe intermission/maintenance point where the player can take a break and the current sortie is safely persisted.

- Exactly one `PITSTOP` event/marker per Story mission.
- Do not generate a second Pitstop because a mission has a miniboss, extra event or long section.
- A Pitstop can include a visual service bay, support carrier, safe radio moment or pause-to-break screen; match the art and story context.
- The Pitstop is not the Hangar and does not replace the permanent upgrade system.
- It must preserve the exact current run state needed to continue: mission ID, world progress, altitude, player lives/health/status, weapon levels, secondary stock, bombs, temporary power-ups, permanent upgrade snapshot/version, XP/rewards, score, combo state, active mission objectives, cleared component flags, current event/phase, trigger ledger, deterministic random seed/state, pending encounter schedule and relevant story/dialogue flags.
- Prefer placing the Pitstop at an authored safe boundary with no hostile bullets actively crossing the player. If an encounter must be suspended, implement exact restoration of its active state; do not silently restart the encounter while claiming an exact resume.
- Saving must be atomic and validated. Keep recovery/backup behaviour from the original save-system requirements.
- When returning to the game, offer a clear resume option. Do not unexpectedly replay the Prologue or restart the whole Story mission.

## G.2 Story autosave/recovery anchors are not extra Pitstops

In addition to the one visible Pitstop, Story mission data must support automatic safe-save checkpoint anchors at these authored boundaries when they exist:

1. Mission start.
2. Midpoint (if the mission has a meaningful midpoint).
3. Immediately after a miniboss (if present and once the encounter has resolved).
4. Before the final boss or major final event.
5. At the start of any additional major event that would be frustrating to repeat from far earlier in the level.

These are **silent/low-friction recovery snapshots**, not visible repair stations, not additional Pitstops and not extra Hangar visits. Each mission may omit anchors that do not apply; never invent a miniboss solely to justify a checkpoint.

Checkpoint snapshots must be taken at a deterministic safe boundary. Record the trigger ID and mission progress. If an automatic snapshot would otherwise occur during an active projectile pattern, wait for the authored safe transition rather than serialising a half-resolved combat frame without the required state.

## G.3 Death, continues and resume rules

Separate ordinary arcade life loss from suspend/resume:

- Losing a life during a sortie follows the mission's documented shmup rules (hit feedback, invulnerability, respawn and any weapon degradation). It does not automatically teleport the player to the Pitstop.
- A player who closes the game or chooses to take a break resumes from the most recent valid Story snapshot/Pitstop according to the save policy.
- A game-over/continue flow may offer the most recent recovery anchor, with continue use clearly recorded for score/medal eligibility. It must not create an infinite free perfect-run loop.
- Replaying from a checkpoint must reset transient enemy/projectile state to the exact authored checkpoint definition and must not double-award previous XP, drops, score or story unlocks.
- Preserve a true fresh-mission restart option for players who want to practise the full perfect run.

## G.4 Remix has no Pitstop or checkpoint system

Remix missions have **zero Pitstops and zero in-level recovery checkpoints**. No visible Pitstop, no safe break station and no continue-from-mid-stage snapshot. The player can pause the game normally, but after a failed/abandoned Remix run, restarting begins that selected Remix mission according to its arcade rules. Preserve completed scores and records independently.

Create `docs/PITSTOP-AND-CHECKPOINTS-101.md` explaining the one-Pitstop rule, the separate automatic save anchors, the exact resume payload and the different Story/Remix failure rules.

---

# H. AUDIO, DIALOGUE AND MISSION TIMING ADDITIONS

Keep all original audio architecture requirements. Add these concrete mappings:

- The first soundtrack slot is the existing **Oasis** track found in `Outgun_ost`.
- The first Story mission's initial visual route is shallow beach → beach → above-cloud pursuit. Do not reorder the actual soundtrack files merely because the first environment is coastal.
- The first mission's missile launch receives a clear music/impact cue; cloud pursuit can use a controlled transition/crossfade; missile defusal receives a dedicated tension/defusal cue where the existing music supports it.
- Remix missions should have alternate/remixed OST slots. Use source audio only when present and verified. Missing alternate compositions are authoring slots, not permission to generate or overwrite the original OST.
- Radio dialogue for the first mission must include the initial enemy-platform sighting, missile launch/city warning, ascent/pursuit, Operator's explanation of the close-range hack requirement, defusal progress feedback and successful defusal/failure result.
- Do not allow repetitive Operator lines to fire on every frame while the player enters/leaves the defusal radius. Use one-shot trigger IDs and sensible thresholds/cooldowns.

Music remains presentation support, not a rhythm dependency. Enemy pattern timing and vertical world triggers must be stable even if audio is muted, paused or unavailable.

---

# I. REQUIRED FOLDER/EDITOR GUIDES — BEGINNER FRIENDLY

Preserve the original folder-organisation goal and make the project approachable to a beginner. Add or update these guides, using actual implemented resource paths and screenshots/diagrams where useful:

- `docs/START-HERE-101.md` — how to open the project, run it, understand the GameMaker asset browser, find the player, enemy, boss, room and script resources, and make a safe change.
- `docs/PROJECT-MAP-101.md` — annotated folder/resource map; what belongs where; what not to edit casually.
- `docs/ASSET-ASSIGNMENT-MAP.md` — source asset → imported resource → in-game use → replacement instructions.
- `docs/LEVEL-AUTHORING-101.md` — finite stage construction, room dimensions, world progress, camera scroll, parallax layers and level end.
- `docs/SPAWNER-101-GUIDE.md` — place a vertical progress trigger and choose straight, diagonal, V, wedge, line, column, staggered, arc or orbital formations.
- `docs/ALTITUDE-SYSTEM-101.md` — LOW/MEDIUM/HIGH markers, allowed enemy types and visual transitions.
- `docs/EMITTERS-AND-WEAPONS-101.md` — configure the player gun and enemy/boss weapon emitters without editing core logic.
- `docs/PITSTOP-AND-CHECKPOINTS-101.md` — the one Pitstop and separate save/recovery anchors.
- `docs/STORY-AND-REMIX-101.md` — differences between Prologue, six Story missions, ten Remix missions and the optional Dynamic Operations mode.
- `docs/POWERUPS-AND-UPGRADES-101.md` — authored Story pickups, randomized Remix pickups and permanent Hangar upgrades.
- `docs/MISSION-AND-BOSS-AUTHORING-101.md` — add a mission event, boss phase, weakpoint and semantic animation trigger.

Guides must explain actual controls and files, not imagined features. Include concise diagrams/contact sheets where they materially help. The brother must be able to find an enemy, alter its speed/HP/formation/emitter, place its spawn trigger, preview it and run the mission without searching through unrelated GML code.

---

# J. UPDATED CONTENT AND ACCEPTANCE CHECKLIST

Update `docs/IMPLEMENTATION-TRACKER.md` (create it if absent) so every item below has a status of `NOT STARTED`, `IN PROGRESS`, `IMPLEMENTED`, `TESTED` or `BLOCKED`. `IMPLEMENTED` is not the same as `TESTED`.

## Prologue

- [ ] Real Diamond Ace playable with the original art where available.
- [ ] No tutorial/training curriculum or training menu.
- [ ] OMEN appears in a scripted unwinnable encounter.
- [ ] Canonical death/transition to Rookie is delivered without a fake skill-failure loop.
- [ ] Prologue is one-time per campaign and cannot be selected/replayed from menus.

## Story campaign

- [ ] Exactly six Story mission definitions (`story_01`–`story_06`).
- [ ] Mission 01: shallow beach → beach → Dune Regent launch encounter → missile launch to city coordinates → above-cloud chase → fixed enemy waves → close-radius missile defusal objective.
- [ ] Defusal circle, progress rules, Operator radio cues, circular enemy attack choreography and one-shot completion event tested.
- [ ] Mission 02: Snow/WHITEOUT.
- [ ] Mission 03: Hills/IRON HART.
- [ ] Mission 04: Ground Army/WAR MAUL.
- [ ] Mission 05: Sea/LEVIATHAN with independently animated tentacles.
- [ ] Mission 06: final fortress/OMEN.
- [ ] Existing Dune Regent design/art retained and its later reuse documented.

## Remix/Arcade campaign

- [ ] Exactly ten Remix mission definitions (`remix_01`–`remix_10`).
- [ ] No story campaign dependency or required story dialogue.
- [ ] Reuses and recombines existing six-mission art/environment/boss assets intentionally.
- [ ] Each mission has a fixed authored enemy/attack schedule and a boss/boss-like climax.
- [ ] Target 3–5 minutes; five-minute design ceiling.
- [ ] No Pitstops and no in-stage recovery checkpoints.
- [ ] Temporary random power-ups use a recorded/reproducible seed.
- [ ] Ranked Remix uses a standardised aircraft stat baseline; separate any unranked upgraded-loadout records.

## Dynamic Operations

- [ ] Separate selectable mode and separate ruleset/records if implemented.
- [ ] No adaptive wave changes leak into Story or Remix.
- [ ] If only an extension point exists, it is clearly marked as not implemented.

## Level progression and altitude

- [ ] Finite, unique, non-looping level path and end-of-level behaviour.
- [ ] Spawn triggers use vertical world progress/Room markers rather than only timers.
- [ ] Frame spikes cannot skip or reorder triggers.
- [ ] Straight line, diagonal (both directions), V, wedge, staggered, columns, diamond/box, arc and orbital formations available as presets.
- [ ] Player aircraft guns use editable emitter configurations.
- [ ] Enemy and boss weapons use the same reusable emitter architecture where appropriate.
- [ ] LOW/MEDIUM/HIGH altitude meter is visible and correct.
- [ ] LOW allows ground and aerial encounters; MEDIUM supports a designed mix; HIGH is flight-only and unlocks tougher/faster/more unique aerial enemies.
- [ ] Altitude changes are manually authored progress triggers and are previewable.

## Power-ups, permanent upgrades and perfect runs

- [ ] Story pickups and enemy drops are authored and repeatable.
- [ ] Permanent Hangar upgrades persist separately from run power-ups.
- [ ] Remix random temporary power-ups are seeded/reproducible.
- [ ] No hidden dynamic difficulty or adaptive enemy replacement in Story/Remix.
- [ ] Perfect/no-continue records use transparent eligibility rules.

## Pitstops and save/resume

- [ ] Exactly one named/visible Pitstop in each of the six Story missions.
- [ ] Multiple silent automatic safe-save anchors may exist only at the listed authored boundaries and are not called Pitstops.
- [ ] Pitstop resume restores the exact documented run state.
- [ ] Trigger ledger prevents duplicate waves, rewards, dialogue and events after resume.
- [ ] Remix has no Pitstops or checkpoint resume.

## Assets and music

- [ ] All files under `Assets for Use` inventoried, classified, assigned and scaled deliberately.
- [ ] Original source materials preserved unchanged.
- [ ] Asset contact sheets and replacement map generated.
- [ ] All audio files under `Outgun_ost` inventoried and original files preserved.
- [ ] Oasis is assigned as soundtrack track one.
- [ ] Music assets have verified import settings and documented cue/loop data.

---

# K. DECISIONS AND CONTRADICTIONS — RESOLVE EXPLICITLY

Record these decisions in `docs/DECISIONS.md`. Do not silently interpret the old and new requirements as if they were identical.

| Earlier directive / potential contradiction | Authoritative v2 decision |
|---|---|
| Replayable Training sortie, advanced training, training menu | Replaced by a one-time Prologue starring real Diamond Ace and a scripted unwinnable OMEN encounter. No separate Training mode. |
| Six missions plus a separate training mission | Exactly six Story missions; Prologue is outside the mission count; ten Remix missions are additional. |
| Dynamic encounters affecting ordinary enemy waves | Story and Remix use fixed authored schedules. Dynamic Operations is separate and cannot affect them. |
| First Story biome assumed to be Oasis | Mission 01's actual visual path is shallow beach → beach → cloudline. The existing Oasis soundtrack file remains track one. Preserve Oasis art and use it elsewhere where appropriate. |
| Dune Regent as the first conventional boss fight | Dune Regent is first encountered as a huge non-combat launch platform that fires the missile. The missile-defusal objective is the first boss-equivalent climax. Preserve Dune Regent for future/remix reuse. |
| Automatically looping scrolling backgrounds | Story and Remix are finite, authored level paths. Parallax scrolls; the overall stage does not loop. Individual decorative textures may repeat only when explicitly configured. |
| Spawners driven by elapsed time | Spawners are primarily triggered by vertical world-progress markers. Timers remain valid inside attack/cooldown/animation behaviour. |
| Altitude represented only as visual atmosphere | LOW/MEDIUM/HIGH is a visible, authored gameplay state that controls ground/flight enemy eligibility and the high-altitude enemy roster. |
| Random power-ups and persistent upgrades treated as one system | Story uses fixed curated drops; Remix may use seeded temporary random drops; permanent upgrades remain a separate Hangar progression system. |
| Multiple Pitstop-like checkpoints | Exactly one visible Pitstop per Story mission. Other safe autosave anchors are silent recovery snapshots, not Pitstops. Remix has none. |
| Boss practice/training as default genre onboarding | Normal learning is through repeatable authored level play and pattern memorisation. The Prologue cannot be replayed. Keep only already specified practice features that do not recreate a separate Training mode and do not undermine the perfect-run design. |

## K.1 One remaining design distinction to preserve, not “fix” by guessing

The first soundtrack track is named **Oasis**, but the new first-mission visuals are **shallow beach → beach → clouds**. Keep the Oasis track first exactly as requested and keep the beach visual path exactly as requested. Do not rename the track or force the level to become an Oasis stage. If the source assets later show that the coastal scene is intentionally part of a larger Oasis/coastal region, record that as a creative decision only after inspecting the actual art and notes.

## K.2 Pitstop versus checkpoint distinction is mandatory

“One Pitstop per Story mission” and “several Story checkpoint save boundaries” are compatible only when the terms are kept separate. A Pitstop is a single visible, named in-world rest/service event. Mission-start, midpoint, post-miniboss, pre-final-boss and additional-event save anchors are automatic safe snapshots. They do not create a second Pitstop. Never place multiple visible Pitstop stations in one Story mission.

---

# L. IMPLEMENTATION PRIORITY UPDATE

Keep the original M0–M6 milestones and every non-conflicting exit criterion. Reorder only the affected scope as follows:

1. **M0:** inspect the exact `Administrator` project path, `Assets for Use`, `Outgun_ost`, current `.yyp`, Git and installed GameMaker version. Preserve assets and establish baseline.
2. **M1:** core application shell, input, save system, fixed world-progress director, finite camera/scroll system, parallax profiles, altitude state, emitter architecture and Hangar foundation.
3. **M2:** implement the complete first playable Story slice from the one-time Prologue into Story Mission 01's shallow beach → beach → cloudline → missile-defusal finale, then debrief/Hangar. Do not build a training room. Prologue must be separate from the six Story missions.
4. **M3:** complete Story missions 02–06, six Story definitions, all boss frameworks, one Pitstop per Story mission, automatic safe-save anchors, XP and permanent upgrades.
5. **M4:** complete ten Remix missions, each with fixed patterns, short arcade duration, a boss climax, no Pitstop and seeded temporary random power-up rules. Preserve independent score records.
6. **M5:** implement the separate Dynamic Operations mode only after the fixed Story/Remix modes work; then complete local co-op, platform adapter, achievements and production polish.
7. **M6:** full campaign/Remix QA, save recovery tests, asset scaling audit, frame-rate tests, performance profiling, GitHub verification and release-readiness review.

Do not postpone the first mission's missile defusal mechanic until late polish. It is a core gameplay identity feature and must be playable in the first vertical slice.

---

# M. REQUIRED UPDATE TO THE FINAL HANDOFF

In addition to the original master directive's final handoff list, report:

1. Confirmation that the one-time Prologue uses real Diamond Ace and OMEN, and whether its scripted outcome is complete/tested.
2. Confirmation of exactly six Story mission definitions and ten Remix mission definitions.
3. A concise encounter schedule for all six Story missions and all ten Remix missions.
4. Story Mission 01's stage-progress trigger list and missile-defusal test evidence.
5. A diagram/list of LOW/MEDIUM/HIGH altitude segments for each mission.
6. A count of authored spawner presets and verified player/enemy emitter configurations.
7. Confirmation of one and only one Pitstop marker per Story mission, zero Pitstops in Remix, and the separate autosave anchor list.
8. Proof that Story/Remix wave schedules are deterministic and do not invoke adaptive encounter logic.
9. The random power-up seed/replay policy for Remix and the permanent-upgrade normalisation policy for ranked Remix.
10. The asset assignment/scaling report for `Assets for Use`.
11. The soundtrack mapping proving Oasis is track one and the source files in `Outgun_ost` were preserved.
12. Any creative distinction or unresolved art/music mismatch, particularly Oasis music versus coastal opening visuals.

**Start by preserving the existing project and applying this addendum to the original master directive. Then implement; do not stop at updating documentation.**


---

# 0. EXECUTION ORDER — READ THIS FIRST

You are the lead GameMaker engineer, gameplay architect, technical artist, tools engineer, QA engineer and production integrator for OUTGUN. Work directly in the existing project at:

`C:\Users\Administrator\GameMakerProjects\Outgun_Framework`

Placeholders have been added. Please make sure everything is correctly assigned and scaled. The directory is C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Assets for Use (includes arts, vectors, texts, sprites, and bgs). also orginial game soundtrack ,C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_ost , starting as oasis beign the1st track, laso update github so we dont lost track , 

The user has already created a blank GameMaker project and assigned this folder to OpenCode. **The existing project and all user-created material are the source of truth.** Do not assume it is literally empty. Inspect it first.

## Non-negotiable working rules

1. **Do the work, not just the plan.** Inspect, design, implement, run, test, fix, document and continue through the milestones below. Do not stop after producing an architecture proposal or a list of TODOs.
2. **Do not ask the user questions.** Make conservative, reversible assumptions, record them in `docs/DECISIONS.md`, and continue. Never invent story canon or overwrite original creative material when a detail is unknown.
3. **Preserve existing work.** Before changing files, inspect Git status, project resources and all asset folders. Identify original art, existing animation, music and sound effects. Back them up through Git or a timestamped backup before modifying anything. Never delete or overwrite original art/audio to make room for placeholders.
4. **Do not fabricate verification.** A file existing is not proof that it imports; a project compiling is not proof the gameplay works; a test stub is not a passed test. Record the exact commands, GameMaker version, build result, test result and any unverified items.
5. **Prefer native GameMaker capabilities.** Use the Room Editor, tilemaps, tilesets, Sequences, UI Layers/Flex Panels, Particle Systems, shaders, surfaces, audio groups and GML where they fit. Do not build a custom replacement for a capable native feature without a measurable reason.
6. **Keep the framework understandable.** The user's brother must be able to replace sprites, edit enemy stats, adjust movement, alter bullet patterns, change wave timing, tune boss phases and edit mission dialogue without understanding every engine subsystem.
7. **Every GML script must teach as well as work.** Give every public function a clear `///` documentation header stating purpose, parameters, return value and side effects. Add useful `//` comments explaining non-obvious calculations, state transitions, timing, collision decisions and tuning variables. Centralise adjustable values and explain what changing them does. Do not bury essential tuning values in large Step events.
8. **No architecture theatre.** Keep the system modular but proportionate. Avoid unnecessary frameworks, speculative abstractions, duplicate managers, unused variables, empty scripts, fake features and decorative documentation that does not match the implementation.
9. **Protect performance.** Profile before and after optimisations. Avoid repeated global searches, `instance_find()`/`with()` scans in hot Step loops, per-frame DS allocation, unbounded projectile creation, per-object shader changes and unbounded particle emitters. Cache stable references, use arrays/structs appropriately, pool high-volume projectiles where measurement supports it, and clean up resources deterministically.
10. **Use source control safely.** Never force-push, overwrite a remote branch, discard user changes or commit generated build output. Create a working branch if a Git repository already exists. Make small, descriptive commits after verified milestones.
11. **Continue until the available session budget is nearly exhausted or a genuine external blocker is reached.** If blocked by an unavailable GameMaker installation, missing export module, authentication or hardware-only test, finish all work that can be completed, record the blocker and leave exact next actions in `docs/EXECUTION-STATE.md`. Do not silently skip the item.
12. **Keep `docs/EXECUTION-STATE.md` current.** Create it during the first inspection pass, then update it before context/session limits and after each major milestone. It is the handoff ledger for a long-running project.

## Initial safety and inventory pass

Before implementation:

- Print the current path and inspect the directory tree.
- Find the `.yyp` project file and read its actual resource format. Do not invent a new project file if one already exists.
- Check `git status`, current branch, remotes and recent commits. Preserve dirty/untracked files.
- Inventory all sprites, audio, sequences, shaders, rooms, scripts, objects, fonts, tilesets and included files.
- Make a manifest of every image/audio file with path, dimensions/duration where available, format, file size and classification: `ORIGINAL`, `PLACEHOLDER`, `LICENSED_EXTERNAL`, or `UNKNOWN`.
- Identify the brother's actual Diamond Ace/player sprite and the enemy ship art. Preserve and reuse them where compatible; do not replace them with generated shapes.
- Identify the already-created soundtrack. Preserve it byte-for-byte; do not recompose, normalise destructively, rename destructively or overwrite it. Copy/import it into GameMaker only as required.
- Record the installed IDE/runtime versions and available export modules. Use GameMaker's own version/help/release information rather than guessing CLI switches.
- Create or update `docs/PROJECT-INVENTORY.md`, `docs/DECISIONS.md` and `docs/EXECUTION-STATE.md`.
- Establish a clean baseline build before modifying the project. If the blank project does not build, record the original error first.

---

# 1. THE PRODUCT VISION

## 1.1 The game

**OUTGUN** is a medium-fast-paced, 2D vertical arcade shoot-'em-up with cinematic presentation, precise aircraft handling, memorable enemy formations, large multi-part bosses, meaningful progression, replayable missions and a characterful radio cast.

Its foundation is the immediacy and readability of classic Raiden-style arcade shooters. It should then improve the experience with modern animation, multi-plane parallax, responsive transitions, strong audio direction, an interactive training sortie, a hangar/upgrade hub, persistent progression, local co-op support and memorable boss mechanics.

The target feeling is:

- The aircraft responds immediately and predictably.
- Shooting has weight, rhythm and satisfying impact.
- Enemies look and behave like different machines, not recoloured copies.
- The player can read an attack, understand the mistake and learn a better response.
- Bosses feel enormous, dangerous and mechanically distinct.
- The world feels alive behind the action without hiding threats.
- Music, weapon fire, explosions, animation and radio dialogue reinforce one another.
- Progress continues to feel meaningful even when a mission is failed.
- A new player understands the basics quickly; a skilled player finds optimisation, scoring routes, advanced patterns and mastery challenges.

## 1.2 Story premise — preserve this canon

The player pilots a fighter aircraft known as **Diamond Ace**. The playable pilot is **Rookie**. The real Diamond Ace pilot is dead. Rookie and a mentor/HQ contact are continuing the original pilot's mission while flying the same legendary jet. Hostile forces frequently mistake Rookie for the dead ace because they recognise the aircraft.

Story is delivered primarily through short radio/walkie-talkie conversations between Rookie and HQ, with occasional cinematic sequences and the brother's hand-drawn 2D cutscene animation.

Do not rewrite this premise. Expand it carefully through mission briefings, radio calls, enemy recognition, boss introductions and post-mission conversations. Unknown names, ranks, dates and faction details must remain editable placeholders—not falsely established canon.

The enemy confusion is a strong narrative hook. Turn it into a recurring identity/legacy motif: enemies recognise the jet, call it by the legendary pilot's name, challenge it, fear it or misunderstand Rookie's actions. Let the player gradually define what the Diamond Ace legacy means. This recognition system may alter dialogue and enemy presentation, but must never secretly change hitboxes or make combat unfair.

## 1.3 Signature identity to build into the framework

The game must not become a generic vertical shooter with a story pasted on top. Use these OUTGUN-specific pillars:

1. **The Diamond Ace legacy:** enemy recognition and HQ dialogue evolve around the legendary aircraft and Rookie's identity.
2. **The hangar as the campaign hub:** return after sorties, inspect the aircraft, improve systems, read mission intelligence, review records, select the next sortie and receive a briefing.
3. **Modular cinematic bosses:** major enemy machines have independently animated and, where appropriate, destructible components. Destroying a component changes the boss's available attacks and visual silhouette.
4. **Radio personality:** brief, context-aware, occasionally comical banter celebrates combos and memorable combat moments without distracting from survival.
5. **Music-aware presentation:** the existing soundtrack is treated as the timing foundation for transitions, animated accents, boss events and selected combat effects. OUTGUN is not a rhythm game; no required action may depend on hearing a beat.
6. **Two layers of growth:** immediate in-sortie weapon/power development and slower hangar aircraft development. Both must be satisfying without turning the game into mandatory grinding.

---

# 2. GAME FLOW AND CAMPAIGN STRUCTURE

## 2.1 Main flow

Implement a clean state-driven flow:

`BOOT → TITLE → INTRO / TRAINING → HANGAR → MISSION BRIEFING → SORTIE → MISSION DEBRIEF → HANGAR`

The flow must also support:

- Pause and resume.
- Restart current mission.
- Return to hangar after a completed or failed sortie.
- Load an unlocked mission from the hangar.
- Enter Training from the hangar at any time.
- Enter a practice encounter or boss practice without corrupting campaign records.
- Two-player local co-op selection and joining.
- Settings and control remapping from title, hangar and pause menus.
- Save/load and recovery after interruption.

Use a transition manager. Do not make room changes happen abruptly in the middle of a boss death, dialogue line or audio transition. Use fades, short camera moves, sequence-controlled animation and audio crossfades where appropriate.

## 2.2 The hangar is the mission selector

Do **not** build a generic grid of six level buttons as the only campaign interface. The hangar is the home base and the place the player returns to between missions.

The hangar must provide:

- A prominent animated Diamond Ace aircraft display/turntable.
- **Calibration Bay / Flight Upgrade Room:** inspect aircraft systems and buy or unlock upgrades.
- Mission board with unlocked sorties, mission thumbnails, threat intelligence and completion records.
- Mission briefing screen with objective, expected enemy types, commander/HQ dialogue and music identity.
- Pilot record: level, experience, sorties, kills, deaths, boss records, score, best combo and medals.
- Aircraft status: weapon, special equipment, survivability and upgrade slots.
- Training access and boss/section practice access.
- Codex/intelligence archive for encountered enemy craft, ground units, bosses and story discoveries.
- Settings, accessibility and control configuration.
- Co-op join/player status when available.

The upgrade room must have a clear visual representation of the aircraft and selected modules. Upgrade changes should visibly affect the aircraft preview when art supports it. Use clearly labelled placeholder modules until final art arrives.

## 2.3 Campaign mission set

Build a data-driven six-mission campaign. Names below are **provisional working names**, not permanent story canon; keep them in mission data so the brother can rename them without editing engine code.

| Mission | Biome / setting | Required identity | Major boss direction |
|---|---|---|---|
| 00 | Training sortie / flight range | Interactive onboarding, flight controls, weapons and survival | Practice target / non-lethal training drone |
| 01 | Oasis | Warm desert, dunes, ancient structures, palm silhouettes, sun haze and wind | **DUNE REGENT** — enormous desert assault platform with independently moving weapon emplacements |
| 02 | Snow | Frozen mountains, blizzard layers, ice cliffs, avalanche/whiteout set pieces | **WHITEOUT** — cold-weather aerial fortress or interceptor carrier with sweeping frost/ice attack geometry |
| 03 | Hills | Green highlands, valleys, bridges, forests, cloud shadows and wind | **IRON HART** — mobile armoured walker/artillery machine that uses terrain and long-range firing lanes |
| 04 | Ground Army | Mechanised front, tanks, SAM sites, artillery, transport columns and fortified roads | **WAR MAUL** — ground-force command platform with multiple independent turrets and weapon bays |
| 05 | Sea | Ocean, coastal cliffs, naval vessels, foam, spray, underwater silhouettes and tentacles | **ABYSSAL CROWN / LEVIATHAN** — colossal tentacle boss; appendages control different screen lanes and expose a central core |
| 06 | Enemy black-site / fortress / final approach | A strong final visual identity distinct from the earlier biomes; keep art and name editable | **OMEN** — flexible articulated robot with modular limbs, transforming weapon configurations and animation-triggered phase changes |

This gives six main bosses across six campaign missions, with the training encounter separate. If the story later requires a different order, mission data—not engine code—must control the order.

Each mission definition must support:

- Mission ID, display title, subtitle and briefing.
- Biome/theme ID and parallax profile.
- Scroll speed profile and optional controlled camera movement.
- Wave/spawner schedule with explicit timing or progress triggers.
- Enemy formations and ground-target sets.
- Optional mid-boss and set-piece events.
- Main boss reference and entrance sequence.
- Mission music, music BPM/offset metadata when known, and loop/cue markers.
- Radio dialogue events and their priority.
- Optional route or encounter variations.
- Mission length estimate, difficulty rating and checkpoint location.
- Completion, failure, score and reward rules.
- Practice section markers and boss phase entry points.

At least one later mission should deliberately be longer and more demanding, but it must have a meaningful midpoint checkpoint or practice access. Do not make players replay a long opening section repeatedly just to learn the last boss phase.

## 2.4 Mission pacing

Use a designed rhythm, not constant maximum projectile density:

1. Introduce the environment and a readable enemy behaviour.
2. Add a second enemy role that changes movement decisions.
3. Combine known threats in a new formation.
4. Give the player a brief breathing window and a visual landmark.
5. Introduce a set piece, miniboss or new terrain interaction.
6. Increase pressure through layered but readable combinations.
7. Provide a short boss approach/recognition moment.
8. Deliver a boss encounter with clear attack tells, phase changes and recovery windows.
9. Provide a clean result/debrief transition.

Vary intensity. Do not keep every enemy, bullet, particle and background event at full intensity continuously. Use quiet moments so dramatic moments feel dramatic.

---

# 3. PLAYER EXPERIENCE — EASY TO ENTER, HARD TO MASTER

## 3.1 Training sortie

Create an actual playable tutorial room with instructor/HQ radio prompts. The tutorial should teach through short tasks, not paragraphs.

Training modules, in order:

1. Move up/down/left/right and understand arena boundaries.
2. Fire the primary weapon while moving.
3. Hold Focus/Precision mode and see the aircraft move more slowly; briefly show its true hitbox.
4. Switch or trigger the secondary weapon.
5. Use a bomb/defensive special and see its area-clear effect.
6. Collect a weapon power-up and see the in-sortie weapon level change.
7. Read an aimed projectile and dodge it by moving away from the point at which it was aimed.
8. Read a fixed lane/fan pattern and find a safe route.
9. Destroy a turret by understanding its rotation and firing tell.
10. Fight a training boss with a weakpoint and a phase transition.
11. Finish with a score/grade explanation and access to the hangar.

Training requirements:

- It must be replayable from the hangar.
- It must be skippable after the first completion.
- Prompts must pause or slow the exercise only when necessary; never leave a tutorial overlay hiding bullets.
- Each instruction is short, concrete and tied to a visible action.
- Provide a clear fail/retry route with no campaign penalty.
- Persist tutorial completion.
- Include an optional advanced training set: streaming, focus dodging, bullet herding, turret prioritisation, laser lane escape, boss phase practice and score routes.

## 3.2 Difficulty modes

Create difficulty presets as data, not duplicated gameplay code. Use descriptive names and explain what changes.

- **CADET / PILOT ASSIST:** generous survival support, slower or less dense patterns, clearer tells and an approachable continue policy.
- **STANDARD:** intended first full campaign experience and baseline balance.
- **ACE:** faster/more layered patterns, tougher encounter combinations and greater scoring demands.
- **EXPERT / ACE TRIAL:** unlocked later; demanding authored patterns and score optimisation, not random unfairness.

The names are editable. The underlying profile must be explicit and testable. Separate leaderboards/records by ruleset where rules differ materially.

Assist options should be transparent and configurable where practical: reduced screen shake, stronger bullet outlines, high-contrast bullets, hitbox display, lower flash intensity, auto-fire, optional Focus toggle/hold, controller vibration, subtitle size, independent music/SFX/voice volumes, remapping and an optional practice-only speed control.

Do not secretly reduce difficulty based on hidden performance. If a future director adjusts encounter pressure, keep the change bounded, documented, deterministic where possible and visible as a selectable assist/ruleset. Never alter a projectile after it is emitted to make it unfairly home into the player.

## 3.3 Recoverability and frustration prevention

Implement these as core quality rules:

- After a hit, cancel or clear nearby hostile projectiles in a predictable, clearly communicated way.
- Grant a short, visible post-hit invulnerability period so one mistake does not become an unavoidable chain of deaths.
- Provide a small configurable bomb/death grace window so a bomb pressed within a few frames of a hit can save the player. Test it deliberately; do not rely on input polling order.
- Do not spawn a fast bullet directly on top of the player without a telegraph or escape route.
- Do not allow an enemy to fire while off-screen in a way that produces invisible attacks entering the arena without warning.
- Do not create patterns that completely seal the entire playfield with no safe lane.
- Avoid unannounced high-speed bullets hidden in explosions, bright pickups or background effects.
- Protect players from immediate spawn-kill loops after losing a life.
- Make death feedback clear, brief and useful. Do not cover the next attack with a full-screen effect.
- Preserve earned progression after mission failure. The player should keep a portion of mission XP/salvage or unlock progress; failure must not erase all progress.
- Continues should allow campaign progress on accessible settings. A no-continue clear can receive separate medals/leaderboard eligibility for mastery.

A failed run should still teach the player something and, where possible, leave a useful reward or clear next objective.

---

# 4. PLAYER AIRCRAFT, CONTROLS AND MOVEMENT

## 4.1 Aircraft identity

Use the brother's original Diamond Ace sprite/art if it exists in the project. Preserve its original files and create a compatible GameMaker sprite resource around it. If it is incomplete, use a clearly labelled placeholder only for missing frames/effects.

Separate:

- Rendered aircraft sprite/animation.
- Actual player collision hurtbox.
- Weapon muzzle positions.
- Thruster/engine effect anchors.
- Optional wing/engine animation pieces.
- Player state and progression data.

The visible aircraft sprite must never be used as the entire player hitbox. Keep a small, predictable hurtbox and a debug hitbox display.

## 4.2 Control scheme

Provide a fully remappable control system and sensible defaults:

| Action | Keyboard default | Gamepad default |
|---|---|---|
| Move | WASD / arrows | Left stick / D-pad |
| Primary fire | Z / Space | Face button 1 / configurable trigger |
| Focus / Precision | Shift | Shoulder button |
| Secondary weapon | X | Face button 2 |
| Bomb / Countermeasure | C | Face button 3 |
| Pause | Escape | Start |
| Optional special/Overdrive | V | Face button 4 |
| Menu confirm/back | Enter / Escape | Confirm / Cancel |

These are defaults only. Build an action-map layer so devices map to actions, not hardcoded logic in player movement or weapons. Support held, pressed and released states, input buffers for critical actions, analog dead zones and safe disconnect/reconnect handling.

Local co-op input must not assume that a controller is always slot 0 or slot 1. Detect connected devices and assign them through a player-join flow.

## 4.3 Movement model

This is a precision arcade aircraft, not a realistic flight simulator. Do **not** add floaty physics simply to make it look more realistic.

Implement:

- Consistent movement speed in all directions; normalise diagonal input.
- Immediate, predictable direction changes and stopping. Avoid noticeable player-body inertia.
- Normal movement speed and lower Focus/Precision speed.
- Independent tuning for normal speed, focus speed and (if useful) a temporary slow/overdrive state.
- Clean movement bounds inside the playfield with a small safe margin.
- Framerate-independent timing. Use one documented time model; do not mix frame-count timers with second-based timers.
- Clamp unusually large frame deltas after window focus loss or debugger pauses.
- Separate visual banking/tilt from the collision position. The sprite can smoothly bank while the true collision point remains stable.
- Optional very small engine trail/afterimage that helps motion readability without obscuring bullets.
- A debug display for movement speed, focus multiplier, current hitbox and playfield bounds.

All important movement values belong in one aircraft configuration resource. Include a short tuning guide explaining how each value changes the feel. Test movement at 30, 60, 120 and 144 FPS; identical input must not create different movement distance or projectile timing merely because the frame rate changed.

## 4.4 Player combat state machine

Use explicit states such as:

`INTRO`, `NORMAL`, `FOCUS`, `HIT`, `RESPAWN`, `BOMB`, `DEFEATED`, `EXITING`.

Use a single authority for transitions. State transitions must define what happens to movement, invulnerability, weapon firing, animation and sound. No contradictory booleans that allow the player to be simultaneously defeated and firing.

---

# 5. WEAPONS, POWER-UPS, XP AND HANGAR UPGRADES

## 5.1 Two separate progression tracks

Do not conflate temporary arcade power with persistent aircraft development.

**In-sortie power:**

- Primary weapon level, projectile spread/width and damage profile.
- Secondary weapon stock, cooldown or heat.
- Bomb/countermeasure availability.
- Temporary weapon pickups or temporary tactical boosts.
- Resets or partially degrades according to an explicit death/restart rule.

**Persistent pilot/aircraft progression:**

- Pilot experience/level earned from defeating enemies, completing objectives and finishing sorties.
- Salvage/intel/currency earned from combat and mission objectives.
- Aircraft modules improved in the Calibration Bay.
- New weapon variants, equipment slots, cosmetic parts and challenge unlocks.
- Mission records, codex entries and story unlocks.

## 5.2 XP on every kill

Every valid enemy destruction awards XP according to threat class, not a single flat value. Show a subtle XP/level meter and satisfying small feedback for kills.

Interpret “level up every time you kill” as **every kill contributes to experience**; do not cause an intrusive full-screen level-up choice for every individual enemy. A level-up occurs when the cumulative threshold is reached. Use a concise effect, sound and HUD notification. Offer tactical upgrade choices at safe wave breaks, boss intermissions or in the hangar, not in the middle of a bullet pattern.

Design XP requirements so ordinary players see progress during normal play. Do not create a deliberately tedious farming loop. A player must not need to replay the same easy wave dozens of times to unlock the next campaign mission.

## 5.3 Hangar upgrade bay

Implement a modular upgrade catalogue with editable entries and clear trade-offs. Candidate branches:

- **Engine / Mobility:** movement tuning within balanced caps, Focus precision, recovery control.
- **Primary Weapon:** damage, shot spread, firing cadence or alternate shot behaviour.
- **Secondary Weapon:** missile spread, lock-on count, penetration or cooldown.
- **Defence:** shield charge, recovery window or limited damage protection.
- **Countermeasures:** bomb capacity, effect radius or utility.
- **Support Systems:** optional drone/options, pickup magnet range or utility modules.

Avoid making permanent upgrades so strong that they trivialise the difficulty curve. Prefer small improvements, sidegrades and playstyle choices over huge multiplicative damage inflation. A newly unlocked upgrade should feel interesting, not mandatory.

Each upgrade definition needs:

- Stable ID and display name.
- Description and icon/sprite reference.
- Currency/XP cost and unlock prerequisite.
- Stat/effect changes.
- Maximum level.
- Trade-off or cap if needed.
- Preview information.
- Save key and migration/default behaviour.

Buying an upgrade must validate ownership, currency and prerequisites, then save safely. The hangar display must reflect the new configuration.

---

# 6. ENEMY FRAMEWORK

## 6.1 Enemy roles

Build an expandable roster. Every enemy must have a role and a readable tactical purpose.

Required baseline archetypes:

1. **Scout / Popcorn Fighter:** low HP, quick entrance, short burst; introduces a pattern or forces movement.
2. **Interceptor:** follows a readable swoop/curve and attempts a pass; leaves a clear exit path.
3. **Formation Fighter:** joins a lane or formation, then fires a coordinated fan/line.
4. **Bomber:** slow, high-priority target with a telegraphed payload or aimed volley.
5. **Gunship:** large target, layered firing ports, slower movement and multiple hit effects.
6. **Flanker:** enters from side lanes and pressures the player's preferred position; never traps the player without an escape.
7. **Support Carrier:** releases smaller craft or support units on clear timings.
8. **Armoured Elite:** more HP and a distinct attack purpose; not merely an inflated ordinary enemy.
9. **Turret / Cannon:** fixed or rotating ground emplacement with a separate barrel/pivot and a visible firing tell.
10. **Tank / APC:** ground unit with movement, armour, turret and optional deployment behaviour.
11. **SAM / Missile Battery:** locks a lane, paints a target or telegraphs a missile launch before firing.
12. **Artillery / Railgun:** slow charge, visible targeting line and a dangerous but avoidable attack.
13. **Mine / Drone / Hazard:** clear warning state and predictable movement or area denial.
14. **Mini-boss:** multi-step attack set and optional destructible components.

Create enemy variants through data and component configuration, not copy-pasted objects with slightly different numbers.

## 6.2 Movement profiles

Implement reusable movement behaviours:

- Straight entry and exit.
- Smooth sine weave.
- Lane-to-lane formation sweep.
- Dive/bombing run.
- Curved intercept/swoop.
- Waypoint route with easing.
- Orbit/spiral around a point.
- Side entrance and cross-screen strafe.
- Hover, aim and fire, then retreat.
- Escort/formation follow.
- Boss-linked component movement.

Every profile must be time-based and configurable. Define its start point, duration, easing, destination, speed, firing window and exit behaviour. Do not make enemies randomly accelerate toward the player with no telegraph.

## 6.3 Enemy state machine

A shared enemy controller should support clear states such as:

`SPAWN`, `ENTER`, `MOVE`, `TELEGRAPH`, `ATTACK`, `RECOVER`, `RETREAT`, `DESTROYED`.

Each enemy configuration should declare:

- ID, role, faction and display name.
- Sprite/animation set and animation triggers.
- HP, armour, score and XP reward.
- Hurtbox dimensions/shape.
- Movement profile and tuning.
- Weapon/emitter definitions.
- Attack telegraph duration and cooldown.
- Entry, exit and off-screen behaviour.
- Drop table.
- Audio profile and hit/death effects.
- Codex entry and mission tags.

Enemy reactions are important: flash or change material on hit, spawn controlled sparks, play a restrained hit sound, animate recoil and finish with a distinctive explosion/debris sequence. Do not rely on a sprite simply vanishing.

## 6.4 Turret and ground-unit mechanics

Turrets are independent functional parts, not a static gun graphic.

Implement:

- Pivot/rotation anchor separated from the base/body.
- Barrel sprite or separate child/part with its own origin.
- Configurable traverse speed, rotation limits, dead zones and target tracking.
- Optional lead-aim mode with tightly bounded prediction; use direct aim as the default for clear patterns.
- Muzzle point, firing cadence, burst count and recoil.
- Laser/paint telegraph before powerful shots.
- Separate armour/HP and optional destructibility.
- Attack state feedback: rotate, lock, charge, fire, recoil and cool down.
- Target loss and destroyed-state handling.

Never assume the player instance exists. Check for a valid active target and gracefully return to a safe idle/fire pattern when not.

## 6.5 Dynamic encounter logic

Enemies should make the player's actions matter. Efficient kills can prevent later pressure, but do not punish a beginner so severely for missing a target that the run becomes unwinnable. Design each enemy for optimal kill, normal kill and delayed kill cases.

Do not place two high-priority armoured enemies simultaneously if the player cannot reasonably understand which threat to answer first. Introduce them in a readable order, then combine them once the player understands both roles.

Use 5–7 broad movement lanes as a design aid where helpful, but never make all patterns rigid lane scripts. Leave edge escape room and avoid spawning threats directly against the playfield border.

---

# 7. PROJECTILE, EMITTER AND PATTERN ENGINE

This is a core system. Implement it as reusable GML code with documented parameters and a small catalogue of verified patterns.

## 7.1 Projectile data and lifecycle

Separate player projectiles, enemy bullets, missiles, lasers, beams and persistent hazards where their behaviours differ. Do not force every attack into one opaque generic object.

A projectile definition should support:

- Owner/team and damage target mask.
- Position, direction, speed and acceleration.
- Visual sprite/animation and trail/effect.
- Collision radius/shape.
- Lifetime and screen-bound cleanup.
- Optional homing target and bounded turn rate.
- Optional gravity/arc or curve parameters.
- Spawn effect, hit effect and impact sound.
- Pattern/emitter ID for diagnostics.
- Damage, piercing, armour interaction and score interaction.

Use centralised collision rules. Enemy bullets collide with player hurtboxes, player shots collide with valid enemy hurtboxes, and friendly fire is disabled unless a documented mechanic calls for it. Keep collision logic independent from visual sprite bounds.

## 7.2 Required pattern catalogue

Implement and test these patterns, with a named configuration and a preview/debug mode:

- Single aimed shot.
- Aimed burst with repeated cadence.
- Fixed-angle fan / spread.
- Symmetric radial ring.
- Offset radial ring.
- Rotating spiral.
- Accelerating/decelerating spiral.
- Rotating fan.
- Multi-shot aimed spread.
- Lane wall with explicit gaps.
- Sweeping arc.
- Alternating left/right wave.
- Delayed split / cluster.
- Curved or sinusoidal projectile path.
- Slow tracking missile with limited turn rate.
- Painted laser/beam with charge, active and cooldown phases.
- Rotating laser or articulated beam.
- Mine/field hazard with a telegraph and activation delay.
- Boss pattern composed from multiple emitters.

A pattern is data-driven: origin, count, angle, spread, speed, acceleration, interval, duration, rotation rate, telegraph, bullet type and seed. Use consistent angle units and central helper functions. Check GameMaker's angle conventions before coding; do not mix mathematical radians with GameMaker degree-based functions unintentionally.

## 7.3 Fairness and readability rules

- Prefer aimed, static and carefully controlled hybrid patterns. Use random variation only when its range and resulting escape space are tested.
- Never create unavoidable random spawn points on top of the player's current position.
- Telegraph dangerous lasers, artillery and screen-control attacks.
- Keep a visible safe lane or a deliberate way to create one.
- Give unusual trajectories trails or shape cues.
- Use distinct visual language for player bullets, enemy bullets, pickups, explosions and warning areas.
- Keep backgrounds predominantly mid-value; reserve strong light/dark contrast for gameplay-critical objects.
- Test the entire scene in greyscale and at reduced brightness to validate value contrast.
- Draw hostile projectiles at a depth that keeps them visible over explosions and background decoration.
- Limit decorative particles during dense patterns.

## 7.4 Performance and object lifecycle

Implement explicit capacity limits and graceful overflow behaviour. Pool high-volume bullets and short-lived effects if benchmarks show a benefit. Reset every mutable field when a pooled instance is reused; stale owner IDs, hit flags, trails or timers are unacceptable.

Do not allocate a new dynamic list/struct for every projectile every frame. Avoid full enemy scans from every bullet. Keep a clear active/inactive lifecycle and deterministic cleanup on room transitions, death, pause and game reset.

Provide a debug overlay for active bullets by team/type, emitters, enemies, bosses, pooled object count, overflow events and frame time. Add stress-test patterns with hundreds and then thousands of projectiles. The game must not crash or leak objects when capacity is reached; it must log and fall back safely.

---

# 8. BOSS ARCHITECTURE — SIX DISTINCT ENCOUNTERS

## 8.1 Shared boss director

Build a reusable boss state machine:

`OFFSCREEN → ENTRANCE → INTRO/TAUNT → PHASE_TELL → PHASE_ATTACK → PHASE_RECOVERY → PHASE_TRANSITION → DEFEATED → EXIT/SEQUENCE`.

A boss configuration must describe:

- Boss ID, name, mission and art references.
- Body sprite/animation and separate hurtbox.
- Component/weakpoint definitions.
- Phase graph and phase transition thresholds.
- Attack patterns, emitter IDs and timing.
- Movement profile and bounds.
- Telegraph and recovery timings.
- Boss health UI data.
- Music/score/sequence triggers.
- Radio dialogue and subtitle cues.
- Destruction sequence and rewards.
- Practice section markers.

Make the boss's body, limbs, turrets, armour plates, weapons, weakpoints and health components independently controllable where the design needs it. A destroyed turret should stop firing. A broken appendage should change the pattern. A disabled armour panel should reveal a readable vulnerable core.

## 8.2 Animation-triggered actions

Use GameMaker Sequences for authored boss introductions, transformations, camera moves, foreground events, explosions and music accents. Use sequence event callbacks or an explicit, documented event dispatcher to invoke gameplay events at known moments.

Never execute arbitrary strings from a data file as code. Use an allowlisted event ID such as `BOSS_REVEAL_CORE`, `OPEN_MISSILE_BAY`, `START_TENTACLE_SWEEP` or `OMEN_CHANGE_FORM`, mapped to a known function. Validate the boss/state before applying the event. If an event is triggered twice, it must not duplicate damage, spawn an extra limb or fire an attack twice.

Keep gameplay hitboxes and attack timing in the boss controller, not hidden inside an animation that can become out of sync when the frame rate changes.

## 8.3 Boss roster design

### Boss 1 — DUNE REGENT (Oasis)

A giant desert assault platform/fortress. Distinctive mechanics: side weapon emplacements, ground-to-air artillery, rotating cannon arcs, sand-haze warning zones and a central reactor/command weakpoint. Destroying side turrets should visibly remove attack lanes. Keep the first boss readable and teach component targeting.

### Boss 2 — WHITEOUT (Snow)

A cold-weather aerial fortress or heavy interceptor carrier. Distinctive mechanics: wide ice-like projectile fans, sweeping wing weapons, crosswind/visibility set pieces and a charged freezing beam with a clear warning. The snowstorm must never camouflage its dangerous bullets.

### Boss 3 — IRON HART (Hills)

A large armoured mobile walker/artillery machine. Distinctive mechanics: a long-range railgun, staggered leg/arm movement, terrain-linked firing lanes and a vulnerable power system. The player learns to read charge time, positioning and weapon-component priority.

### Boss 4 — WAR MAUL (Ground Army)

A mechanised command battleship/fortress with several turrets, missile silos and deployment bays. Distinctive mechanics: independent turret tracking, staggered launch windows, destructible weapon bays and a multi-stage core. It tests the player's ability to prioritise components without filling every lane at once.

### Boss 5 — ABYSSAL CROWN / LEVIATHAN (Sea)

A colossal sea creature or tentacled machine rising from the ocean. The tentacles are individual animated components that sweep, slam, coil, block selected lanes and create temporary safe/unsafe areas. Their attack tells must be unmistakable. Destroying or stunning specific tentacles changes the boss's available patterns and exposes the core. Water, spray, foam and reflections should create spectacle without obscuring projectiles.

### Boss 6 — OMEN (Finale)

OMEN is the special flexible robot. Do not implement it as a conventional static boss with a few sprite swaps. Build it from articulated modules: torso, head/core, arms, shoulder weapons, optional tail/extra limbs and interchangeable weapon units. Its shape and attack geometry change between clearly telegraphed forms. It may fold, extend, reconfigure, split its weapon arcs or change from mobile close-range pressure to long-range area denial.

OMEN's transformations must be animated through sequences and component state, with explicit gameplay trigger IDs. Its form changes should be spectacular but fair: announce the transformation, clear or resolve existing attacks consistently, establish the next attack lanes and give the player a moment to understand the new geometry. Never track the player with an unavoidable attack during a transformation.

All six bosses need functional placeholder implementations and a boss-practice entry point. Use provisional names in data so the artist/writer can rename them later.

---

# 9. VISUAL PRESENTATION — LAYERED 2D WITH CONTROLLED 2.5D

## 9.1 Visual target

OUTGUN remains a 2D-art game. The objective is not to replace the brother's art with generic 3D models. Use high-quality 2D sprites, hand-drawn animation, tiles, particles, shader work and layered movement to create depth and cinematic scale.

The player and important enemies must remain the visual focus. A 2.5D effect is successful only when it improves depth, atmosphere and impact without reducing combat readability.

## 9.2 Camera and playfield

Use a vertical arcade playfield with a stable aspect ratio, surrounded by a modern HUD layout on widescreen displays. Recommended starting design: a **3:4 logical playfield** with a flexible side-information/HUD area inside a 16:9 presentation. This is a design target, not a reason to distort existing art. Verify the exact room/camera dimensions against the artwork and project before locking them.

Support at minimum:

- 16:9, 16:10 and 21:9 desktop aspect ratios.
- 4:3 fallback with a sensible HUD overlay/compact layout.
- Windowed, borderless and fullscreen settings where supported.
- Integer/pixel-friendly scaling for pixel-art assets and high-quality scaling for non-pixel art.
- Camera-safe boundaries and a clearly defined world-to-GUI transform.
- No stretched aircraft, bullets, UI or cutscene artwork.

Keep the core playfield dimensions stable; adapt side panels and surrounding UI, not the gameplay geometry.

## 9.3 Parallax and layer stack

Build each biome from a reusable layer profile. A typical layer stack:

1. Sky/space/atmospheric base.
2. Distant clouds, stars, haze or mountain silhouettes.
3. Far terrain or skyline.
4. Mid-distance terrain, cliffs, architecture or ocean horizon.
5. Main scrolling ground/ocean/terrain tilemap.
6. Tactical foreground set pieces and destructible scenery.
7. Aircraft/environment interaction particles.
8. Player/enemies/ground units.
9. Player and enemy projectiles/lasers.
10. Foreground debris, spray, snow, leaves or light haze—kept visually restrained.
11. UI/HUD.

For every layer, expose an editor-friendly configuration for speed multiplier, X/Y offset, repeat/tile mode, opacity, scale, tint, depth, shader/filter and optional movement event. Use a central camera/scroll controller so layer timing does not drift independently.

Ensure seamless vertical scrolling: no one-frame gaps, visible texture seams, snapping or sudden layer resets. Test the repeat boundary at multiple resolutions and scroll speeds.

## 9.4 Biome-specific visual language

- **Oasis:** warm sun, distant dunes, palm silhouettes, ruins, heat shimmer, dust devils, long shadows and layered sand movement.
- **Snow:** layered ridgelines, drifting snow at several depths, wind gusts, ice reflections and controlled whiteout moments. Bullets must remain distinct from snow particles.
- **Hills:** layered green ridges, forests, bridges, rock faces, moving cloud shadows, valley depth and occasional falling debris.
- **Ground Army:** long industrial routes, roads, convoys, bases, radar arrays, artillery, smoke columns, wreckage and ground explosions.
- **Sea:** moving ocean tilemap/texture, layered waves, foam, reflections, spray, distant ships, wakes, underwater shadows and tentacle silhouettes.
- **Final approach:** a distinctive enemy stronghold/black-site atmosphere with controlled industrial lights, machinery and a dramatic reveal for OMEN.

## 9.5 Tilesets and ground

Use GameMaker Tilesets and Tile Map layers for large scrolling terrain where appropriate. Create coherent tileset families, consistent tile dimensions per family, clean margins, animation rules and documented collision/visual roles. Use automatic tiling/brush tools where they genuinely speed authoring.

Build placeholder tile families for sand, snow/ice, grass/rock, military roads/metal, ocean/foam and final-fortress panels. Do not force one global tile size on original art that uses a different design grid. Generate a tileset contact sheet and a short guide showing which tiles are edges, corners, transitions, animated water or decoration.

## 9.6 2.5D effects and shaders

Create reusable, optional shader/effect modules such as:

- Atmospheric colour gradient and distance tint.
- Parallax depth tint/fog.
- Heat distortion for the Oasis.
- Wind/snow overlay for the Snow mission.
- Water ripple/reflection distortion for the Sea mission.
- Controlled glow and bloom-style accents for weapons and boss cores.
- Hit flash, damage tint, shield shimmer and brief impact distortion.
- Background light rays or volumetric-looking streaks using 2D layers.
- Optional CRT/scanline filter only as an accessibility-safe optional presentation choice, never the default.

Do not run expensive multi-pass effects over the whole screen unless profiling demonstrates the cost is acceptable. Reuse surfaces safely, handle surface loss/recreation, and provide a no-shader fallback. Never make bullet visibility depend on a shader working.

## 9.7 Particles and impacts

Create a reusable effect catalogue: muzzle flashes, engine flames, trails, small hit sparks, medium/large explosions, smoke, debris, ground impact, shield impact, bullet trails, snow, sand, sea spray, tentacle impact, boss core energy and mission transition effects.

Prefer GameMaker's Particle System assets/editor for authored reusable effects. Use code-driven effects where gameplay needs dynamic parameters. Cap emitters and particles and reduce cosmetic particles automatically during heavy combat. Critical telegraphs and bullets are never culled to preserve decorative effects.

---

# 10. ANIMATION AND CINEMATIC SYSTEM

## 10.1 Replaceable animation workflow

The brother will produce the 2D aircraft/enemy artwork and animated cutscenes. Build around replaceable sprite/sequence resources.

Create an art-swap guide that specifies:

- Required sprite names and folder locations.
- Frame naming/order convention.
- Expected frame dimensions and origin/anchor.
- Animation speed/rate and loop behaviour.
- Collision/hurtbox data kept separate from artwork.
- Layer/order and blend mode requirements.
- Transparency and export format.
- Whether an asset is a complete animation, a modular body part or a single still.
- Which sequence/animation events are gameplay-critical.

Never hardcode gameplay logic to sprite frame numbers. Use semantic animation events such as `MUZZLE_FLASH`, `WING_OPEN`, `CORE_EXPOSED`, `TENTACLE_SLAM` and `OMEN_TRANSFORM_COMPLETE`.

## 10.2 Cutscenes

Support short in-engine Sequence-based scenes and externally authored 2D animation clips/slideshows. Create a cutscene player that can:

- Display a sequence, image sequence or supported video format after checking the target/runtime capabilities.
- Play the correct music/audio cue.
- Display subtitles and speaker labels.
- Skip only when allowed by story settings.
- Resume the campaign at the correct state.
- Avoid double-triggering unlocks or dialogue if replayed.

Provide placeholder intro, mission transition, boss reveal and ending sequence slots. The brother must be able to replace the animation without editing mission logic.

---

# 11. STORY, RADIO AND COMEDIC COMBO DIALOGUE

## 11.1 Radio system

Create a data-driven comms system with speaker, message, subtitle, trigger, priority, delay, duration, audio reference and cooldown.

Trigger categories:

- Mission start/briefing.
- First encounter with a new enemy family.
- Enemy recognition of the Diamond Ace aircraft.
- Mid-boss appearance and defeat.
- Main boss arrival, phase shift and defeat.
- Objective completed or failed.
- Player damaged, low health or recovered.
- Combo/kill-chain milestones.
- Pickup/weapon level-up.
- Mission midpoint and debrief.

## 11.2 Comedic, timed combo chatter

Create a contextual radio-banter system. At combo milestones or special kill chains, Rookie/HQ may exchange short, funny lines. The humour should feel characterful and timed—not a random joke every few seconds.

Rules:

- Trigger only at defined combo thresholds or memorable combat events.
- Use per-speaker and per-category cooldowns.
- Prevent duplicate lines and queue overflow.
- Never allow low-priority comedy to interrupt a boss telegraph, damage warning, critical story line or player death/recovery event.
- If the player is in an intense pattern, delay or discard the banter rather than covering the screen with text.
- Use concise radio captions, maximum two short lines on screen at once.
- Add a setting for `FULL`, `REDUCED` and `OFF` banter.
- Include editable placeholder line data, not hard-coded strings spread through enemy scripts.
- Avoid voice acting assumptions. Support text-only radio now and optional recorded voice assets later.

## 11.3 Story triggers and state safety

A story event must be idempotent: replaying a mission or reloading a save cannot accidentally award the same permanent unlock twice. Keep dialogue state separate from combat state. All story/cinematic events must have a tested fallback if a sequence, sound or sprite is missing.

---

# 12. SOUNDTRACK, SFX AND MUSIC SYNCHRONISATION

The user has already created the soundtrack. Treat those audio files as original project assets and preserve them.

## 12.1 Audio architecture

Build an audio manager with:

- Music, SFX, radio/voice and UI buses or equivalent gain categories.
- Independent volume settings and a master volume.
- Mission music selection and safe transitions.
- Optional music crossfade between two valid tracks.
- Audio-group loading/unloading for mission assets where helpful.
- Sound priority and concurrency limits.
- Boss-intro and radio-dialogue ducking.
- Pause/resume behaviour that does not desynchronise visuals.
- Missing-asset handling and debug logs.

Use GameMaker Audio Groups and Audio Buses/effects where appropriate. Check that a group is loaded before playing its sounds; handle invalid sound-instance IDs.

## 12.2 Music metadata and beat clock

Create `data/audio/music_manifest.csv` or an equivalent documented data source with:

- Track ID and mission assignment.
- Source file and imported GameMaker sound asset.
- Duration.
- BPM only when verified from the actual track.
- Beat/bar count where known.
- Beat offset/start offset.
- Loop start and loop end markers.
- Intro/transition/coda markers.
- Optional cue points for boss arrival, phase changes or set pieces.

Do not guess a track's BPM. Analyse the actual file or leave the BPM unset and use manually authored cue markers.

For beat-aware visual timing, use the actual playing music instance's playback position as the master reference (GameMaker provides track-position functions). Do not rely on a free-running timer that slowly drifts away from the music. Recalculate the current beat from track position, BPM and offset. Handle pauses, loop points, crossfades and track restarts explicitly.

Music sync should power optional visual pulses, UI accents, engine/light accents, combo celebration and selected cinematic cues. **No critical dodge window, projectile speed, boss hitbox or required player input may be tied to an audio beat.**

## 12.3 SFX hierarchy

Create distinct sound profiles for:

- Primary weapon and focused/alternate fire.
- Secondary weapon/missile launch and impact.
- Bomb/countermeasure.
- Small, medium, large and boss explosions.
- Enemy hits, armour breaks and component destruction.
- Player damage, shield, recovery and low-health warning.
- Turret rotation/lock/charge/fire.
- Laser warning/active/cooldown.
- Pickups, XP, level-up, upgrade purchase and unlock.
- UI navigation/confirm/back.
- Radio static, radio start/end and dialogue stingers.
- Environmental wind, snow, rain, ocean, machinery and distant combat.

Use layered impact sounds sparingly and with priority. Critical player feedback must not be voice-stolen by low-priority ambience.

---

# 13. LOCAL CO-OP AND PLATFORM FEATURES

## 13.1 Co-op foundation

Implement **two-player local co-op** as the first supported co-op model. The core architecture must support two independent player data/input records even when only player one is active.

Co-op requirements:

- P1 and P2 have independent movement, weapon state, hitbox, lives/status, score contribution and effects.
- A shared vertical playfield and camera with no split-screen by default.
- Both players remain inside a readable shared arena. Avoid allowing one player to drag the camera so far that the other is unfairly trapped.
- Rebalance projectile count, boss targeting and enemy pressure for two players. Do not simply double every enemy's HP.
- Distinguish player projectile colours and UI identity.
- Define shared versus individual bombs, pickups, lives and continue rules. Default to a simple, clearly explained team resource model with an option to alter it later.
- Support P2 joining at hangar/mission start and a clearly controlled mid-mission join policy.
- Handle controller disconnect, reconnect and device reassignment safely.
- Never let a player disconnect crash or corrupt the other player's run.
- Add optional team-combo/revive/assist mechanics only if they do not interrupt precision movement.

## 13.2 Online play scope

Do not build custom online netcode before the local game is stable. Use the architecture so an online adapter can be added later, but do not pretend local co-op is native online multiplayer.

For a Steam release, investigate **Steam Remote Play Together** as the low-risk way to let remote friends participate in a local co-op session. It streams the host's game and forwards additional player input; the game still needs genuine local multi-controller support. Keep Steam APIs behind a small platform adapter. Native online multiplayer, rollback, matchmaking and server-authoritative progression are separate future scope and must not block the first campaign framework.

## 13.3 Steam achievements and leaderboards

Create an achievement definition catalogue and platform adapter with a safe offline/local fallback. Do not hardwire Steam API calls into mission or boss logic. Implement achievement triggers for campaign progress, boss victories, training, score milestones, combo milestones, no-continue clears and co-op events.

Do not claim achievements are uploaded unless a valid Steam integration and App ID are configured and tested. Add leaderboards only after scoring rules, difficulty separation and anti-cheat considerations are documented.

---

# 14. SAVE DATA AND PROGRESSION INTEGRITY

Build a versioned save system. Save format must be documented in `docs/SAVE-SCHEMA.md`.

Save at least:

- Save schema version.
- Campaign mission unlocks/completion.
- Story/briefing/cutscene unlock state.
- Tutorial completion and training records.
- Pilot XP/level.
- Currency/salvage/intel.
- Aircraft upgrade ownership/levels.
- Unlocked weapon variants/equipment.
- Mission high scores, grades, medals, times and best combo.
- Boss practice unlocks and best phase/clear records.
- Player settings, volume, controls and accessibility options.
- Co-op settings/records where applicable.
- Statistics/codex entries.

Implementation requirements:

- Validate file existence and content before parsing.
- Validate required fields, types, numeric ranges and array sizes.
- Supply safe defaults for absent/invalid optional values.
- Keep a schema version and explicit migration functions.
- Write to a temporary file first; verify it; then replace the active save using a tested platform-safe strategy.
- Keep a backup/previous save when safe.
- Recover gracefully from a corrupt or partially written save.
- Do not use untrusted raw data as executable code.
- Save after important campaign/upgrade events, not just at game exit.
- Provide a debug-only save inspector/reset tool; make reset confirmation explicit.
- Ensure practice/debug runs do not corrupt permanent campaign records.

Test first boot, normal save/load, interrupted write recovery, malformed JSON, missing keys, future schema version, migration from an older schema, full disk/error handling where testable and two-player state persistence.

---

# 15. HUD, MENUS AND USABILITY

## 15.1 In-sortie HUD

Build a clean arcade HUD with:

- Score and multiplier/chain.
- Pilot XP progress and level-up feedback.
- Lives/continues/assist status as applicable.
- Primary weapon level and secondary weapon status.
- Bomb/countermeasure stock.
- Special/Overdrive meter if implemented.
- Boss health and phase information when relevant.
- Player-two indicators during co-op.
- Clear objective/mission status.

HUD elements must be anchored to the safe area, not hardcoded to one absolute window size. Keep text readable, use consistent iconography, avoid filling the action area and never obscure bullets or the true player position.

## 15.2 Menus

Create working menus, not static screenshots:

- Title/main menu.
- Training selection.
- Hangar and aircraft inspection.
- Calibration Bay upgrades.
- Mission board and briefing.
- Mission debrief/results.
- Pause menu.
- Settings/accessibility.
- Controls remapping.
- Records/codex.
- Co-op join/player status.
- Save/load/profile recovery feedback where appropriate.

Use GameMaker's UI Layers/Flex Panels where supported by the selected LTS version, and Sequences/room layers for authored presentation. Keep menu navigation keyboard- and controller-accessible. Add visible focus states and predictable back/cancel behaviour.

---

# 16. PROJECT ARCHITECTURE AND RESOURCE ORGANISATION

First inspect the current project. Adapt the following layout to existing conventions rather than creating duplicate folders or breaking resource paths.

Suggested logical organisation:

```text
Outgun_Framework/
  Outgun_Framework.yyp                 # preserve the real existing project file
  README.md
  CHANGELOG.md
  .gitignore
  assets/
    art_source/                         # untouched originals from the artist
    audio_source/                       # untouched soundtrack/SFX originals
    placeholders/                       # clearly marked replaceable prototype art
    external/                           # licensed third-party source assets only
    manifests/                          # source, licence and replacement manifests
  data/
    aircraft/
    weapons/
    upgrades/
    enemies/
    patterns/
    bosses/
    missions/
    dialogue/
    audio/
    difficulty/
  docs/
    PROJECT-INVENTORY.md
    DECISIONS.md
    EXECUTION-STATE.md
    GAME-DESIGN.md
    TECHNICAL-ARCHITECTURE.md
    PLAYER-EXPERIENCE.md
    ART-ASSET-SWAP-GUIDE.md
    ENEMY-CATALOG.md
    BOSS-BIBLE.md
    MISSION-BIBLE.md
    COMBAT-PATTERNS.md
    AUDIO-DIRECTION.md
    SAVE-SCHEMA.md
    TEST-PLAN.md
    PERFORMANCE.md
    TROUBLESHOOTING.md
  scripts/
    core/
    state/
    input/
    movement/
    combat/
    projectiles/
    emitters/
    enemies/
    bosses/
    missions/
    progression/
    hangar/
    saves/
    dialogue/
    audio/
    presentation/
    ui/
    platform/
    debug/
  objects/
    bootstrap/
    managers/
    player/
    enemies/
    bosses/
    projectiles/
    pickups/
    effects/
    ui/
    dialogue/
  rooms/
    boot/
    title/
    training/
    hangar/
    missions/
    tests/
  sequences/
    cutscenes/
    bosses/
    transitions/
  shaders/
  tilesets/
  fonts/
  sounds/
  sprites/
  tests/
  tools/
```

Actual GameMaker resource folders may need to be named and structured according to the existing `.yyp` format. Do not blindly create this exact tree if the current project already has a valid structure.

## 16.1 Naming rules

Use predictable lower-case snake_case names and standard resource prefixes:

- Objects: `obj_...`
- Scripts/functions: `scr_...` or the current project's established script naming convention.
- Sprites: `spr_...`
- Rooms: `rm_...`
- Sequences: `seq_...`
- Sounds: `sfx_...` and `mus_...`
- Shaders: `shd_...`
- Tilesets: `ts_...`
- Fonts: `fnt_...`
- Data IDs: stable lower-case strings, never display labels used as save keys.

Keep stable IDs separate from visible display names so the artist/writer can rename a boss or mission without breaking save data.

## 16.2 Script responsibilities

Create a small number of clearly owned managers/controllers:

- Bootstrap/application manager.
- Global state/room transition manager.
- Input/action-map manager.
- Camera/playfield manager.
- Combat director/wave spawner.
- Projectile pool/manager.
- Audio manager/music clock.
- Dialogue/radio manager.
- Save/progression manager.
- UI/settings manager.
- Debug/performance manager.

Avoid a single god object. Each system must have one authority for its state, an explicit API and a cleanup path.

## 16.3 Data-driven authoring

Use structs/arrays and validated configuration assets for missions, enemy variants, bullet patterns, bosses, upgrades and dialogue. Keep content separate from lifecycle/engine code. The brother should be able to tune an enemy's HP, movement speed, firing interval, projectile speed, reward and animation references without changing shared logic.

Use JSON for persisted save data and where it makes authoring easier; use native GML structs/resources where that is simpler and safer in GameMaker. Validate every external/config data field before use. Do not parse a configuration on every Step event; load and validate it once, cache the resulting data and reuse it.

---

# 17. PLACEHOLDER AND EXTERNAL ASSET PIPELINE

## 17.1 Preserve and classify

The project may already contain the brother's original player ship and an enemy ship. Identify them, preserve the source files and integrate those exact assets where possible. Do not overwrite them with a new placeholder or rename their source files without a safe copy.

Everything else may currently be a placeholder, but verify this by inspecting the actual files. Create an asset manifest with `ORIGINAL`, `PLACEHOLDER`, `LICENSED_EXTERNAL` or `UNKNOWN` status.

## 17.2 Placeholder generation

For missing art, create clean, purposeful placeholder sprites rather than plain coloured rectangles. Each placeholder should have a recognisable silhouette and role:

- Diamond Ace aircraft (only if the original is missing/incomplete; otherwise use the original).
- Enemy scout, interceptor, bomber, gunship and carrier.
- Ground turret, tank, SAM launcher, artillery and convoy.
- Each of the six boss silhouettes and modular components.
- Primary/secondary projectile, enemy bullet shapes, missiles, lasers and warning markers.
- Pickups, bomb, shield, XP/level-up marker and upgrade modules.
- Explosion, hit spark, smoke, engine trail and environmental effect sheets.
- Biome background and tile placeholders for Oasis, Snow, Hills, Ground Army, Sea and final fortress.
- HUD/menu icons and character/radio portraits if no supplied art exists.

Placeholder art must be clearly labelled in names and in the asset-swap guide. Use a coherent provisional art direction, crisp outlines, transparent backgrounds and correct origins. Do not mistake a placeholder for final art in documentation or screenshots.

If a suitable legally reusable asset is downloaded, record its exact source URL, creator, licence, date accessed, modifications and required attribution in `assets/manifests/ASSET-LICENSES.md`. Do not assume an asset is free because a search result says “free”. Do not use ripped Raiden sprites, logos, soundtrack, characters or proprietary assets. References are for design analysis, not copying.

Generate a contact sheet showing every placeholder and its resource name. Add a replacement table that maps each placeholder resource to its expected final asset.

## 17.3 Asset replacement must not require code edits

Use stable sprite/resource references and a central asset mapping/config layer. Do not make game logic depend on animation frame numbers or source-file names. Keep collision shapes, hurtboxes, emitter anchors and gameplay tuning in dedicated configuration so a new sprite can be imported without changing combat logic.

---

# 18. CODE QUALITY, ROBUSTNESS AND PERFORMANCE

## 18.1 GML style

- Use clear descriptive names and consistent formatting.
- Document every public script/function with purpose, parameters, return value and side effects.
- Explain formulas, angle math, pooling, timing, collision and state transitions in `//` comments.
- Keep all tuning values grouped and labelled with units.
- Use enums/state constants rather than unexplained numeric state values.
- Validate array bounds, struct fields, instance references and asset references before access.
- Check that target instances/resources still exist before using cached references.
- Avoid duplicate systems and dead scripts.
- Keep functions focused and short enough to understand.
- Never hide an error with an empty catch/fallback. Log it and use a safe fallback.

## 18.2 Timing and simulation

Use seconds-based timing driven by GameMaker's frame delta (with a documented upper clamp) for movement, animation state timing, waves, cooldowns and projectile lifetime. Avoid timers that advance once per rendered frame unless they are deliberately frame-domain and documented. Test after pausing, losing window focus, changing resolution and resuming.

If deterministic fixed-step simulation becomes necessary for reproducible replays or future netcode, introduce it as a deliberate, measured design with interpolation—not as an accidental mixture of two clocks. Local co-op does not require a rollback system.

## 18.3 Memory and lifecycle

- Define ownership for every DS structure, surface, buffer, audio instance and sequence handle.
- Destroy/free resources deterministically when ownership ends.
- Avoid repeated allocation in high-volume loops.
- Reset pooled instances completely.
- Clear room-specific emitters and projectiles during transitions.
- Handle a missing/destroyed player, enemy, boss part, camera or audio instance safely.
- Bound arrays, queues, dialogue lines, effects and object pools.
- Provide a visible/logged fallback on capacity exhaustion.

## 18.4 Performance goals

Target stable 60 FPS as a minimum baseline on a normal modern desktop, with headroom for 120/144 Hz output where hardware allows. Do not promise a fixed benchmark without testing on the actual machine.

Profile:

- Dense projectile patterns.
- Two-player combat.
- Six boss components active simultaneously.
- Maximum particle load.
- Full parallax/shader effects.
- Audio/dialogue overlap.
- Long mission runtime and repeated room transitions.

Create a performance document with measured object counts, bullet counts, frame-time samples and known bottlenecks. Prefer simple collision masks and clear broad-phase filtering. Do not trade away collision accuracy to inflate an FPS number.

---

# 19. TESTING AND ACCEPTANCE CRITERIA

A milestone is not complete just because code was written. Run the project and verify it. Create a test room/harness and automated self-test scripts where practical. For every result, distinguish automated pass, manual pass, not tested and blocked.

## 19.1 Build/project integrity

- Existing `.yyp` opens in the selected GameMaker LTS without missing resource errors.
- No broken resource references or duplicate asset IDs.
- Clean build with no new warnings/errors attributable to this work.
- Project launches from its actual boot room.
- Source files and resource metadata are consistent.

## 19.2 Player and controls

- Eight-direction movement is normalised.
- Normal and Focus speeds are consistent and documented.
- No accidental inertia or frame-rate-dependent movement.
- Bounds are correct at all supported aspect ratios.
- Input mapping works for keyboard and gamepad.
- Disconnect/reconnect does not crash.
- Hitbox is smaller than visual aircraft and debug-visible.
- Bomb grace, post-hit invulnerability and respawn are tested.

## 19.3 Enemies, bullets and emitters

- Each movement profile plays, exits and cleans up correctly.
- Turret barrel aims from the actual muzzle pivot and obeys traverse limits.
- All listed pattern types produce expected geometry.
- Aimed shots use the intended player position at emission time.
- Static patterns are repeatable when seeded.
- Laser warning/active/cooldown states are visible and correct.
- Enemy bullets do not collide with the wrong team.
- Off-screen cleanup and pool reuse do not leak stale state.
- High-density stress tests reach the documented caps safely.

## 19.4 Bosses

- All six bosses can enter, transition phases, attack, recover and be defeated.
- Each major component/weakpoint behaves independently where designed.
- Animation events trigger once and at the correct time.
- Missing sequence/audio/sprite references fall back safely.
- OMEN changes form and attack geometry correctly.
- Tentacle segments/limbs use valid collision and animation state.
- Practice mode can start at documented boss phases.

## 19.5 Campaign and progression

- Training can be started, completed, skipped and replayed.
- Hangar displays mission unlocks correctly.
- Mission briefing launches the intended mission.
- Completion and failure return to the correct hangar state.
- XP is granted once for each valid kill.
- Level thresholds and upgrade costs are correct.
- Upgrades validate prerequisites/currency and persist.
- Failure retains the documented amount of progress.
- No duplicate unlock/reward on reload or replay.

## 19.6 Audio, dialogue and visual transitions

- Music begins at the intended point and stays in sync with cue markers.
- Beat-based visuals calculate from audio playback position, not a drifting timer.
- Pause/resume and loop points do not break sync.
- Music/SFX/dialogue volumes are independent.
- Important SFX are not stolen by ambience.
- Dialogue respects priority, cooldown and two-line display cap.
- Combo banter does not cover telegraphs or critical dialogue.
- Layer repeat boundaries show no seams.
- Shaders/surfaces recover safely after a lost surface or unsupported effect.
- Reduced-flash/reduced-shake settings work.

## 19.7 Co-op and save tests

- P1 and P2 can join and play at once.
- Both players have correct controls, projectiles and UI identity.
- Pressure/HP/score do not become unfair merely because P2 joins.
- Disconnecting P2 does not affect P1's state.
- Save/load works for new, normal, malformed and migrated saves.
- A failed or interrupted save does not destroy the previous valid save.
- Debug/practice state does not corrupt campaign records.

## 19.8 Visual/performance review

Capture screenshots or recordings of:

- Title screen.
- Training room.
- Hangar and Calibration Bay.
- Oasis gameplay with visible parallax.
- Each biome placeholder.
- Enemy formations and ground turrets.
- Each boss and OMEN transformation.
- Co-op HUD.
- Mission debrief.

Inspect at 16:9, 16:10, 21:9 and 4:3 where possible. Check bullet readability in normal view, greyscale, reduced brightness and with effects lowered. Run long-session and heavy-projectile tests. Record measured frame times and active object counts.

---

# 20. IMPLEMENTATION MILESTONES — KEEP MOVING

Do the milestones in this order. Do not spend the entire session writing docs before making a playable game. The docs explain the actual implementation and must be updated as it changes.

## M0 — Inspect, preserve and establish baseline

**Deliver:** inventory, Git status/backup, version record, baseline build report, project decisions and `EXECUTION-STATE.md`.

**Exit criteria:** original art/music preserved; project opens/builds or original failure documented; no user work overwritten.

## M1 — Foundation and application shell

Implement:

- Boot and global state flow.
- Room transition/fade manager.
- Input/action map.
- Camera/playfield dimensions and GUI scaling.
- Main menu, hangar shell, pause/settings shell.
- Save system v1 with schema version/defaults/recovery.
- Audio manager and volume settings.
- Debug overlay and logging.

**Exit criteria:** launch → title → hangar → training shell → back; settings persist; new/corrupt save fallback works; build succeeds.

## M2 — First playable vertical slice

Implement a working, enjoyable Oasis slice:

- Original Diamond Ace art if available; otherwise clearly labelled placeholder.
- Precise movement and Focus.
- Primary/secondary weapons, bomb and pickup.
- Player hurtbox, damage, death recovery and lives.
- Reusable enemy state/movement framework.
- At least five enemy roles and one ground turret/tank.
- Spawner/wave schedule.
- Pattern/emitter engine with the required basic patterns.
- Parallax Oasis background using layers/tilemaps.
- Particle/effect catalogue starter set.
- Music and SFX manager using the supplied soundtrack.
- Radio dialogue and at least one contextual combo line.
- Training sortie with core lessons.
- DUNE REGENT boss with at least two clear phases and a weakpoint.
- Mission results and return to hangar.

**Exit criteria:** a fresh player can launch, learn, play, defeat or fail the boss, receive a debrief, return to the hangar and save progress. This slice must feel responsive and visually readable, not merely technically connected.

## M3 — Campaign and progression framework

Implement:

- Six mission data definitions and room/scene scaffolds.
- Six distinct boss controllers/configurations.
- Mission board/briefings.
- Pilot XP and per-kill rewards.
- Calibration Bay and upgrade catalogue.
- Score/combo/medal/mission grade systems.
- Mission and boss practice entry points.
- Story and radio event definitions.
- Save schema expansion and migrations.

**Exit criteria:** all six missions can be selected/unlocked through the hangar flow and can launch into their own distinct, playable placeholder encounter. All six boss frameworks have unique phase/pattern/component behaviour—not six clones with different names.

## M4 — Presentation and audio integration

Implement:

- All biome parallax/tiling profiles.
- 2.5D layer/filter/shader effects and safe fallbacks.
- Boss entrance/phase/defeat sequences.
- Cutscene player and image-sequence placeholders.
- Music manifests, loop/cue markers and beat-clock sync.
- Radio banter priorities/cooldowns and subtitles.
- Expanded hit, explosion, environmental and weapon effects.
- Full HUD and hangar/mission UI.

**Exit criteria:** mission transitions, boss sequences, music and gameplay events remain coherent under pause/resume and multiple frame rates. Critical bullets remain readable.

## M5 — Co-op, Steam adapter and production polish

Implement:

- Two-player local co-op.
- Co-op balancing and UI.
- Platform/Steam adapter for achievements and Remote Play Together investigation/integration as available.
- Expanded control remapping and accessibility options.
- Performance caps/profiling and stress tests.
- Asset swap guide and contact sheets.
- Finalised documentation and test matrix.

**Exit criteria:** stable local co-op, safe fallback without Steam services, no critical save/input/projectile bugs, documented unresolved platform-specific items.

## M6 — Release-readiness pass

Perform a full source review, test every route and error case, remove unused/deprecated code, verify resource references, review licenses, capture evidence, build a release candidate and update `CHANGELOG.md`, `README.md` and `docs/EXECUTION-STATE.md`.

Do not label this milestone complete until actual GameMaker build/run evidence exists. A visually impressive mockup is not a release candidate.

---

# 21. GITHUB AND LOCAL REPOSITORY POLICY

The user asked for GitHub to be updated where appropriate. First inspect the current project remote and authenticated GitHub state.

- If the local project already has a remote, verify that it is the correct OUTGUN repository before pushing.
- Do not push OUTGUN files into an unrelated shambrain repository.
- A check of the connected `shambrain` repository list on 3 November 2026 did not find an `Outgun_Framework` repository. Recheck from the actual machine because local/remotely-created state may have changed.
- If no OUTGUN remote exists and GitHub CLI is authenticated with permission to create repositories, create a **private** `shambrain/Outgun_Framework` repository by default, then push the verified project branch. Do not make the project public without an explicit instruction.
- If repository creation is unavailable, keep the complete work safely in the local folder, initialise/maintain Git locally, and document the exact commands required to connect it later.
- Never force-push or overwrite a remote history. Never commit API keys, Steam App IDs/secrets, private keys, machine-specific paths or build caches.
- Add a useful README, `.gitignore`, milestone tags and concise commits. Do not commit temporary exports, caches, large generated builds or duplicate backups.

---

# 22. REQUIRED FINAL HANDOFF

At the end of the available implementation session, report:

1. Exact GameMaker IDE/runtime version used.
2. Actual project file and branch.
3. Files/resources created or materially changed.
4. Existing original assets preserved and which were integrated.
5. Placeholder assets generated and how to replace each category.
6. Working gameplay systems, clearly separated from scaffolds.
7. Exact six-mission/six-boss implementation status.
8. Training, hangar, upgrade and save-system status.
9. Co-op status and whether it is local, Remote Play Together tested, or merely prepared.
10. Build command and exact result.
11. Test count/results and any manual tests not performed.
12. Performance measurements and the test conditions.
13. Git branch, commits and remote push result—or why the work remains local.
14. Known bugs, technical limits and external blockers.
15. The next three highest-value tasks.

Update `docs/EXECUTION-STATE.md` with the same evidence. Never describe a system as complete when it is a placeholder, stub or untested path.

---

# 23. RESEARCH BASELINE AND DESIGN REFERENCES

Use the following sources to inform implementation. Recheck official GameMaker documentation against the actual IDE/runtime before using a function or migrating project resources.

## Engine and authoring tools

- GameMaker release notes: https://releases.gamemaker.io/
- GameMaker LTS 2026.0 feature overview (21 May 2026): https://gamemaker.io/en/blog/lts-2026-release
- GameMaker 2026.0 release notes: https://releases.gamemaker.io/release-notes/2026/0
- Room Editor: https://manual.gamemaker.io/lts/en/The_Asset_Editors/Rooms.htm
- Layer types and properties: https://manual.gamemaker.io/lts/en/The_Asset_Editors/Room_Properties/Layer_Properties.htm
- Sequence Editor: https://manual.gamemaker.io/lts/en/The_Asset_Editors/Sequences.htm
- Gamepad input: https://manual.gamemaker.io/lts/en/GameMaker_Language/GML_Reference/Game_Input/GamePad_Input/Gamepad_Input.htm
- Audio track position: https://manual.gamemaker.io/lts/en/GameMaker_Language/GML_Reference/Asset_Management/Audio/audio_sound_get_track_position.htm
- SHMUP Creator official website: https://www.shmupcreator.com/
- SHMUP Creator Steam page and feature description: https://store.steampowered.com/app/1289520/

**Version note:** GameMaker's official release information lists LTS 2026.0 as the recommended stable line, while 2026.1/2026.100 is the newer beta line in the November 2026 timeframe. Use LTS 2026.0 for this baseline unless the actual installed stable release and project compatibility checks establish a newer LTS. Do not adopt GMRT beta merely to claim the newest runtime; verify every required feature and target first.

## Shmup design and player experience

- Boghog's Bullet Hell Shmup 101 — movement, focus, hitboxes, bullet visibility, enemy roles, level pacing and scoring: https://shmups.wiki/library/Boghog%27s_bullet_hell_shmup_101
- r/shmups — beginner recommendations, modes and continuing: https://www.reddit.com/r/shmups/comments/1lctvpg/games_for_new_player/
- r/shmups — beginner entry points, genre literacy and accessible presentation: https://www.reddit.com/r/shmups/comments/1ugt8gu/entry_point_babys_first_shmup/
- r/shmups — why some games work better for beginners and how continues/rank affect frustration: https://www.reddit.com/r/shmups/comments/l5lh0f/shmups_thats_good_for_beginners/
- Dynamic Difficulty Adjustment review (2018): https://onlinelibrary.wiley.com/doi/10.1155/2018/5681652
- Systematic review of dynamic difficulty adjustment (2025): https://www.sciencedirect.com/science/article/pii/S1875952125001211

## Contemporary examples to study, not copy

- Raiden series: learn its clear arcade flow, weapon feedback and formation pacing.
- Devil Blade Reboot: accessible entry, difficulty options and replay value.
- Blue Revolver: modern arcade presentation and score-focused depth.
- Angel at Dusk: tutorial that teaches general shmup literacy, not only button controls.
- Graze Counter GM: approachable feel and generous progression/extends.
- Shinorubi: modern bullet-hell structure and difficulty/mode breadth.
- Zeroranger: progression, encounter identity and strong presentation.
- Bullet Banshee (October 2026 coverage): example of a vertical shmup experimenting with XP and stacked upgrade choices; study the progression readability, but do not turn OUTGUN into a randomised survivor-like unless that becomes a deliberate design choice.

## Key research conclusions

1. Control consistency and precise movement are more important than physically realistic inertia.
2. Bullet visibility depends on contrast, shape, pattern grouping, depth sorting and predictable trajectories—not merely bright colours.
3. Enemy roles should shape player positioning; weak enemies should not be needlessly evasive or bulky.
4. Death recovery, a small bomb grace window and post-hit invulnerability prevent a single mistake becoming an unavoidable chain of deaths.
5. Boss and level intensity should vary. Breathing space makes set pieces and difficult patterns more memorable.
6. Scoring should make the player consider meaningful short- and long-term choices, not merely add a large number to the HUD.
7. Modern accessibility does not require removing mastery. It requires training, practice access, honest difficulty choices and reliable feedback.
8. OUTGUN's unique identity should come from Diamond Ace's legacy, the hangar campaign loop, articulated bosses, music-aware presentation and radio personality—not from adding every genre feature indiscriminately.

---

# 24. START NOW

Begin with **M0** in the specified workspace. Inventory and preserve the current project, verify GameMaker version and baseline build, then continue into M1 and M2. Create `docs/EXECUTION-STATE.md` immediately and keep it truthful.

The immediate quality target is a complete, enjoyable Oasis vertical slice with the real player art and soundtrack preserved, an expressive parallax environment, readable enemy patterns, a functioning turret, responsive controls, a multi-phase first boss, training, a hangar return and persistent progression. After that slice is stable, extend the same architecture across all six campaign missions and six bosses.

**The final framework must be something the brother can open in GameMaker, understand, play, tune and finish by replacing clearly labelled art/audio assets and editing documented content data—not a complicated black box that only its original programmer can maintain.**


# OUTGUN — Master Directive v2.0 Creative and Gameplay Addendum

**Document:** `OUTGUN-OpenCode-Master-Directive-v2-Addendum.md`  
**Applies to:** `OUTGUN-OpenCode-Master-Directive.md`  
**Project:** OUTGUN  
**Project root:** `C:\Users\Administrator\GameMakerProjects\Outgun_Framework`  
**Asset source:** `C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Assets for Use`  
**Original soundtrack:** `C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_ost`  
**Prepared:** 6 November 2026  
**Revision:** 2.0 — additive clarification; original directive is retained.

---

# IMPORTANT — HOW THIS ADDENDUM APPLIES

This is an **additive amendment** to the existing `OUTGUN-OpenCode-Master-Directive.md`. Insert this entire addendum immediately after the original document's title/metadata and before `# 0. EXECUTION ORDER`, or provide it to OpenCode alongside the original master directive and explicitly instruct OpenCode to read this addendum first.

**Do not delete, shorten, silently omit or replace any non-conflicting requirement from the original master directive.** The original sections, technical requirements, quality rules, code standards, documentation obligations, six-boss ambition, art requirements, audio requirements, save requirements, co-op requirements and implementation milestones remain in force.

Where the original master directive conflicts with the new, more specific creative decisions below, this v2.0 addendum is authoritative. OpenCode must update the affected original sections in place, preserve all unrelated content, and record the reconciliation in `docs/DECISIONS.md`. Do not leave contradictory requirements active in the final documentation or code.

The key authoritative changes are:

1. The opening playable sequence is a **one-time, non-replayable Prologue starring the real Diamond Ace**, ending in a scripted, unwinnable encounter with OMEN. It is not a tutorial or training stage.
2. There are **six Story missions and ten Remix/Arcade missions**. The Prologue is separate and is not counted as one of the six Story missions.
3. Story and Remix encounters use **authored, repeatable, memorisable spawn and attack choreography**. Adaptive/dynamic encounter generation is isolated into a separately selected mode and never changes the normal Story or Remix patterns.
4. Spawners and altitude changes are driven by **authored vertical world progression and level markers**, not merely elapsed time.
5. Campaign backgrounds are **long, authored, finite, non-looping level paths**. Parallax layers move at different rates but must not restart the entire background when they reach an edge.
6. The altitude display has three clear states: **LOW, MEDIUM and HIGH**, with explicit gameplay and spawn eligibility rules.
7. There is **exactly one named/visible Pitstop per Story mission** and no Pitstops in Remix. Additional automatic safe-save anchors are not Pitstops.
8. Story power-ups are authored and predictable; Remix may use seeded random temporary power-ups. Permanent aircraft upgrades remain a separate, persistent Hangar system.
9. The first Story mission follows the specific beach-to-cloud-to-missile-defusal sequence described below. The existing Oasis soundtrack track remains the first soundtrack track as requested, even though the newly specified opening visuals are coastal.
10. Import and correctly assign the existing placeholder/source assets from `Assets for Use`, and preserve and correctly import the original soundtrack from `Outgun_ost`.

---

# A. PROJECT PATHS AND SOURCE-ASSET RULES — AUTHORITATIVE

Use these exact paths unless inspection proves the workspace has been moved. Do not substitute an older `C:\Users\shambrain\...` path or another user's profile path.

```text
C:\Users\Administrator\GameMakerProjects\Outgun_Framework\
    Assets for Use\       # Existing art, vectors, text material, sprites and backgrounds
    Outgun_ost\           # Original OUTGUN soundtrack; preserve source files
```

## A.1 Inspect and correctly assign all supplied assets

The user states that placeholder/source materials have already been added. Do not generate replacements before inspecting these folders.

Inventory every file under `Assets for Use` recursively. Identify, where possible:

- Player/Diamond Ace aircraft artwork.
- Enemy aircraft and ground-unit artwork.
- Boss concepts and boss-part artwork.
- Backgrounds and long-form level artwork.
- Tiles, tile sheets and environment components.
- Vector artwork and icon material.
- Text files, design notes, labels, reference sheets and animation notes.
- Sprite sheets, animation frames and cutscene material.
- HUD, menu, pickup, projectile and effect artwork.

For each asset, record the relative path, file type, dimensions, transparency, animation/frame information where detectable, intended role, source classification and GameMaker resource to which it is assigned. Create or update `docs/ASSET-ASSIGNMENT-MAP.md` and `assets/manifests/ASSET-MANIFEST.csv` (or an equivalent machine-readable manifest).

**Correct assignment and scaling are mandatory.** Do not simply import every image at arbitrary dimensions or assign every image to a generic sprite. Inspect image aspect ratios and transparency; set origins deliberately; preserve intended pixel density; avoid stretching; set appropriate filtering/compression; and distinguish backgrounds, sprites, UI, tiles, vector exports and sequence frames. Where a supplied vector format is not supported directly by the chosen runtime, create a documented raster export at appropriate resolutions while preserving the original vector source unchanged.

For every imported sprite, deliberately configure:

- Resource name and stable ID.
- Frame order and playback speed where animated.
- Origin/anchor appropriate to the object's behaviour.
- Bounding box and collision mask independent from the visual artwork where necessary.
- Texture page/atlas grouping where practical.
- Filtering and interpolation appropriate to pixel art or smooth illustration.
- Scale in world units based on source dimensions and intended on-screen size.
- Animation events through semantic events, not hardcoded frame-number gameplay logic.

For backgrounds, record their intended world width/height, visible playfield fit, scroll direction, art scale, parallax depth and whether each piece is finite or deliberately repeatable. **Do not loop a long unique stage background.**

Create contact sheets for supplied assets and a visual audit showing the imported resource beside its source. Report any unidentified, duplicated, corrupted or unsupported assets. Never silently discard an asset because its purpose is unclear.

## A.2 Original soundtrack — preserve and map it

Recursively inspect `Outgun_ost`. Preserve all original audio files byte-for-byte. Do not overwrite, destructively normalise, re-encode over, rename or move the source files. Import/copy into GameMaker's managed resource structure only as needed, keeping the source folder intact.

The **Oasis track is the first soundtrack track**. Identify the actual file and map it to the first intended soundtrack slot in `data/audio/music_manifest` (or the project's equivalent). Do not guess a filename. Do not rename the source file to make the mapping convenient. Record the GameMaker sound resource name, source path, format, duration, loop metadata and any verified BPM/cue data.

The first Story mission's newly specified opening environment is a beach/coastal sequence. This is a creative distinction from the Oasis track name. Keep the Oasis track first as requested; do not force an Oasis visual biome into the beach sequence simply because of the music filename. Preserve all Oasis artwork for later authored use, Remix missions, or a deliberately designed transition where it fits.

Create `docs/AUDIO-ASSET-MAP.md` linking every original track to its GameMaker sound resource, mission/mode usage, loop/cue markers and import settings. BPM and beat offsets must be measured from the actual files or left unknown; never invent them.

---

# B. CANONICAL GAME MODES AND CONTENT COUNTS

OUTGUN has three distinct gameplay categories. Keep their rules, records, progression and encounter systems clearly separated.

## B.1 One-time Story Prologue — Diamond Ace

The Prologue is the opening story sequence, not a training/tutorial level.

- The player initially controls the **real Diamond Ace**, the original legendary pilot.
- The player gets to experience the aircraft and basic shooting naturally through a short playable combat sequence. Do not stop the action for a tutorial curriculum, instructional pop-ups or training-room objectives.
- OMEN appears as the overwhelming threat.
- The player fights OMEN in a deliberately **unwinnable, canonically scripted encounter**. The player may shoot, dodge and demonstrate skill, but the encounter inevitably reaches its authored defeat/cutscene outcome.
- This outcome must not be presented as a normal skill failure. Do not make the player repeat an impossible boss fight, lose campaign progress or believe they failed to execute a hidden mechanic. The game must communicate through presentation that this is a story event.
- The sequence transitions into the established premise: the real Diamond Ace is dead; Rookie continues the mission in the same aircraft, and enemies subsequently mistake Rookie for Diamond Ace.
- The Prologue plays once on a fresh campaign/save. It is not listed as a mission, is not available through mission select, has no boss-practice entry, and cannot be replayed from the menu or archive. A deliberate full campaign reset may create a new campaign and replay its opening once; do not provide a separate Prologue replay button.
- Save a durable `prologue_completed`/`prologue_seen` flag so returning to the title or loading a save does not replay it unexpectedly.
- Use the existing OMEN boss architecture and art placeholders. The Prologue version may use a limited early form/moveset, while the final Story encounter uses the complete final boss. Both must be recognisably the same entity.

There is **no separate Training mode or Training room**. Remove/reconcile references to a replayable tutorial sortie, advanced training curriculum, tutorial selection or training completion as a campaign requirement. This does not prohibit a normal controls/settings screen or concise in-context HUD labels when they are genuinely needed.

The original directive's replay and learning philosophy remains: players improve by replaying authored missions, recognising patterns, refining routes and attempting cleaner runs. Do not replace that loop with a mandatory tutorial.

## B.2 Six Story missions

The Prologue is separate. The main narrative consists of exactly **six Story missions**, unlocked through the Hangar/mission board. Story missions carry the plot, briefings, radio conversations, mission objectives, persistent aircraft progression and one Pitstop each.

The canonical order is:

| Story mission | Primary environment/identity | Core encounter |
|---|---|---|
| 01 | Shallow Beach → Beach → Cloudline pursuit | Dune Regent launch-platform appearance, missile chase and missile-defusal climax |
| 02 | Snow | WHITEOUT |
| 03 | Hills | IRON HART |
| 04 | Ground Army | WAR MAUL |
| 05 | Sea | ABYSSAL CROWN / LEVIATHAN with animated tentacles |
| 06 | Enemy fortress / black-site / final approach | OMEN, full final boss encounter |

These are six narrative missions, not six training stages plus six missions. Mission IDs should be stable and data-driven (`story_01` through `story_06`) regardless of display-name changes.

Preserve the previous DUNE REGENT concept and art direction. In Story Mission 01, Dune Regent is the enormous enemy platform/commander encountered over the beach. It is **not fought in that opening encounter**; it launches the missile and withdraws/escapes according to the authored sequence. Preserve Dune Regent as an enemy/boss asset and behaviour concept so it can return in a later Remix mission or additional encounter if the content design calls for it. Do not delete its resource or concept merely because it is not the first conventional boss battle.

For the six principal Story climaxes, treat the missile-defusal encounter as the first boss-equivalent finale/set piece, followed by WHITEOUT, IRON HART, WAR MAUL, LEVIATHAN and OMEN as the five conventional major boss battles. Do not count the Prologue's scripted OMEN encounter as an additional campaign mission or as a seventh final boss.

## B.3 Ten Remix / Arcade missions

Create **ten separate Remix missions** (`remix_01` through `remix_10`). These are side missions, not story chapters.

- No narrative campaign dependency, no new mandatory story scenes and no story dialogue required. Short non-story callouts are allowed only when they serve gameplay.
- Reuse and recombine the six Story missions' environments, enemy art, bosses/components, tilesets, effects and parallax assets. Reuse should be intentional: new route composition, enemy formations, altitude sequences, projectile choreography, boss combinations and visual arrangement—not merely the same level copied ten times.
- Use alternate/remixed soundtrack arrangements or suitable alternate tracks where available. Preserve the original soundtrack files; do not invent or overwrite music. If alternate OST material is not present yet, provide clearly named configurable music slots and mark them as waiting for the creator's audio.
- Target roughly **3–5 minutes per Remix mission**, with **five minutes as the hard authored target ceiling** before results. Do not pad missions with empty scrolling. Shorter, purposeful stages are preferable.
- Remix is the pure arcade challenge: demanding, tightly authored, pattern-focused and replayable. Every mission ends with a meaningful boss or boss-like climax. A Remix boss may be a returning Story boss, Dune Regent, a component variant or a newly configured composite assembled from existing assets.
- **No Pitstops and no Story checkpoint/continue anchors** in Remix missions. A failed run restarts the selected Remix mission according to its arcade rules. Preserve the score/record history, not an in-progress checkpoint.
- Remix enemy waves and attack choreography remain fixed and learnable. Random temporary power-ups may be used as described in Section F; do not randomise enemy encounters.
- Maintain separate score records, medals and leaderboard rules from Story mode.

## B.4 Separate Dynamic Operations mode

Dynamic/adaptive encounter generation is not part of the normal OUTGUN shmup campaign or standard arcade Remix mode.

If dynamic encounter logic is implemented, it must be a **separate, explicitly selected mode** (working label: `Dynamic Operations`). It must have its own menu entry, ruleset ID, records and documentation. Its adaptive spawners may vary encounter composition within authored safety limits, but they must never alter Story or Remix wave order, spawn locations, pattern choreography, altitude schedule, boss phase timing or fixed level triggers.

Do not put adaptive difficulty, reactive wave replacement, performance-based enemy spawning or hidden encounter changes behind the normal Story/Remix director. The player should be able to memorise the standard mode and pursue a repeatable perfect run.

If resource/time limits prevent a full Dynamic Operations mode, implement only a clearly isolated future extension point and mark the mode as not implemented. Do not activate a fake dynamic system in Story or Remix to claim completion.

---

# C. FIRST STORY MISSION — EXACT OPENING SEQUENCE

Story Mission 01 must implement this sequence as an authored, vertical-progress-driven set piece. This is a core OUTGUN identity feature, not a generic placeholder mission.

## C.1 Shoreline opening

1. Start low over a **shallow beach**. The player sees the sea/shoreline, shallows, sand and coastal features moving at controlled parallax rates.
2. The level progresses into the main **beach** environment. Use the supplied backgrounds and art where suitable; do not loop a short image to fake a long level.
3. The player encounters the enormous Dune Regent/launch platform. The platform is revealed with a memorable entrance, radio reaction and environmental scale.
4. Do not turn this into a conventional boss fight. Dune Regent launches a missile aimed at specified city coordinates, then escapes or becomes unavailable to attack according to the authored story beat.
5. The missile launch, city target and urgency must be visually and narratively clear. Show the target/destination through an appropriate HUD/radar cue and Operator radio dialogue.

## C.2 Cloudline pursuit

6. Rookie climbs above the clouds to pursue/intercept the missile. This is an authored altitude transition from LOW/MEDIUM to HIGH.
7. The background changes from beach/low-altitude scenery to clouds and high-altitude sky layers. Preserve the finite stage path and smooth parallax continuity; do not reset or loop the beach background.
8. Spawn enemy aircraft in fixed formations along the vertical course while Rookie catches up to the missile. These waves are triggered by stage progress markers, not by a free-running timer.
9. The missile remains ahead of the player until the authored interception point. Use a distance/progress indicator if it improves readability. The player fights through the waves but cannot accidentally bypass the mission event by scrolling past its trigger.

## C.3 Missile defusal — first boss-equivalent climax

10. Once Rookie catches up, enter the first mission's boss-equivalent encounter: the missile is the central mission-critical target. This is a focused objective encounter, not an ordinary enemy that can be destroyed by shooting.
11. Operator explains over the radio that she can hack/defuse the missile, but Rookie must remain close enough to establish the link.
12. Display a **medium-sized circular defusal radius** around the missile. The circle must be visually readable and scaled in world-space so its interior is large enough for precise dodging while still requiring the player to stay close.
13. The player must remain inside the circle for the defusal bar to fill. While inside, the bar advances at a clearly communicated rate. When the player leaves the radius, progress pauses (do not silently reset the whole bar). Re-entering continues the existing progress.
14. Taking a hit must not erase all defusal progress. Apply the normal player hit/invulnerability rules; the defusal action may pause briefly during a hit/respawn state only if the reason is clearly communicated.
15. Enemy aircraft attack in an authored **circular/orbital formation around the missile**. Use the emitter/pattern system to produce rotating formations, aimed attacks and controlled gaps. Their orbit and firing sequence must be fixed and repeatable so the player can learn it. Never rotate/spawn enemies randomly into the player's position.
16. Provide clear HUD feedback for the defusal bar, link status, distance/radius and any missile impact deadline. The Operator's dialogue must explain the objective once, then remain concise during combat.
17. Completion of the defusal bar triggers a validated, one-shot `MISSILE_DEFUSED` event, stops/neutralises the missile, resolves remaining hostile attacks safely and plays a short cinematic/radio payoff. Do not award the mission reward twice if the event is replayed or a save is loaded.
18. If the missile deadline is used, make the time pressure understandable and fair. Define the failure/retry path. A failed attempt must not create a softlock or erase unrelated permanent progression. Prefer a clear authored retry from the nearest valid Story recovery point.

The missile encounter must be implemented as a reusable objective/boss-event component so other missions can reuse hold-radius, escort, disable, intercept or multi-terminal mechanics without copying the entire mission script.

---

# D. AUTHORED VERTICAL PROGRESSION, FINITE LEVELS AND MANUAL TRIGGERS

## D.1 The level is a finite authored path

The main Story and Remix backgrounds are long, unique, vertically progressing level environments. They are **not automatically looping backgrounds**.

- Define a world-space level length and a monotonically increasing stage-progress value.
- The camera/scroll controller advances through the authored level path.
- Each parallax layer has its own offset and speed multiplier but is bounded by its authored content length or explicit end behaviour.
- At the final world coordinate, execute the mission's exit/boss/result event; never wrap the stage to its beginning.
- Do not repeat a finite background texture merely to hide the end of the artwork.
- Individual effects, small water textures, clouds or decorative strips may repeat only when the asset configuration explicitly marks them as repeatable. This must not make the overall stage path loop.
- Preserve tilemaps for authored terrain and ground surfaces where they are useful. Tiling does not mean the mission itself loops.

## D.2 Vertical progress is the primary encounter clock

The primary mission director must use **vertical level progression/distance** as its trigger domain. Time may still be used inside a projectile pattern, animation, cooldown, laser charge or dialogue delay, but it must not be the sole clock for deciding when a wave appears along a finite level.

Support both editor-placed trigger instances/markers and validated data-defined trigger records. The creator should be able to visually place a trigger at a world-space position in the Room Editor or change its progress coordinate in a documented mission data file.

Each marker must have:

- Unique stable trigger ID.
- Mission ID and world progress coordinate.
- Trigger type: spawn formation, altitude change, dialogue, music cue, camera event, background/layer change, objective, miniboss, boss entrance, Pitstop, autosave checkpoint or other allowlisted event.
- Spawn/formation/pattern ID where applicable.
- Altitude requirement/filter.
- Optional prerequisites and one-shot/repeat policy (default one-shot per attempt).
- Debug label and editor-visible marker/category.

When the camera crosses a trigger coordinate, process every crossed trigger in deterministic coordinate/priority/ID order. Large frame-time spikes must not skip a marker. Restarting or resuming must not fire an already completed one-shot trigger twice. Save the trigger ledger/progress so the mission resumes correctly.

Do not use a dynamic encounter director to move triggers or replace the authored wave schedule in Story or Remix. The same inputs and mode configuration should lead to the same encounter order.

## D.3 Spawner and formation library

Build a reusable authorable spawner system. It must support formation templates including at least:

- Single entry.
- Straight horizontal line.
- Straight vertical column.
- Diagonal line (left-to-right and right-to-left).
- V formation and inverted V.
- Wedge/arrow formation.
- Staggered/offset rows.
- Double-column formation.
- Diamond/box formation.
- Arc/curved formation.
- Circular/orbital formation.
- Split formation that opens into two lanes.
- Custom waypoint formation.

Each formation is configurable for enemy type, count, spacing, spawn edge/anchor, direction, movement profile, entry speed, delay between members, attack delay, altitude filter and optional emitter pattern. Store reusable presets in clearly named folders and provide a `docs/SPAWNER-101-GUIDE.md` with visual diagrams/contact sheets showing what each pattern looks like and which parameters the brother can change.

Provide an in-game/debug preview or test room that lets the designer select a formation, enemy type, altitude state and movement profile and see the result without editing shared engine code.

## D.4 Emitter configuration belongs to every ship

The **player ship's guns must use configurable emitters**, and enemies/bosses must use the same reusable emitter architecture where applicable.

An emitter configuration should define muzzle/anchor offset, projectile type, cadence, burst size, spread, angle, speed, acceleration, damage, lifetime, visual/trail/effect, sound, cooldown, simultaneous emitter group and weapon-level modifications. Support multiple emitters per ship (for example left/right guns, central gun, wing pods, missile launcher or boss weapon ports).

The player weapon resource must not hardcode a single projectile pattern into the player Step event. The brother must be able to modify the ship's emitter list and each enemy's emitter list through data/configuration. Keep player/enemy ownership, collision masks and damage rules explicit.

---

# E. ALTITUDE SYSTEM — LOW / MEDIUM / HIGH

Altitude is a readable gameplay/world-state system. The player does not need to freely fly into a different depth plane using a 3D vertical axis. The authored mission path defines the aircraft's altitude state through manual vertical-progress trigger markers.

The HUD meter must display exactly the understandable bands **LOW**, **MEDIUM** and **HIGH** (visual styling may include a simple icon/bar, but do not require a numeric height readout).

## E.1 Altitude rules

- **LOW:** the aircraft is close to the ground. Ground forces are eligible and expected: turrets, tanks, APCs, SAM sites, artillery, convoys and ground-boss components. Aerial enemies may also appear.
- **MEDIUM:** transitional/mixed altitude. Use a designed mixture of ground and flight encounters according to the mission's authored trigger schedule. Ground units remain eligible where the art/terrain supports them.
- **HIGH:** the aircraft is well above the ground/cloud layer. Ground combat is locked out: do not spawn ground-unit encounters in HIGH zones. Use flight-only encounters. High-altitude aircraft should be more challenging, with more unique, faster, stronger or more advanced ship archetypes and attack patterns.

High altitude must not simply mean all existing enemy bullets become unfairly fast. Introduce advanced archetypes and more complex but readable combinations. The difficulty increase is fixed and authored, not adaptive to player performance.

## E.2 Manual altitude triggers

Create editor-placeable `ALTITUDE_ZONE` markers at vertical world-progress coordinates. A marker changes the altitude state and may reference a short transition sequence, parallax profile, cloud/fog effect, music cue or allowed enemy roster.

The altitude transition must be visible and smooth: change background depth, ground visibility, cloud layers and HUD state with a short controlled transition. The gameplay state changes at a deterministic trigger point. Do not spawn ground units in advance of a HIGH state and then delete them abruptly; filter the spawn catalogue before spawning.

Every mission should have a deliberate altitude schedule. Story Mission 01 must specifically transition from beach/LOW through the ascent to cloudline/HIGH. Other missions may move between LOW/MEDIUM/HIGH as appropriate to their environment. Remix schedules may reuse and rearrange these zones, but remain authored and fixed.

Create `docs/ALTITUDE-SYSTEM-101.md` explaining the three bands, how the author places a trigger, which enemy categories are permitted, and how to preview the transition.

---

# F. PERFECT-RUN DESIGN, FIXED ENCOUNTERS AND POWER-UP ECONOMY

## F.1 Story and Remix must be repeatable

A core shmup pleasure is learning an encounter, remembering formations and bullet patterns, then improving the route toward a cleaner or perfect run. Respect that genre contract.

In **Story and Remix**:

- Enemy waves, spawn locations, formation types, altitude changes, boss phases and attack schedules are authored and repeatable.
- Do not change the next wave because the player scored highly, took damage, killed an enemy quickly or lingered in a lane.
- Do not silently tune bullet speed, number, angle, enemy HP or spawn position during a run.
- Do not use hidden dynamic difficulty/adaptive encounter replacement.
- Any aimed shot may calculate its direction from the player's position at the exact firing/commit moment, as a normal authored shmup attack, but it must then travel according to its defined trajectory. Do not continuously home an ordinary bullet at the player unless it is a clearly signposted homing projectile with bounded turn rate.
- Seeded variation is allowed only where explicitly specified, recorded and reproducible. Do not randomise the core pattern choreography.

The goal is not that every run looks identical in every incidental detail; the goal is that the player can learn the level's structure, timings, threat roles and safe routes through repeated play. Maintain deterministic trigger ordering and stable attack patterns.

## F.2 Separate Story power-ups from permanent upgrades

Use three distinct concepts:

1. **Permanent Hangar upgrades:** persist across Story sorties and are purchased/unlocked through the Calibration Bay. They modify the aircraft within balanced caps and are documented in the save schema.
2. **Story mission power-ups:** placed at authored vertical-progress positions and/or delivered by specifically authored enemy drops. These are predictable, curated and repeatable. Do not use random spawn locations or hidden probability rolls in Story mode.
3. **Remix temporary power-ups:** randomised/weighted arcade-style temporary pickups may be used in Remix. Their spawn opportunities must be authored (the pickup can vary, but an unannounced object must not appear in an unsafe location). Use a controlled seed and record it with the run so the exact pickup sequence can be reproduced for testing/replay. Temporary Remix power-ups reset according to the Remix run rules and do not silently become permanent aircraft upgrades.

## F.3 Permanent upgrade balance and arcade fairness

Permanent upgrades should provide meaningful but bounded choices. Do not create huge multiplicative damage/health bonuses that make the Story campaign or leaderboard difficulty collapse.

For **ranked/standard Remix**, use a standardised aircraft baseline so permanent Hangar upgrades cannot create an unfair score advantage. The selected aircraft appearance may carry over, but ranked gameplay statistics must be normalised. If a future unranked Remix variant allows a fully upgraded Hangar loadout, label it clearly and keep its records separate.

Story missions use the player's permanent aircraft configuration plus their mission's authored temporary power-up route. The two systems must be visible in the UI and saved separately.

## F.4 Perfect-run scoring and records

Track clean clears, continues, deaths, bombs used, damage received, score, combo, enemy route efficiency, boss part destruction and mission time. Define a clear perfect/no-continue/low-damage medal policy. Never call a run perfect if assists/continues or a different ruleset invalidate that award. Preserve the encounter and score ruleset version with the record so later balance changes do not misrepresent old results.

---

# G. PITSTOPS AND STORY RECOVERY CHECKPOINTS

The terms **Pitstop** and **automatic save checkpoint** must be distinct in code, UI and documentation.

## G.1 Exactly one Pitstop per Story mission

There is **one and only one named, visible Pitstop per Story mission**. It is a deliberately authored safe intermission/maintenance point where the player can take a break and the current sortie is safely persisted.

- Exactly one `PITSTOP` event/marker per Story mission.
- Do not generate a second Pitstop because a mission has a miniboss, extra event or long section.
- A Pitstop can include a visual service bay, support carrier, safe radio moment or pause-to-break screen; match the art and story context.
- The Pitstop is not the Hangar and does not replace the permanent upgrade system.
- It must preserve the exact current run state needed to continue: mission ID, world progress, altitude, player lives/health/status, weapon levels, secondary stock, bombs, temporary power-ups, permanent upgrade snapshot/version, XP/rewards, score, combo state, active mission objectives, cleared component flags, current event/phase, trigger ledger, deterministic random seed/state, pending encounter schedule and relevant story/dialogue flags.
- Prefer placing the Pitstop at an authored safe boundary with no hostile bullets actively crossing the player. If an encounter must be suspended, implement exact restoration of its active state; do not silently restart the encounter while claiming an exact resume.
- Saving must be atomic and validated. Keep recovery/backup behaviour from the original save-system requirements.
- When returning to the game, offer a clear resume option. Do not unexpectedly replay the Prologue or restart the whole Story mission.

## G.2 Story autosave/recovery anchors are not extra Pitstops

In addition to the one visible Pitstop, Story mission data must support automatic safe-save checkpoint anchors at these authored boundaries when they exist:

1. Mission start.
2. Midpoint (if the mission has a meaningful midpoint).
3. Immediately after a miniboss (if present and once the encounter has resolved).
4. Before the final boss or major final event.
5. At the start of any additional major event that would be frustrating to repeat from far earlier in the level.

These are **silent/low-friction recovery snapshots**, not visible repair stations, not additional Pitstops and not extra Hangar visits. Each mission may omit anchors that do not apply; never invent a miniboss solely to justify a checkpoint.

Checkpoint snapshots must be taken at a deterministic safe boundary. Record the trigger ID and mission progress. If an automatic snapshot would otherwise occur during an active projectile pattern, wait for the authored safe transition rather than serialising a half-resolved combat frame without the required state.

## G.3 Death, continues and resume rules

Separate ordinary arcade life loss from suspend/resume:

- Losing a life during a sortie follows the mission's documented shmup rules (hit feedback, invulnerability, respawn and any weapon degradation). It does not automatically teleport the player to the Pitstop.
- A player who closes the game or chooses to take a break resumes from the most recent valid Story snapshot/Pitstop according to the save policy.
- A game-over/continue flow may offer the most recent recovery anchor, with continue use clearly recorded for score/medal eligibility. It must not create an infinite free perfect-run loop.
- Replaying from a checkpoint must reset transient enemy/projectile state to the exact authored checkpoint definition and must not double-award previous XP, drops, score or story unlocks.
- Preserve a true fresh-mission restart option for players who want to practise the full perfect run.

## G.4 Remix has no Pitstop or checkpoint system

Remix missions have **zero Pitstops and zero in-level recovery checkpoints**. No visible Pitstop, no safe break station and no continue-from-mid-stage snapshot. The player can pause the game normally, but after a failed/abandoned Remix run, restarting begins that selected Remix mission according to its arcade rules. Preserve completed scores and records independently.

Create `docs/PITSTOP-AND-CHECKPOINTS-101.md` explaining the one-Pitstop rule, the separate automatic save anchors, the exact resume payload and the different Story/Remix failure rules.

---

# H. AUDIO, DIALOGUE AND MISSION TIMING ADDITIONS

Keep all original audio architecture requirements. Add these concrete mappings:

- The first soundtrack slot is the existing **Oasis** track found in `Outgun_ost`.
- The first Story mission's initial visual route is shallow beach → beach → above-cloud pursuit. Do not reorder the actual soundtrack files merely because the first environment is coastal.
- The first mission's missile launch receives a clear music/impact cue; cloud pursuit can use a controlled transition/crossfade; missile defusal receives a dedicated tension/defusal cue where the existing music supports it.
- Remix missions should have alternate/remixed OST slots. Use source audio only when present and verified. Missing alternate compositions are authoring slots, not permission to generate or overwrite the original OST.
- Radio dialogue for the first mission must include the initial enemy-platform sighting, missile launch/city warning, ascent/pursuit, Operator's explanation of the close-range hack requirement, defusal progress feedback and successful defusal/failure result.
- Do not allow repetitive Operator lines to fire on every frame while the player enters/leaves the defusal radius. Use one-shot trigger IDs and sensible thresholds/cooldowns.

Music remains presentation support, not a rhythm dependency. Enemy pattern timing and vertical world triggers must be stable even if audio is muted, paused or unavailable.

---

# I. REQUIRED FOLDER/EDITOR GUIDES — BEGINNER FRIENDLY

Preserve the original folder-organisation goal and make the project approachable to a beginner. Add or update these guides, using actual implemented resource paths and screenshots/diagrams where useful:

- `docs/START-HERE-101.md` — how to open the project, run it, understand the GameMaker asset browser, find the player, enemy, boss, room and script resources, and make a safe change.
- `docs/PROJECT-MAP-101.md` — annotated folder/resource map; what belongs where; what not to edit casually.
- `docs/ASSET-ASSIGNMENT-MAP.md` — source asset → imported resource → in-game use → replacement instructions.
- `docs/LEVEL-AUTHORING-101.md` — finite stage construction, room dimensions, world progress, camera scroll, parallax layers and level end.
- `docs/SPAWNER-101-GUIDE.md` — place a vertical progress trigger and choose straight, diagonal, V, wedge, line, column, staggered, arc or orbital formations.
- `docs/ALTITUDE-SYSTEM-101.md` — LOW/MEDIUM/HIGH markers, allowed enemy types and visual transitions.
- `docs/EMITTERS-AND-WEAPONS-101.md` — configure the player gun and enemy/boss weapon emitters without editing core logic.
- `docs/PITSTOP-AND-CHECKPOINTS-101.md` — the one Pitstop and separate save/recovery anchors.
- `docs/STORY-AND-REMIX-101.md` — differences between Prologue, six Story missions, ten Remix missions and the optional Dynamic Operations mode.
- `docs/POWERUPS-AND-UPGRADES-101.md` — authored Story pickups, randomized Remix pickups and permanent Hangar upgrades.
- `docs/MISSION-AND-BOSS-AUTHORING-101.md` — add a mission event, boss phase, weakpoint and semantic animation trigger.

Guides must explain actual controls and files, not imagined features. Include concise diagrams/contact sheets where they materially help. The brother must be able to find an enemy, alter its speed/HP/formation/emitter, place its spawn trigger, preview it and run the mission without searching through unrelated GML code.

---

# J. UPDATED CONTENT AND ACCEPTANCE CHECKLIST

Update `docs/IMPLEMENTATION-TRACKER.md` (create it if absent) so every item below has a status of `NOT STARTED`, `IN PROGRESS`, `IMPLEMENTED`, `TESTED` or `BLOCKED`. `IMPLEMENTED` is not the same as `TESTED`.

## Prologue

- [ ] Real Diamond Ace playable with the original art where available.
- [ ] No tutorial/training curriculum or training menu.
- [ ] OMEN appears in a scripted unwinnable encounter.
- [ ] Canonical death/transition to Rookie is delivered without a fake skill-failure loop.
- [ ] Prologue is one-time per campaign and cannot be selected/replayed from menus.

## Story campaign

- [ ] Exactly six Story mission definitions (`story_01`–`story_06`).
- [ ] Mission 01: shallow beach → beach → Dune Regent launch encounter → missile launch to city coordinates → above-cloud chase → fixed enemy waves → close-radius missile defusal objective.
- [ ] Defusal circle, progress rules, Operator radio cues, circular enemy attack choreography and one-shot completion event tested.
- [ ] Mission 02: Snow/WHITEOUT.
- [ ] Mission 03: Hills/IRON HART.
- [ ] Mission 04: Ground Army/WAR MAUL.
- [ ] Mission 05: Sea/LEVIATHAN with independently animated tentacles.
- [ ] Mission 06: final fortress/OMEN.
- [ ] Existing Dune Regent design/art retained and its later reuse documented.

## Remix/Arcade campaign

- [ ] Exactly ten Remix mission definitions (`remix_01`–`remix_10`).
- [ ] No story campaign dependency or required story dialogue.
- [ ] Reuses and recombines existing six-mission art/environment/boss assets intentionally.
- [ ] Each mission has a fixed authored enemy/attack schedule and a boss/boss-like climax.
- [ ] Target 3–5 minutes; five-minute design ceiling.
- [ ] No Pitstops and no in-stage recovery checkpoints.
- [ ] Temporary random power-ups use a recorded/reproducible seed.
- [ ] Ranked Remix uses a standardised aircraft stat baseline; separate any unranked upgraded-loadout records.

## Dynamic Operations

- [ ] Separate selectable mode and separate ruleset/records if implemented.
- [ ] No adaptive wave changes leak into Story or Remix.
- [ ] If only an extension point exists, it is clearly marked as not implemented.

## Level progression and altitude

- [ ] Finite, unique, non-looping level path and end-of-level behaviour.
- [ ] Spawn triggers use vertical world progress/Room markers rather than only timers.
- [ ] Frame spikes cannot skip or reorder triggers.
- [ ] Straight line, diagonal (both directions), V, wedge, staggered, columns, diamond/box, arc and orbital formations available as presets.
- [ ] Player aircraft guns use editable emitter configurations.
- [ ] Enemy and boss weapons use the same reusable emitter architecture where appropriate.
- [ ] LOW/MEDIUM/HIGH altitude meter is visible and correct.
- [ ] LOW allows ground and aerial encounters; MEDIUM supports a designed mix; HIGH is flight-only and unlocks tougher/faster/more unique aerial enemies.
- [ ] Altitude changes are manually authored progress triggers and are previewable.

## Power-ups, permanent upgrades and perfect runs

- [ ] Story pickups and enemy drops are authored and repeatable.
- [ ] Permanent Hangar upgrades persist separately from run power-ups.
- [ ] Remix random temporary power-ups are seeded/reproducible.
- [ ] No hidden dynamic difficulty or adaptive enemy replacement in Story/Remix.
- [ ] Perfect/no-continue records use transparent eligibility rules.

## Pitstops and save/resume

- [ ] Exactly one named/visible Pitstop in each of the six Story missions.
- [ ] Multiple silent automatic safe-save anchors may exist only at the listed authored boundaries and are not called Pitstops.
- [ ] Pitstop resume restores the exact documented run state.
- [ ] Trigger ledger prevents duplicate waves, rewards, dialogue and events after resume.
- [ ] Remix has no Pitstops or checkpoint resume.

## Assets and music

- [ ] All files under `Assets for Use` inventoried, classified, assigned and scaled deliberately.
- [ ] Original source materials preserved unchanged.
- [ ] Asset contact sheets and replacement map generated.
- [ ] All audio files under `Outgun_ost` inventoried and original files preserved.
- [ ] Oasis is assigned as soundtrack track one.
- [ ] Music assets have verified import settings and documented cue/loop data.

---

# K. DECISIONS AND CONTRADICTIONS — RESOLVE EXPLICITLY

Record these decisions in `docs/DECISIONS.md`. Do not silently interpret the old and new requirements as if they were identical.

| Earlier directive / potential contradiction | Authoritative v2 decision |
|---|---|
| Replayable Training sortie, advanced training, training menu | Replaced by a one-time Prologue starring real Diamond Ace and a scripted unwinnable OMEN encounter. No separate Training mode. |
| Six missions plus a separate training mission | Exactly six Story missions; Prologue is outside the mission count; ten Remix missions are additional. |
| Dynamic encounters affecting ordinary enemy waves | Story and Remix use fixed authored schedules. Dynamic Operations is separate and cannot affect them. |
| First Story biome assumed to be Oasis | Mission 01's actual visual path is shallow beach → beach → cloudline. The existing Oasis soundtrack file remains track one. Preserve Oasis art and use it elsewhere where appropriate. |
| Dune Regent as the first conventional boss fight | Dune Regent is first encountered as a huge non-combat launch platform that fires the missile. The missile-defusal objective is the first boss-equivalent climax. Preserve Dune Regent for future/remix reuse. |
| Automatically looping scrolling backgrounds | Story and Remix are finite, authored level paths. Parallax scrolls; the overall stage does not loop. Individual decorative textures may repeat only when explicitly configured. |
| Spawners driven by elapsed time | Spawners are primarily triggered by vertical world-progress markers. Timers remain valid inside attack/cooldown/animation behaviour. |
| Altitude represented only as visual atmosphere | LOW/MEDIUM/HIGH is a visible, authored gameplay state that controls ground/flight enemy eligibility and the high-altitude enemy roster. |
| Random power-ups and persistent upgrades treated as one system | Story uses fixed curated drops; Remix may use seeded temporary random drops; permanent upgrades remain a separate Hangar progression system. |
| Multiple Pitstop-like checkpoints | Exactly one visible Pitstop per Story mission. Other safe autosave anchors are silent recovery snapshots, not Pitstops. Remix has none. |
| Boss practice/training as default genre onboarding | Normal learning is through repeatable authored level play and pattern memorisation. The Prologue cannot be replayed. Keep only already specified practice features that do not recreate a separate Training mode and do not undermine the perfect-run design. |

## K.1 One remaining design distinction to preserve, not “fix” by guessing

The first soundtrack track is named **Oasis**, but the new first-mission visuals are **shallow beach → beach → clouds**. Keep the Oasis track first exactly as requested and keep the beach visual path exactly as requested. Do not rename the track or force the level to become an Oasis stage. If the source assets later show that the coastal scene is intentionally part of a larger Oasis/coastal region, record that as a creative decision only after inspecting the actual art and notes.

## K.2 Pitstop versus checkpoint distinction is mandatory

“One Pitstop per Story mission” and “several Story checkpoint save boundaries” are compatible only when the terms are kept separate. A Pitstop is a single visible, named in-world rest/service event. Mission-start, midpoint, post-miniboss, pre-final-boss and additional-event save anchors are automatic safe snapshots. They do not create a second Pitstop. Never place multiple visible Pitstop stations in one Story mission.

---

# L. IMPLEMENTATION PRIORITY UPDATE

Keep the original M0–M6 milestones and every non-conflicting exit criterion. Reorder only the affected scope as follows:

1. **M0:** inspect the exact `Administrator` project path, `Assets for Use`, `Outgun_ost`, current `.yyp`, Git and installed GameMaker version. Preserve assets and establish baseline.
2. **M1:** core application shell, input, save system, fixed world-progress director, finite camera/scroll system, parallax profiles, altitude state, emitter architecture and Hangar foundation.
3. **M2:** implement the complete first playable Story slice from the one-time Prologue into Story Mission 01's shallow beach → beach → cloudline → missile-defusal finale, then debrief/Hangar. Do not build a training room. Prologue must be separate from the six Story missions.
4. **M3:** complete Story missions 02–06, six Story definitions, all boss frameworks, one Pitstop per Story mission, automatic safe-save anchors, XP and permanent upgrades.
5. **M4:** complete ten Remix missions, each with fixed patterns, short arcade duration, a boss climax, no Pitstop and seeded temporary random power-up rules. Preserve independent score records.
6. **M5:** implement the separate Dynamic Operations mode only after the fixed Story/Remix modes work; then complete local co-op, platform adapter, achievements and production polish.
7. **M6:** full campaign/Remix QA, save recovery tests, asset scaling audit, frame-rate tests, performance profiling, GitHub verification and release-readiness review.

Do not postpone the first mission's missile defusal mechanic until late polish. It is a core gameplay identity feature and must be playable in the first vertical slice.

---

# M. REQUIRED UPDATE TO THE FINAL HANDOFF

In addition to the original master directive's final handoff list, report:

1. Confirmation that the one-time Prologue uses real Diamond Ace and OMEN, and whether its scripted outcome is complete/tested.
2. Confirmation of exactly six Story mission definitions and ten Remix mission definitions.
3. A concise encounter schedule for all six Story missions and all ten Remix missions.
4. Story Mission 01's stage-progress trigger list and missile-defusal test evidence.
5. A diagram/list of LOW/MEDIUM/HIGH altitude segments for each mission.
6. A count of authored spawner presets and verified player/enemy emitter configurations.
7. Confirmation of one and only one Pitstop marker per Story mission, zero Pitstops in Remix, and the separate autosave anchor list.
8. Proof that Story/Remix wave schedules are deterministic and do not invoke adaptive encounter logic.
9. The random power-up seed/replay policy for Remix and the permanent-upgrade normalisation policy for ranked Remix.
10. The asset assignment/scaling report for `Assets for Use`.
11. The soundtrack mapping proving Oasis is track one and the source files in `Outgun_ost` were preserved.
12. Any creative distinction or unresolved art/music mismatch, particularly Oasis music versus coastal opening visuals.

**Start by preserving the existing project and applying this addendum to the original master directive. Then implement; do not stop at updating documentation.**

# OUTGUN — MASTER DIRECTIVE V3.0
## Reference Gameplay Reconstruction, Native GameMaker Authoring, Complete Project Organisation and Quality Expansion

**Document:** `OUTGUN-OpenCode-Master-Directive-v3-Reference-Reconstruction-and-Project-Usability.md`

**Project:** OUTGUN

**Authoritative project root:**
`C:\Users\Administrator\GameMakerProjects\Outgun_Framework`

**Authoritative GameMaker project:**
`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_Framework.yyp`

**Original playable reference game:**
`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_shmup creator build1\_stand-alone_executable\outgun.exe`

**Reference build directory:**
`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_shmup creator build1`

**Original game artwork:**
`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_real_game_sprites`

**Sprite source directory:**
`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_real_game_sprites\Sprites`

**Aseprite source files:** Located recursively under `Outgun_real_game_sprites`.

**Engine:** GameMaker LTS 2026.0 / GMS2 Runtime 2026.0.0.23, subject to installed-version and compatibility verification.

**Revision:** 3.0

**Purpose:** Add a complete reference-game reconstruction and professional project-authoring standard to the existing OUTGUN master directive.

---

# IMPORTANT — PRESERVATION AND ADDITIVE DEVELOPMENT

This directive supplements:

1. `OUTGUN-OpenCode-Master-Directive.md`
2. `OUTGUN-OpenCode-Master-Directive-v2-Addendum.md`
3. All existing project decisions, implementation records, completed systems, audits, tests, documentation, assets and source code.

**The existing work must be preserved.**

Do not remove, overwrite, discard, simplify, disable or replace an existing implemented feature merely because a different implementation seems easier.

Do not restart the project.

Do not create a separate prototype as a substitute for the authoritative GameMaker project.

Do not replace the existing project architecture without proving that the replacement preserves its functionality.

The existing source project, audit work, build process, mission architecture, assets, gameplay systems and documentation must remain intact.

The purpose of this amendment is to inspect, understand, reproduce, improve, complete and organise everything.

Where an improvement requires changing an existing implementation:

1. Inspect the existing implementation.
2. Record its current behaviour.
3. Identify the limitation or defect.
4. Design the improvement.
5. Preserve existing functionality.
6. Migrate the implementation safely.
7. Run regression tests.
8. Document the change and its dependencies.
9. Retain a recoverable history through Git.

Where a feature becomes obsolete, document and deprecate it. Do not silently delete its source or erase its historical purpose.

If requirements appear contradictory, record the issue in `docs/DECISIONS.md` and preserve both requirements until the correct reconciliation is established.

The existing six Story missions, ten Remix missions, one-time Prologue, optional Dynamic Operations, six major boss concepts, authored encounter systems, finite backgrounds, altitude system, emitters, missile defusal, Pitstops, checkpoint system and permanent upgrades remain authoritative.

---

# N. COMPLETE PROJECT ORGANISATION AND BEGINNER-FRIENDLY AUTHORING

## N.1 The project must explain itself

The OUTGUN project must be understandable to someone opening it for the first time.

The objective is not to make the project technically simple by removing sophisticated systems.

The objective is to make a sophisticated game framework easy to understand and operate.

A beginner must be able to locate:

- The player aircraft.
- Player movement and banking.
- Player animations and weapon emitters.
- Player health, lives and damage handling.
- Every enemy aircraft.
- Every ground enemy.
- Every boss and boss component.
- Every projectile.
- Every pickup and power-up.
- Every explosion and visual effect.
- Every background and parallax layer.
- Every mission and room.
- Every mission trigger.
- Every formation preset.
- Every altitude zone.
- Every boss phase.
- Every soundtrack and sound effect.
- Every HUD element.
- Every menu and settings screen.
- Every save and progression system.

The user must not need to search through unrelated GML files to find a basic gameplay setting.

## N.2 Native GameMaker Asset Browser organisation

Organise the actual authoritative `.yyp` using meaningful GameMaker Asset Browser groups.

Use a logical structure similar to the following, adapting it to the project's existing resources rather than blindly duplicating them:

```text
OUTGUN
│
├── 00_PROJECT_CORE
│   ├── Game_Configuration
│   ├── Global_Controllers
│   ├── Game_State
│   ├── Input
│   ├── Save_System
│   └── Platform_Services
│
├── 01_PLAYER
│   ├── Diamond_Ace
│   ├── Rookie
│   ├── Aircraft_Animations
│   ├── Banking_Frames
│   ├── Movement
│   ├── Weapons
│   ├── Emitters
│   ├── Damage
│   └── Player_Effects
│
├── 02_ENEMIES
│   ├── Aircraft
│   ├── Ground_Units
│   ├── Turrets
│   ├── Missiles
│   ├── Enemy_Weapons
│   └── Enemy_Effects
│
├── 03_BOSSES
│   ├── Dune_Regent
│   ├── Whiteout
│   ├── Iron_Hart
│   ├── War_Maul
│   ├── Leviathan
│   ├── Omen
│   ├── Boss_Components
│   ├── Boss_Phases
│   └── Boss_Attacks
│
├── 04_PROJECTILES_AND_WEAPONS
│   ├── Player_Projectiles
│   ├── Enemy_Projectiles
│   ├── Boss_Projectiles
│   ├── Secondary_Weapons
│   ├── Bombs
│   └── Weapon_Effects
│
├── 05_ENVIRONMENTS
│   ├── Beach
│   ├── Clouds
│   ├── Snow
│   ├── Hills
│   ├── Ground_Army
│   ├── Sea
│   ├── Fortress
│   ├── Tilesets
│   └── Parallax
│
├── 06_MISSIONS
│   ├── Prologue
│   ├── Story_01
│   ├── Story_02
│   ├── Story_03
│   ├── Story_04
│   ├── Story_05
│   ├── Story_06
│   ├── Remix_01
│   ├── Remix_02
│   ├── ...
│   └── Remix_10
│
├── 07_LEVEL_AUTHORING
│   ├── Spawn_Markers
│   ├── Formation_Previews
│   ├── Altitude_Zones
│   ├── World_Progress
│   ├── Objectives
│   ├── Pitstops
│   └── Checkpoint_Markers
│
├── 08_EFFECTS
│   ├── Explosions
│   ├── Muzzle_Flashes
│   ├── Impact_Effects
│   ├── Hit_Flashes
│   ├── Screen_Shake
│   ├── Particles
│   └── Environmental_Effects
│
├── 09_INTERFACE
│   ├── HUD
│   ├── Menus
│   ├── Mission_Briefing
│   ├── Debrief
│   ├── Hangar
│   ├── Settings
│   ├── Dialogue
│   └── Interface_Components
│
├── 10_AUDIO
│   ├── Music
│   ├── Player_Audio
│   ├── Enemy_Audio
│   ├── Boss_Audio
│   ├── Weapons
│   ├── Impacts
│   ├── UI_Audio
│   └── Ambience
│
├── 11_SEQUENCES
│   ├── Cutscenes
│   ├── Boss_Entrances
│   ├── Mission_Transitions
│   ├── Animations
│   └── Endings
│
├── 12_DATA_AND_CONFIGURATION
│   ├── Player_Configuration
│   ├── Enemy_Configuration
│   ├── Weapon_Configuration
│   ├── Formation_Configuration
│   ├── Mission_Configuration
│   ├── Boss_Configuration
│   ├── Difficulty_Configuration
│   └── Balance_Configuration
│
├── 13_DEBUG_AND_TESTING
│   ├── Debug_Controllers
│   ├── Test_Rooms
│   ├── Visualisation
│   └── Validation
│
└── 14_FUTURE_EXTENSIONS
```

This is an organisational target, not permission to create duplicate resources or move resources recklessly.

Inspect the existing project first. Map current resources to the appropriate groups and retain stable resource references.

Not every conceptual folder requires a GameMaker resource. Documentation, original artwork, manifests and external configuration should remain in appropriate Windows project folders.

## N.3 Consistent naming conventions

Adopt consistent, descriptive naming.

Examples:

| Resource | Naming convention |
|---|---|
| Player object | `obj_player_rookie` |
| Legendary pilot | `obj_player_diamond_ace` |
| Enemy aircraft | `obj_enemy_fighter_basic` |
| Boss | `obj_boss_whiteout` |
| Projectile | `obj_projectile_player_primary` |
| Spawner | `obj_spawner_formation` |
| Altitude marker | `obj_marker_altitude_zone` |
| Mission room | `rm_story_01_beach` |
| Player sprite | `spr_player_rookie_bank_left` |
| Boss sprite | `spr_boss_leviathan_body` |
| Explosion sprite | `spr_fx_explosion_medium` |
| Script | `scr_player_movement_update` |
| Configuration | `cfg_player_weapons` |
| Sequence | `seq_boss_omen_entrance` |
| Sound | `mus_oasis` / `sfx_player_primary_fire` |
| Shader | `shd_hit_flash` |

These names are examples and must be reconciled with existing resources.

Never create duplicate assets with slightly different names when an existing resource can be correctly reused.

Use clear naming for every object, script, room, sprite, sequence, sound, shader and configuration asset.

Avoid meaningless names such as:

- `obj_test2`
- `spr_new`
- `scr_final`
- `obj_enemy_new_fixed`
- `room_copy`
- `data_temp`

## N.4 Resource responsibilities

Every GameMaker object must have a documented responsibility.

For example:

`obj_player_rookie` owns the player's runtime identity and coordinates its components.

It must not contain the entire game engine.

Movement, weapons, animation selection, damage, status, input and visual feedback should be modular systems with clear ownership.

Avoid duplicated movement calculations in different objects.

Avoid multiple systems independently changing the same player state.

Avoid global variables being used as an undocumented communication system.

Document important dependencies, creation order, ownership and cleanup responsibilities.

## N.5 Complete source-to-resource mapping

Create:

`docs/RESOURCE-DEPENDENCY-MAP.md`

For every significant resource, document:

- Resource name.
- Resource type.
- Asset Browser group.
- Source file or source directory.
- Owning gameplay system.
- Parent/related objects.
- Scripts used.
- Configuration data used.
- Mission usage.
- Replacement instructions.
- Validation procedure.
- Known dependencies.

The documentation must make it possible to trace a visual or gameplay feature from its source to its final use in the game.

---

# O. FULL PROJECT 101 DOCUMENTATION

Every major system must have a beginner-friendly guide.

The documentation must be practical, accurate and based on the actual implemented project.

Do not create guides that describe planned features as if they already work.

Every guide must distinguish:

- IMPLEMENTED.
- TESTED.
- IN PROGRESS.
- PLANNED.
- BLOCKED.

## O.1 Required documentation additions

Create or update the following:

### `docs/START-HERE-101.md`

The primary beginner's guide.

Explain:

- What OUTGUN is.
- How to open the correct `.yyp`.
- Which GameMaker IDE/runtime version to use.
- How to find the Asset Browser.
- How to open and edit a sprite.
- How to open an object and inspect its events.
- How to open a script.
- How to open a Room.
- How to run the game.
- How to perform a safe change.
- How to restore a previous version.
- How to build the Windows game.

### `docs/PROJECT-MAP-101.md`

Explain the entire Windows project directory and GameMaker Asset Browser.

Include a hierarchy diagram and descriptions.

### `docs/RESOURCE-NAMING-101.md`

Explain naming conventions and how to name newly created resources.

### `docs/RESOURCE-DEPENDENCY-MAP.md`

Map assets, objects, scripts, rooms, sequences and configurations.

### `docs/ART-REPLACEMENT-101.md`

Explain how to replace artwork without breaking gameplay.

### `docs/ANIMATION-AND-FRAMES-101.md`

Explain Aseprite source files, frame exports, sprite origins, frame order, playback speed, banking animation and replacement.

### `docs/PLAYER-AIRCRAFT-101.md`

Explain movement, banking, shooting, health, damage, lives and weapons.

### `docs/GAMEPLAY-TUNING-101.md`

Explain how to modify speed, acceleration, firing cadence, damage, projectile speed, health, enemy movement and other balance parameters.

### `docs/COMBAT-FEEDBACK-101.md`

Explain muzzle flashes, impact flashes, projectile destruction, enemy damage, explosions, hit reactions, audio and screen shake.

### `docs/GUI-AND-HUD-101.md`

Explain UI resources, HUD layout, fonts, anchors, resolution scaling, menus and replacement.

### `docs/CAMERA-AND-PRESENTATION-101.md`

Explain camera movement, world scrolling, viewport, room dimensions, resolution and transitions.

### `docs/GAMEPLAY-REFERENCE-101.md`

Explain the Shmup Creator reference build, the measurement process, baseline values and deviations.

### `docs/GAMEPLAY-REFERENCE-BASELINE.md`

Store the measured numerical and behavioural reference specifications.

### `docs/TESTING-AND-REGRESSION-101.md`

Explain how to test a change and confirm that previously working systems remain functional.

All previously required v2.0 guides remain mandatory.

## O.2 Guides must be actionable

Every guide should contain:

1. Purpose.
2. Where to find the relevant resource.
3. What the resource controls.
4. Which settings can safely be changed.
5. Which dependencies must be preserved.
6. A step-by-step modification example.
7. How to preview the modification.
8. How to test it.
9. How to restore the original.
10. Common mistakes and troubleshooting.

Use real screenshots from the actual GameMaker IDE where useful.

Use diagrams for complex dependencies, mission flow, weapon emitters, banking frames and level triggers.

Do not use generic stock diagrams that do not represent the implementation.

---

# P. SHMUP CREATOR REFERENCE GAME — BEHAVIOURAL RECONSTRUCTION

This is a core requirement.

The existing Shmup Creator game build is the reference implementation for OUTGUN's original gameplay identity.

Reference executable:

`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_shmup creator build1\_stand-alone_executable\outgun.exe`

Reference directory:

`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_shmup creator build1`

The existing executable must be inspected and played before deciding how the new gameplay should behave.

Do not assume the reference's behaviour based on genre conventions.

Do not guess movement speeds, firing rates, screen dimensions, animation timing or damage behaviour.

Measure and document the actual reference.

## P.1 Establish a reference baseline

First:

1. Verify that the executable exists.
2. Verify its file size, modification date and architecture.
3. Inspect its directory for configuration, data, logs and accompanying resources.
4. Launch the original executable.
5. Determine the intended display resolution and aspect ratio.
6. Inspect all available settings.
7. Identify the actual controls.
8. Play the available gameplay sections.
9. Record the observable behaviour.
10. Capture screenshots and gameplay recordings.
11. Repeat important tests to establish consistency.

Do not modify the original executable or its directory.

Keep the reference build read-only.

Create:

`docs/reference/shmup_creator/`

Store reference captures, notes, recordings, configuration observations and comparison evidence there.

## P.2 Reference measurement categories

Investigate the entire observable gameplay experience.

### A. Input and controls

Record:

- Keyboard controls.
- Mouse controls if present.
- Controller support.
- Movement bindings.
- Primary weapon binding.
- Secondary weapon binding.
- Bomb binding.
- Pause.
- Confirm/back.
- Restart.
- Any focus or speed modifier.
- Input buffering.
- Key-repeat behaviour.
- Simultaneous input behaviour.
- Controller dead zones and sensitivity where applicable.

Reproduce the actual controls before adding optional alternative control schemes.

Preserve user configurability and sensible input defaults.

### B. Player movement

Measure:

- Horizontal speed.
- Vertical speed.
- Acceleration.
- Deceleration.
- Diagonal speed.
- Diagonal normalisation.
- Movement responsiveness.
- Maximum movement bounds.
- Screen-edge behaviour.
- Movement smoothing.
- Banking response.
- Return-to-neutral behaviour.
- Focused movement if present.
- Respawn positioning.
- Invulnerability movement behaviour.

Record values in a clear unit, such as pixels per second or world units per second, with the actual update rate documented.

### C. Aircraft banking and animation

The original aircraft uses banking frames.

This behaviour is important and must be preserved.

Inspect the supplied Aseprite files and PNG exports.

Determine:

- Number of left banking frames.
- Number of neutral frames.
- Number of right banking frames.
- Whether banking is symmetrical.
- Whether the aircraft has separate damage animations.
- Frame order.
- Animation speed.
- Transition behaviour.
- Whether movement input changes animation immediately.
- Whether banking persists while stationary.
- Whether banking uses animation interpolation or discrete frames.

Map the actual frames to the player's movement state.

Do not substitute a generic sprite rotation for authored banking artwork.

Do not discard intermediate frames.

Do not reverse left/right artwork accidentally.

Do not use the wrong banking frame when the player changes direction rapidly.

Use an explicit animation state machine with deterministic transition rules.

Where necessary, use separate animation states for neutral, banking left, banking right, damage, destruction and other existing authored states.

### D. Primary and secondary weapons

Inspect the firing system.

Record:

- Fire input.
- Firing cadence.
- Shots per burst.
- Projectile count.
- Projectile spacing.
- Projectile speed.
- Projectile acceleration.
- Spread.
- Muzzle offsets.
- Left/right emitter positions.
- Muzzle flash artwork.
- Flash duration.
- Projectile appearance.
- Projectile lifetime.
- Hit detection.
- Damage.
- Firing while moving.
- Firing while banking.
- Weapon upgrades.
- Secondary weapon behaviour.
- Bomb behaviour.
- Weapon sound.
- Weapon feedback.

Reproduce the reference weapon feel before balancing or redesigning it.

Then expose the values through editable configuration resources.

### E. Projectile and combat behaviour

Inspect:

- Player projectile movement.
- Enemy projectile movement.
- Projectile collision.
- Damage calculation.
- Invulnerability frames.
- Bullet visibility.
- Hitbox dimensions.
- Enemy hit response.
- Enemy destruction.
- Projectile cancellation.
- Boss projectile behaviour.
- Collision effects.
- Screen shake.
- Sound and visual feedback.

Keep the actual gameplay hitbox distinct from the sprite's visual dimensions where the reference demonstrates that behaviour.

### F. Enemies

Inspect every enemy type available in the reference.

For each enemy, document:

- Appearance.
- Animation.
- Spawn position.
- Entry direction.
- Movement path.
- Speed.
- Health.
- Damage.
- Attack type.
- Fire cadence.
- Projectile pattern.
- Death animation.
- Score reward.
- Collision behaviour.
- Sound.
- Special behaviour.

Use the original supplied sprite assets whenever available.

Do not replace recognisable original enemies with generic placeholders when their artwork exists.

### G. Bosses and large enemies

Inspect available large enemies and boss encounters.

Document:

- Entrance.
- Screen positioning.
- Movement.
- Attack phases.
- Weak points.
- Health display.
- Damage feedback.
- Projectile patterns.
- Phase transitions.
- Destruction.
- Defeat animation.
- Music and audio transitions.

The existing Dune Regent and six-boss creative requirements remain intact.

Reference reconstruction must inform the implementation without reducing the scope of the established campaign.

### H. Camera and level movement

Record:

- Game resolution.
- Aspect ratio.
- Viewport dimensions.
- Room dimensions.
- Camera position.
- Camera follow behaviour.
- Vertical scrolling speed.
- Background movement.
- Parallax speeds.
- Player screen positioning.
- Screen-edge margins.
- Boss camera behaviour.
- Camera shake.
- Mission transition behaviour.

Preserve the original reference's presentation feel while retaining the v2.0 finite, non-looping level architecture.

The reference's visual scrolling behaviour must not force the new finite levels to loop.

### I. HUD and interface

Inspect every available interface element.

Record:

- Score placement.
- Lives.
- Health.
- Bombs.
- Power level.
- Secondary weapon display.
- Boss health.
- Stage information.
- Objective information.
- Menu layout.
- Font style.
- Font size.
- Colours.
- Icon dimensions.
- Screen margins.
- Alignment.
- Animation.
- Transition effects.
- Pause interface.
- Settings interface.
- Game-over interface.

Recreate the original gameplay information hierarchy and visual readability.

Then improve the presentation without removing useful information or making essential indicators harder to understand.

### J. Audio and feedback

Record:

- Player firing sound.
- Projectile impact.
- Enemy destruction.
- Player damage.
- Player destruction.
- Boss entrance.
- Boss attacks.
- Boss destruction.
- UI interaction.
- Music transitions.
- Sound volume relationships.
- Repeated sound behaviour.
- Audio overlap.

Preserve the original OUTGUN soundtrack and all existing music mappings.

The Shmup Creator reference is a gameplay and presentation reference, not permission to replace the established OST.

---

# Q. REFERENCE COMPARISON AND IMPLEMENTATION METHOD

## Q.1 Create a measurable baseline

Create:

`docs/reference/shmup_creator/REFERENCE-OBSERVATIONS.md`

For every measurable feature, record:

| Field | Description |
|---|---|
| Feature ID | Stable identifier |
| Feature | What is being measured |
| Reference behaviour | Actual observed behaviour |
| Measured value | Numerical value where obtainable |
| Measurement method | How it was established |
| Confidence | Verified / estimated / unknown |
| OUTGUN implementation | Current equivalent |
| Difference | Measured deviation |
| Improvement | Proposed enhancement |
| Test evidence | Screenshot, recording or test |

Never invent a numerical measurement.

When a value cannot be measured accurately, record it as unknown and describe the observable behaviour.

## Q.2 Establish comparison captures

Capture equivalent reference and OUTGUN situations.

Examples:

- Player idle.
- Player moving left.
- Player moving right.
- Player moving diagonally.
- Player banking left.
- Player banking right.
- Continuous firing.
- Firing while moving.
- Enemy entering the screen.
- Enemy firing.
- Projectile impact.
- Enemy destruction.
- Player damage.
- Player respawn.
- Boss appearance.
- Boss attack.
- HUD in active gameplay.
- Pause screen.
- Game-over screen.

Use matching display resolutions and comparable game states.

Where possible, compare frame sequences and timestamps rather than relying only on screenshots.

## Q.3 Gameplay reconstruction is not a visual approximation

The target is behavioural fidelity.

The following must be compared:

- Input-to-movement latency.
- Movement speed and acceleration.
- Banking response.
- Projectile timing.
- Projectile spacing.
- Animation cadence.
- Collision response.
- Damage feedback.
- Enemy movement.
- Attack timing.
- Camera movement.
- HUD alignment.
- Screen composition.
- Audio feedback.

A screenshot that looks similar does not prove that gameplay behaves similarly.

## Q.4 Reference fidelity and improvement must coexist

Use three implementation stages:

**Stage 1 — Reference fidelity**

Reproduce the observable original behaviour as closely as possible.

**Stage 2 — Technical improvement**

Improve implementation quality, determinism, maintainability, resource organisation, performance and reliability without changing the intended gameplay feel.

**Stage 3 — Creative improvement**

Add the new OUTGUN campaign, new missions, advanced boss systems, authored patterns, altitude progression, missile objectives, power-ups and other features.

Maintain a documented baseline profile so that optional enhancements can be compared against the original.

Do not silently change the baseline during balancing.

---

# R. ORIGINAL ASEPRITE AND PNG ARTWORK PIPELINE

Source artwork:

`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_real_game_sprites`

Primary sprite folder:

`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_real_game_sprites\Sprites`

The original artwork is an important part of OUTGUN's identity.

## R.1 Inventory Aseprite sources and PNG exports

Recursively inspect:

- `.ase`
- `.aseprite`
- `.png`
- Sprite sheets.
- Individual animation frames.
- Any associated metadata.
- Related reference artwork.

Do not assume PNGs and Aseprite files are interchangeable.

Where both exist, determine whether the PNG is a complete animation export, a static preview, an individual frame or an outdated copy.

Create:

`docs/ASSET-ASSIGNMENT-MAP.md`

and:

`assets/manifests/ASSET-MANIFEST.csv`

## R.2 Preserve source files

Never overwrite the original Aseprite project files.

Never destructively modify original artwork.

Never discard animation frames because they appear visually similar.

Keep source artwork separate from generated exports.

Record the export settings and frame mapping.

## R.3 Banking animation is a first-class gameplay feature

For the player aircraft:

- Identify every banking frame.
- Preserve the original sequence.
- Map frames to the correct directional states.
- Preserve frame timing.
- Preserve transparency.
- Preserve original image dimensions unless an intentional, documented conversion is necessary.
- Configure the GameMaker sprite origin correctly.
- Configure a gameplay collision mask independently from visual artwork.
- Verify that animation transitions work during rapid direction changes.
- Verify that movement and firing do not interfere with animation.

Apply equivalent animation analysis to enemies and bosses where their artwork contains multiple frames.

## R.4 Correct native GameMaker resources

Import the appropriate artwork as actual native GameMaker Sprite resources.

Use Sequences where they provide a genuine advantage for coordinated animations.

Use native Tilesets for appropriate tile-based environments.

Use native Particle System resources where suitable.

Use external files only where there is a clear technical reason.

The final project must remain editable in the GameMaker IDE.

## R.5 Replacement workflow

Every important sprite must have a replacement guide explaining:

1. Original source file.
2. Exported frame files.
3. GameMaker sprite resource.
4. Frame order.
5. Frame speed.
6. Origin.
7. Collision mask.
8. Texture settings.
9. Objects using the sprite.
10. Gameplay states using the animation.
11. How to replace the artwork safely.
12. How to verify the replacement.

Replacing the artwork must not require rewriting the movement or combat system.

---

# S. CONFIGURATION-FIRST GAMEPLAY AUTHORING

The brother should be able to modify most gameplay values without modifying engine code.

Expose suitable values through clearly named configuration resources.

Examples:

**Player configuration**

- Movement speed.
- Focused movement speed.
- Acceleration.
- Deceleration.
- Movement bounds.
- Banking transition speed.
- Health.
- Lives.
- Invulnerability duration.

**Weapon configuration**

- Fire interval.
- Projectile type.
- Damage.
- Speed.
- Spread.
- Burst count.
- Emitter offsets.
- Flash animation.
- Audio.
- Cooldown.

**Enemy configuration**

- Health.
- Movement speed.
- Entry path.
- Attack profile.
- Emitter configuration.
- Score.
- Collision profile.
- Altitude eligibility.

**Boss configuration**

- Health.
- Movement.
- Weak points.
- Phase sequence.
- Attack configuration.
- Projectile profiles.
- Transition events.

**Mission configuration**

- World length.
- Scroll speed.
- Parallax profiles.
- Altitude schedule.
- Spawn markers.
- Formation presets.
- Objectives.
- Music.
- Pitstop.
- Recovery anchors.
- Mission end condition.

Use typed, validated structures and centralised configuration loading.

Do not introduce unnecessary dynamic allocation in gameplay-critical loops.

Validate configuration data before starting a mission.

Provide safe defaults and actionable diagnostics for missing or invalid values.

Do not silently substitute a completely different gameplay configuration when a required asset is missing.

---

# T. VISUAL AUTHORING AND TEST TOOLS

Provide practical tools that reduce the need to edit engine code.

Where possible, implement these as dedicated GameMaker test rooms, editor-placeable instances or clearly separated debug tools.

Required capabilities:

1. Player movement test room.
2. Banking animation preview.
3. Player weapon emitter preview.
4. Enemy movement preview.
5. Enemy formation preview.
6. Boss phase preview.
7. Projectile pattern preview.
8. Hitbox visualisation.
9. Altitude transition preview.
10. Parallax layer preview.
11. Mission trigger visualisation.
12. HUD layout preview.
13. Resolution/aspect-ratio preview.
14. Audio playback and volume test.
15. Save/recovery validation.

Debug functionality must be isolated from release gameplay.

Do not allow debug shortcuts to change normal Story or ranked Remix behaviour.

---

# U. PROFESSIONAL GAMEPLAY QUALITY AND ENGINE STANDARDS

## U.1 GameMaker-native implementation

Use actual GameMaker resources and documented GameMaker Language APIs.

Use the selected runtime's actual supported functionality.

Do not assume that APIs from another engine or an older GameMaker runtime behave identically.

For each non-trivial API:

- Check the official manual.
- Verify its signature.
- Verify parameter types.
- Verify return values.
- Verify runtime compatibility.
- Verify lifecycle and cleanup requirements.
- Verify performance characteristics.
- Test it in the target runtime.

Prefer native GameMaker systems where appropriate:

- Room Editor and layers.
- Sprite Editor.
- Sequence Editor.
- Particle System Editor.
- Sound Editor and audio buses.
- Asset Browser groups and tags.
- GameMaker object events.
- GML scripts and structs.
- Native configuration and project resources.

Do not create custom runtime systems where an appropriate native GameMaker feature already provides a more reliable and maintainable solution.

Conversely, do not force native visual editors to handle systems that are better represented by clear reusable GML and data-driven configurations.

## U.2 GameMaker documentation hierarchy

Use:

1. Official GameMaker LTS 2026 manual.
2. Official GameMaker release notes.
3. Official GameMaker Help Centre.
4. Official GameMaker tutorials.
5. Official GameMaker examples where relevant.

Do not use outdated forum snippets as authoritative API documentation.

If a forum solution is useful, independently verify it against the installed runtime and official documentation.

Document any compatibility workaround.

## U.3 Architecture and performance

Maintain:

- Clear ownership of runtime state.
- Deterministic update ordering.
- Explicit collision rules.
- Stable resource references.
- Safe cleanup.
- Validated input.
- Controlled memory allocation.
- Reusable emitters.
- Reusable movement profiles.
- Reusable formation definitions.
- Data-driven missions.
- Isolated debug systems.
- Consistent save/load behaviour.

Avoid:

- Giant Step events.
- Duplicated movement logic.
- Repeated global lookups in hot loops.
- Unnecessary `with` blocks in performance-critical code.
- Uncontrolled instance creation.
- Unbounded arrays and lists.
- Per-frame resource loading.
- Hidden cross-system dependencies.
- Unvalidated configuration data.
- Silent asset fallback.
- Unnecessary runtime-generated substitutes for editable resources.

Profile before making performance claims.

## U.4 Resolution and scaling

Reproduce the reference's original resolution and presentation as the baseline.

Then provide a documented, deliberate scaling strategy for supported resolutions.

Test:

- Original reference resolution.
- 16:9 resolutions.
- 16:10 where appropriate.
- Ultrawide where supported.
- Windowed mode.
- Fullscreen mode.
- Resizing.
- UI anchoring.
- Camera and viewport alignment.

Preserve the original gameplay playfield proportions unless a documented design decision requires otherwise.

Do not stretch the aircraft or distort the HUD to fill different aspect ratios.

---

# V. REGRESSION AND NON-DESTRUCTIVE IMPROVEMENT

Every gameplay improvement must be tested against the existing implementation.

Create:

`docs/TESTING-AND-REGRESSION-101.md`

and update:

`docs/BUILD-AND-TEST-REPORT.md`

## V.1 Required player tests

- Idle animation.
- Left movement.
- Right movement.
- Up movement.
- Down movement.
- Diagonal movement.
- Banking left.
- Banking right.
- Rapid direction changes.
- Movement at screen boundaries.
- Continuous firing.
- Firing while moving.
- Firing while banking.
- Secondary weapon.
- Bomb.
- Collision.
- Damage.
- Invulnerability.
- Death.
- Respawn.
- Game over.
- Restart.

## V.2 Required combat tests

- Every projectile type.
- Every emitter type.
- Enemy hit registration.
- Enemy destruction.
- Boss damage.
- Boss weak points.
- Boss phase transitions.
- Explosion animation.
- Hit flash.
- Bullet pattern timing.
- Projectile cleanup.
- Screen shake.
- Audio feedback.

## V.3 Required environment tests

- Camera movement.
- Parallax movement.
- Finite stage ending.
- Altitude transitions.
- Spawn markers.
- Formation placement.
- Ground-unit eligibility.
- High-altitude flight-only restriction.
- Boss entrance.
- Mission completion.

## V.4 Existing mission and campaign regression

Preserve and retest:

- One-time Prologue.
- Six Story missions.
- Ten Remix missions.
- Mission 01 missile defusal.
- Six boss concepts.
- One Pitstop per Story mission.
- Separate Story autosave anchors.
- No Remix Pitstops.
- Deterministic Story/Remix encounters.
- Seeded Remix power-ups.
- Permanent Hangar upgrades.
- Ranked Remix normalisation.
- Save/load and recovery.
- Music mapping.

## V.5 Reference comparison acceptance

The new implementation must be compared against the original Shmup Creator executable.

Use a feature-by-feature comparison report.

Do not mark reference fidelity as complete merely because the game launches or visually resembles the original.

Each comparison must include evidence and a status.

---

# W. UPDATED DOCUMENTATION AND PROJECT TRACKER

Preserve all existing tracker entries.

Add the following sections:

### Project organisation

- [ ] Asset Browser fully organised.
- [ ] Resource naming conventions applied consistently.
- [ ] Resource dependencies mapped.
- [ ] Source artwork mapped to native GameMaker resources.
- [ ] Beginner guides completed.
- [ ] Replacement instructions completed.
- [ ] Actual IDE screenshots and diagrams added where useful.

### Reference reconstruction

- [ ] Reference executable verified.
- [ ] Reference build inspected.
- [ ] Original controls documented.
- [ ] Player movement measured.
- [ ] Banking animation mapped.
- [ ] Weapon behaviour measured.
- [ ] Enemy behaviours documented.
- [ ] Projectile behaviour documented.
- [ ] Collision and damage documented.
- [ ] HUD and GUI documented.
- [ ] Camera and resolution documented.
- [ ] Audio and feedback documented.
- [ ] Baseline comparison report completed.
- [ ] OUTGUN reference-fidelity implementation tested.

### Artwork pipeline

- [ ] Aseprite sources inventoried.
- [ ] PNG exports inventoried.
- [ ] Banking frames mapped.
- [ ] Sprite origins verified.
- [ ] Collision masks verified.
- [ ] Native sprite resources assigned.
- [ ] Replacement workflow tested.
- [ ] Original source artwork preserved.

### Quality and testing

- [ ] GameMaker LTS/runtime verified.
- [ ] Official API compatibility verified.
- [ ] Player regression suite passed.
- [ ] Combat regression suite passed.
- [ ] Camera/resolution tests passed.
- [ ] Mission regression passed.
- [ ] Save/recovery regression passed.
- [ ] Performance profiling completed.

Continue using the original status vocabulary:

`NOT STARTED`, `IN PROGRESS`, `IMPLEMENTED`, `TESTED`, `BLOCKED`.

Do not convert `IMPLEMENTED` into `TESTED` without evidence.

---

# X. UPDATED IMPLEMENTATION ORDER

Preserve the existing M0–M6 milestones and their non-conflicting requirements.

Add the following work before expanding the gameplay implementation further.

## X.1 Immediate preservation and inspection

1. Inspect the authoritative `.yyp`.
2. Inspect the entire current project.
3. Preserve the existing Git state.
4. Inspect the original Shmup Creator executable.
5. Inspect the complete reference build directory.
6. Inspect all original Aseprite and PNG artwork.
7. Inspect the current build and audit records.
8. Record the installed GameMaker IDE/runtime.
9. Establish the reference gameplay baseline.
10. Identify existing implementation gaps.

Do not restart or replace the current build.

## X.2 Source project and organisation

1. Verify the authoritative source project.
2. Resolve native resource assignment issues.
3. Establish the Asset Browser organisation.
4. Establish naming conventions.
5. Build the dependency map.
6. Map original artwork to native resources.
7. Document all existing systems.
8. Verify that the actual IDE workflow works.

## X.3 Reference gameplay reconstruction

1. Implement the measured player movement baseline.
2. Implement the banking animation mapping.
3. Implement the measured weapon behaviour.
4. Reproduce projectile behaviour.
5. Reproduce combat feedback.
6. Reproduce reference enemy behaviour.
7. Reproduce camera and scrolling feel.
8. Reproduce HUD and presentation.
9. Compare and correct discrepancies.
10. Preserve all improvements and original systems.

## X.4 Continue the established OUTGUN roadmap

Continue the original milestones with the existing creative scope:

- Prologue.
- Story Mission 01.
- Story Missions 02–06.
- Ten Remix missions.
- Optional Dynamic Operations.
- Boss architecture.
- Pitstops and recovery.
- Permanent upgrades.
- Co-op and platform extensions.
- Final QA and release preparation.

Do not postpone the reference reconstruction until after building more unrelated content.

Do not delay the first playable missile-defusal mission.

---

# Y. REQUIRED FINAL HANDOFF ADDITIONS

In addition to all existing handoff requirements, report:

1. Whether the authoritative `.yyp` remains the sole source project.
2. Whether the actual GameMaker IDE opens the project.
3. Whether the Asset Browser is organised and understandable.
4. Whether every important resource has a meaningful name and responsibility.
5. Whether the resource dependency map is complete.
6. Whether original artwork is preserved.
7. Whether all banking frames are mapped correctly.
8. Whether replacing a sprite works without rewriting gameplay code.
9. Whether the original reference executable was successfully inspected.
10. The measured reference resolution and aspect ratio.
11. The measured reference controls.
12. The measured player movement values.
13. The banking animation and transition behaviour.
14. The measured weapon firing behaviour.
15. Projectile and combat feedback comparisons.
16. Enemy behaviour comparisons.
17. HUD and camera comparisons.
18. Reference audio and presentation observations.
19. A list of reference differences still outstanding.
20. A list of improvements implemented without breaking baseline behaviour.
21. Regression test results.
22. Build and runtime verification.
23. Remaining blocked or unverified requirements.

**The project is not complete merely because the executable runs.**

The actual deliverable is a complete, playable, maintainable, professionally organised GameMaker source project that can be opened and edited directly in the GameMaker IDE.

The creator must be able to understand it, navigate it, replace its art, adjust its gameplay, edit its rooms, change its encounters and extend its systems without having to reverse-engineer the original implementation.

**Preserve everything. Measure before changing. Improve without regression. Document everything. Build the actual game inside the actual GameMaker project.**


Continue from the existing OUTGUN session and implementation. Do not treat this as a fresh project or a request to rewrite everything.

You are the Orchestrator for this project. Coordinate the full directive reconciliation, existing-project audit, Shmup Creator reference analysis, original asset audit, GameMaker architecture and subsequent implementation.

Start with M0. Delegate independent investigations to appropriately specialized agents where useful, but establish clear ownership of files before allowing implementation agents to write. Prevent conflicting parallel modifications.

Preserve all existing working features and user changes. Do not start a competing project. The canonical source remains Outgun_Framework.yyp.

Your first priority is to establish the actual project state and reconcile every available directive. After that, execute the milestones in order, with verified builds, tests and documented results.

Do not stop after producing a plan. Continue into implementation as dependencies and evidence permit.

# OUTGUN — MASTER DIRECTIVE INTAKE, RECONCILIATION AND EXECUTION ORDER

You are working on **OUTGUN**, a professional, fully editable GameMaker shoot-'em-up framework and game project.

Your task is to inspect, reconcile, organize, implement, test, and document **all OUTGUN requirements and Markdown directives supplied to this project**. Do not work from only the latest message or only one master document. The complete collection of supplied requirements governs the project.

This is an execution order, not a request for a proposal or another high-level plan. Begin by examining the real workspace and its existing implementation. Then proceed through the work in controlled, verifiable stages.

---

## 1. Canonical project — mandatory

The actual GameMaker project is:

`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_Framework.yyp`

Canonical project directory:

`C:\Users\Administrator\GameMakerProjects\Outgun_Framework`

Reference game executable:

`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_shmup creator build1\_stand-alone_executable\outgun.exe`

Original sprite source directory:

`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_real_game_sprites\Sprites`

Additional asset directory:

`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Assets for Use`

Original soundtrack directory:

`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_ost`

**The `.yyp` project above is the actual product being developed.** All real GameMaker resources, rooms, objects, scripts, sequences, sprites, shaders, fonts, sounds, configurations and project metadata must belong to this canonical project.

A directory such as:

`C:\Users\Administrator\GameMakerProjects\_build_out`

may be used for compiler output, temporary files, caches or generated build artifacts where appropriate. It must not silently become a substitute source project or the place where the actual game is implemented.

The existing compiler invocation appears to compile the canonical `Outgun_Framework.yyp` while using `_build_out` for intermediate output. Verify the actual paths and behavior rather than assuming that `_build_out` is a separate project.

Never create a second competing OUTGUN project and call it the completed framework.

## 2. Read and register every supplied Markdown directive

Before changing implementation code, perform a complete directive inventory.

Search the canonical project directory, its applicable documentation and directive folders, and the files attached or explicitly supplied to this OpenCode task. Identify all OUTGUN-related Markdown instructions, including:

- `OUTGUN-OpenCode-Master-Directive.md`
- The v2.0 creative/gameplay addendum.
- The subsequent Shmup Creator reference-game fidelity requirements.
- The subsequent asset preservation, sprite banking, project organization and beginner-friendly documentation requirements.
- Any additional OUTGUN-specific Markdown files, amendments, pasted specifications or implementation instructions supplied in this task.

Do not assume that a document exists simply because it is mentioned. Locate it and verify its contents.

Read each applicable document completely. Do not rely on filenames, summaries, previous AI descriptions, excerpts, or assumptions about what a document contains.

For each document, record:

- Exact path and filename.
- Whether it is an original directive, amendment, technical specification, reference, implementation record or supporting document.
- Its version or date, if actually stated.
- Whether it was read completely.
- Its relationship to the other directives.
- Its SHA-256 hash where practical, so the input can be identified unambiguously.

Create:

`docs/DIRECTIVE-INPUT-REGISTER.md`

This register must identify every directive actually found and read. If an expected document is unavailable, explicitly record it as **MISSING / NOT ACCESSIBLE**. Do not invent its contents or claim to have reviewed it.

Important: Do not assume that OpenCode can access previous ChatGPT conversation messages. Only treat pasted instructions, attached materials and files actually available in the current OpenCode environment as inspected sources. Record unavailable conversation-only material as unavailable rather than pretending it was read.

## 3. Preserve every requirement — additive development only

This is a strict preservation project.

**Do not delete, shorten, silently omit, weaken, or replace any non-conflicting requirement from the supplied directives.**

The original master directive must remain intact in substance. The v2.0 addendum and subsequent requirements must be incorporated without discarding previous requirements.

Rules:

1. Preserve all existing functionality, implementation, resources and documentation unless a specific change is genuinely necessary.
2. Newer explicit creative decisions supersede older conflicting creative decisions only where an actual conflict exists.
3. Non-conflicting requirements from older directives remain fully applicable.
4. When two instructions conflict, identify the exact conflicting statements and explain the resolution.
5. Do not resolve a conflict by deleting the older requirement without documenting what happened.
6. Do not remove working features just to simplify the architecture.
7. Do not perform mass renames, mass migrations, resource deletions or project restructuring without first mapping their dependencies and preserving references.
8. Never mark a feature implemented merely because a placeholder, mockup, menu entry or empty function exists.

Create or update:

- `docs/DIRECTIVE-RECONCILIATION.md`
- `docs/DECISIONS.md`
- `docs/EXECUTION-STATE.md`
- `docs/IMPLEMENTATION-TRACKER.md`

The reconciliation document must provide traceability from each requirement to its resulting specification section, implementation location and verification evidence.

The decisions document must record genuine conflicts, the selected resolution and the affected features.

The implementation tracker must distinguish:

- `NOT STARTED`
- `IN PROGRESS`
- `IMPLEMENTED`
- `TESTED`
- `BLOCKED`

Do not use `IMPLEMENTED` and `TESTED` interchangeably.

## 4. Protect the existing project and implementation

Before modifying anything:

1. Identify the canonical project root and verify the `.yyp`.
2. Inspect Git status, current branch, HEAD commit, tracked changes, staged changes and untracked files.
3. Identify existing resources, scripts, rooms, objects, configurations and documentation.
4. Identify any previous implementation that is incomplete but still valuable.
5. Record the initial state in `docs/EXECUTION-STATE.md`.
6. Establish which files are already modified by the user or earlier development work.

Do not discard, reset, overwrite or clean existing user changes.

Never run destructive Git operations such as `git reset --hard`, `git clean -fd`, or indiscriminate checkout/restore commands.

Do not replace an existing working system with a newly written simplified version without proving why the change is necessary.

Preserve useful existing code. Refactor only when justified by a concrete defect, architectural problem, performance issue or requirement.

If a migration is necessary, provide a safe migration path and preserve the original data/resources until the replacement has been verified.

## 5. Reference-game fidelity — inspect the real Shmup Creator game

The Shmup Creator executable is the behavioral and visual reference for the original OUTGUN gameplay.

Reference executable:

`C:\Users\Administrator\GameMakerProjects\Outgun_Framework\Outgun_shmup creator build1\_stand-alone_executable\outgun.exe`

The goal is to reproduce the reference game's actual gameplay feel and presentation in the real GameMaker project, then improve it without losing its recognizable behavior.

Do not guess what the original game does. Run and inspect it where possible.

Capture and document the original behavior, including:

**Player aircraft**
- Movement and directional response.
- Acceleration, deceleration, turning and stopping behavior.
- Left/right banking and neutral flight.
- Banking animation frames, frame order, timing and transitions.
- Speed, responsiveness and movement boundaries.
- Shooting while moving, turning and banking.
- Player damage, hit response, invulnerability and death behavior.

**Weapons and effects**
- Fire-button behavior and firing cadence.
- Projectile count, positions, speed, direction, spread and lifetime.
- Muzzle locations, muzzle flashes and firing effects.
- Bullet visuals, trails and hit effects.
- Impact feedback, explosions and enemy destruction.
- Sound synchronization and audio feedback.

**Enemies and encounters**
- Enemy appearance and placement.
- Movement paths and formations.
- Attack patterns and projectile behavior.
- Collision, damage and destruction.
- Spawn timing, difficulty pacing and encounter transitions.
- Boss behavior and major encounter effects.

**Camera and environment**
- Camera position and movement.
- Horizontal and vertical scrolling behavior, as applicable.
- Background composition and parallax.
- Player position relative to the visible play area.
- Level arrangement, visual scale and screen composition.
- Environmental effects and transitions.

**HUD, menus and presentation**
- Resolution and aspect ratio.
- Windowed and fullscreen behavior.
- HUD positioning, proportions and visibility.
- Health, lives, score, bombs, weapons and other displayed information.
- Menus, pause screen, game-over screen and transitions.
- Settings and input configuration.
- Text size, visual hierarchy and readability.

**Controls and input**
- Actual keyboard/controller bindings.
- Input responsiveness and simultaneous input behavior.
- Shooting and movement combinations.
- Pause, menu navigation and other supported controls.

Build a reference-game observation document:

`docs/REFERENCE-GAME-PLAYTEST-BASELINE.md`

Measure where possible. Use repeatable playtests, screenshots, video or frame captures, controlled input tests and recorded timings.

When a value cannot be measured reliably, mark it as unknown. Never invent exact timings, speeds, resolutions or control bindings.

Create:

`docs/REFERENCE-GAME-PARITY-MATRIX.md`

For each observed behavior, record:

- Reference behavior.
- Evidence and measurement.
- Corresponding GameMaker resource or system.
- Current implementation status.
- Difference from the reference.
- Required improvement.
- Test procedure.
- Verification result.

The result should preserve the original game's identity, motion, feedback, pace and presentation while improving quality, responsiveness, reliability, extensibility and maintainability.

**Do not replace the original behavior with generic shoot-'em-up conventions simply because they are easier to implement.**

## 6. Preserve and correctly implement the original sprite artwork

Inspect the original sprite source directory recursively:

`Outgun_real_game_sprites\Sprites`

Inventory all Aseprite files, PNG files and other supported source formats.

For each source asset, inspect:

- Dimensions.
- Transparency.
- Layers.
- Frames.
- Frame order.
- Frame durations.
- Animation tags.
- Origins and intended alignment.
- Intended role.
- Related animation states.
- Whether the asset is a complete animation or a single frame.

The aircraft's banking frames are particularly important.

Treat banking frames as actual gameplay animation states, not disposable decoration.

Preserve the original artwork and its animation. Determine the correct neutral, left-bank, right-bank and transition behavior from the source files and reference-game playtest. Reproduce the intended animation and timing accurately.

Do not replace genuine banking animation with a static sprite, arbitrary rotation or invented animation unless the reference and source assets demonstrate that this is appropriate.

Keep visual sprites and collision masks logically independent. Avoid collision behavior changing unintentionally because an animation frame changes.

Preserve original files byte-for-byte. Generated or converted GameMaker assets must be traceable to their source files.

Create:

- `docs/ASSET-ASSIGNMENT-MAP.md`
- `assets/manifests/ASSET-MANIFEST.csv`
- Appropriate sprite contact sheets and visual audit material.

The asset map must explain where each original asset is used, which GameMaker resource represents it, and how another developer can replace it safely.

## 7. Apply the complete gameplay specification from the directives

Do not treat the following as a replacement for reading the original directives. They are mandatory cross-checks so that major requirements are not overlooked.

### Prologue and Story

- The Prologue is a one-time, non-replayable playable opening.
- It is not a tutorial or Training mode.
- It features playable Diamond Ace and ends with the scripted unwinnable OMEN encounter.
- Persist the relevant completion/seen state, including `prologue_completed` and `prologue_seen`.
- Exactly six Story missions exist: `story_01` through `story_06`.
- The Prologue does not count as one of the six missions.

### Story mission progression

Preserve the specified sequence:

1. Mission 01: shallow beach, beach, Dune Regent launch platform, cloud ascent, authored enemy waves and close-range missile defusal.
2. Mission 02: snow environment and WHITEOUT.
3. Mission 03: hills environment and IRON HART.
4. Mission 04: ground army and WAR MAUL.
5. Mission 05: sea environment, ABYSSAL CROWN and LEVIATHAN tentacles.
6. Mission 06: fortress/black-site and OMEN final boss.

For Mission 01, preserve the specified Dune Regent behavior: the platform launches a missile toward city coordinates and escapes; it is not fought as a conventional boss in that encounter.

The missile defusal must use the specified close-radius objective. Defusal progress pauses when the player leaves the required radius but does not reset. Taking a hit does not automatically erase all progress. The encounter must include fixed orbital enemies, one-shot `MISSILE_DEFUSED` behavior, a fair deadline, failure handling and retry.

The Dune Regent concept and assets remain available for appropriate later use, including Remix or a future return.

### Remix/Arcade

- Exactly ten Remix missions: `remix_01` through `remix_10`.
- Each has a distinct authored encounter identity.
- Target duration is 3–5 minutes, with a five-minute ceiling.
- No Pitstops or checkpoints.
- Encounters use fixed schedules and repeatable authored patterns.
- Temporary random pickups use controlled, recorded seeds.
- Each mission has an appropriate boss or boss-like climax.
- Ranked statistics use a normalized baseline.

### Encounter design

Story and Remix must use fixed, authored, memorisable encounter patterns.

Adaptive or dynamic encounter logic belongs only to the separate optional Dynamic Operations mode. It must never silently alter Story or Remix patterns, schedules or difficulty.

### Level construction and parallax

Use finite authored level paths instead of indefinitely looping entire stage backgrounds.

Parallax layers move at distinct configured speeds. Repeating individual decorative textures is acceptable only when explicitly configured.

The mission progress coordinate is the primary encounter clock.

Use editor-placed Room markers or validated data with unique stable IDs, deterministic priority/ID ordering, frame-spike handling, a trigger ledger and save/resume support.

### Spawners

Support reusable formation presets, including:

- Single.
- Horizontal and vertical.
- Both diagonal directions.
- V and inverted V.
- Wedge.
- Staggered.
- Double column.
- Diamond and box.
- Arc.
- Circular and orbital.
- Split.
- Custom waypoint formations.

Provide a designer preview and test room.

Create `docs/SPAWNER-101-GUIDE.md`.

### Weapons and emitters

Use one configurable emitter system shared by the player, enemies and bosses.

Player weapons must not be hardcoded as scattered Step-event logic.

Support editable muzzle offsets, projectile type, cadence, bursts, spread, angle, speed, acceleration, damage, lifetime, visuals, trails, sounds, cooldowns and weapon groups.

Create `docs/EMITTERS-AND-WEAPONS-101.md`.

### Altitude

Implement LOW, MEDIUM and HIGH altitude states with authored progress markers.

- LOW: ground and aerial encounters.
- MEDIUM: mixed encounters.
- HIGH: flight-only encounters and more advanced aerial archetypes.

Display altitude clearly in the HUD. Transitions must not abruptly spawn, delete or reposition entities unfairly.

Create `docs/ALTITUDE-SYSTEM-101.md`.

### Upgrades and pickups

Separate permanent Hangar upgrades, authored repeatable Story pickups and seeded temporary Remix pickups.

Ranked Remix runs must use a normalized baseline and track perfect-run metrics and eligibility.

Create `docs/POWERUPS-AND-UPGRADES-101.md`.

### Pitstops and safe-save anchors

Exactly one named and visible Pitstop exists in each Story mission.

Keep silent automatic safe-save anchors separate from visible Pitstops. Preserve the specified mission-start, midpoint where applicable, post-miniboss, pre-final-boss and major-event safe points.

Save the exact required state atomically and validate it on loading.

Remix has no Pitstops or checkpoints.

Create `docs/PITSTOP-AND-CHECKPOINTS-101.md`.

### OST and audio

Inventory the original OST and identify the actual soundtrack track one. The Oasis track is soundtrack track one, but the first visual environment remains coastal; do not force Oasis visuals into Mission 01.

Preserve the original soundtrack byte-for-byte. Do not fabricate BPM values; measure them or mark them unknown.

Create `docs/AUDIO-ASSET-MAP.md`.

## 8. Project organization — make the framework understandable

The framework must be easy to navigate even though its systems are sophisticated.

Organize resources by clear responsibility and provide a predictable mapping between folders, GameMaker resources, gameplay systems and documentation.

A developer should be able to answer quickly:

- Where is the player movement implemented?
- Where are aircraft banking animations configured?
- Where are player weapons and muzzle effects configured?
- Where are enemy formations created?
- Where are encounter schedules authored?
- Where are boss phases and attacks defined?
- Where are altitude transitions configured?
- Where are Story and Remix missions defined?
- Where are Pitstops and save anchors handled?
- Where is the HUD and menu layout configured?
- Where is each original sprite or sound assigned?
- How can an asset be replaced without breaking references?
- How can a new enemy, boss, weapon, formation or mission be added?

Do not organize by arbitrary alphabetic grouping alone. Organize by purpose, responsibility and workflow.

Use descriptive, consistent names. Avoid confusing abbreviations and generic names such as `obj_controller2` or `scr_helper`.

Before renaming or moving any existing resource, inspect all references and dependencies. Preserve stable IDs and project references wherever possible.

Where the existing project structure is already sound, retain it and document it rather than restructuring for cosmetic reasons.

Required beginner-friendly guides:

- `docs/START-HERE-101.md`
- `docs/PROJECT-MAP-101.md`
- `docs/ASSET-ASSIGNMENT-MAP.md`
- `docs/LEVEL-AUTHORING-101.md`
- `docs/SPAWNER-101-GUIDE.md`
- `docs/ALTITUDE-SYSTEM-101.md`
- `docs/EMITTERS-AND-WEAPONS-101.md`
- `docs/PITSTOP-AND-CHECKPOINTS-101.md`
- `docs/STORY-AND-REMIX-101.md`
- `docs/POWERUPS-AND-UPGRADES-101.md`
- `docs/MISSION-AND-BOSS-AUTHORING-101.md`

These must be genuine guides for someone new to the project, not superficial descriptions.

Explain the relationship between the GameMaker Asset Browser, filesystem folders, scripts, objects, rooms, sequences, data definitions and runtime behavior. Include exact resource names and actual paths from the implemented project.

For asset replacement, explain the source file, GameMaker resource, dimensions, origins, animation frames, collision considerations, references and safe replacement procedure.

Do not document a fictional structure. Documentation must match the actual project.

## 9. GameMaker compatibility and coding standards

Use the actual installed GameMaker IDE, project format and runtime.

The directives specify a GameMaker LTS 2026 baseline, while the previously supplied compiler command showed runtime `2024.14.4.268`. This difference must be investigated. Verify the installed IDE and runtimes, project metadata and the runtime used for the real build.

Do not silently assume the required runtime is installed. Do not upgrade the project or use APIs from another runtime without verifying compatibility and documenting the decision.

Use official GameMaker documentation matching the runtime actually selected. Verify API names, signatures, lifecycle, resource behavior and supported features.

All GML must be valid for that runtime.

Code requirements:

- Clear object and script responsibilities.
- Small, focused scripts rather than enormous event blocks.
- State machines for complex behavior.
- Alarms and timers where appropriate.
- Cached references instead of repeated expensive lookups in Step.
- Safe instance and resource checks.
- Validated arrays, structs, buffers and save data.
- Deterministic gameplay where required.
- Controlled resource creation and cleanup.
- No unnecessary allocations in frequent loops.
- No duplicated systems, unused variables or deprecated code.
- No placeholder logic presented as complete behavior.
- Consistent naming and useful comments for non-obvious logic.
- Centralized diagnostics and graceful handling of missing or invalid resources.

Every script must document its purpose, parameters and return value where applicable. Every major object and system must have a documented responsibility and lifecycle.

## 10. Required project documentation and tracking

Maintain the following records as living project documentation:

`docs/DIRECTIVE-INPUT-REGISTER.md`

`docs/DIRECTIVE-RECONCILIATION.md`

`docs/DECISIONS.md`

`docs/EXECUTION-STATE.md`

`docs/IMPLEMENTATION-TRACKER.md`

`docs/REFERENCE-GAME-PLAYTEST-BASELINE.md`

`docs/REFERENCE-GAME-PARITY-MATRIX.md`

`docs/ASSET-ASSIGNMENT-MAP.md`

`docs/AUDIO-ASSET-MAP.md`

`assets/manifests/ASSET-MANIFEST.csv`

Do not create duplicate documents with competing information if equivalent authoritative documents already exist. Inspect and update the existing document where appropriate.

The execution-state document must always show:

- Current branch and commit.
- Canonical project path.
- Selected GameMaker version/runtime.
- Current build status.
- Current test status.
- Completed work.
- In-progress work.
- Blocked work and exact reason.
- Changed files and resources.
- Known defects.
- Next actionable task.

## 11. Execute the milestones in order

Use the original M0–M6 milestone structure. Preserve every additional milestone or requirement present in the original directives.

**M0 — Audit and establish the baseline**

Inspect the complete project, all directives, assets, soundtrack, reference executable, Git state, installed GameMaker version and existing implementation.

Produce the directive register, reconciliation, decisions, execution state, asset inventory, reference-game baseline and implementation tracker.

Do not begin destructive restructuring.

**M1 — Core framework**

Implement or complete the necessary project shell, input, validated save system, progress director, finite scrolling, parallax, altitude, emitters, weapons and Hangar foundations.

Reuse and improve existing implementations where possible.

**M2 — Playable Prologue and Mission 01**

Implement the actual Prologue and full Mission 01, including the authored beach/cloudline progression, missile launch, enemy waves, close-range defusal, debrief and Hangar flow.

Verify the actual gameplay against the reference where applicable.

**M3 — Remaining Story missions**

Implement Missions 02–06, preserving the specified mission order, encounters, bosses and progression.

**M4 — Ten Remix missions**

Implement all ten fixed, repeatable Remix missions with the specified duration, seed policy, pickup rules, boss climaxes, ranking and no-checkpoint rules.

**M5 — Dynamic Operations and additional systems**

Implement Dynamic Operations only as a separate mode after fixed Story and Remix behavior is stable. Continue the remaining co-op, platform, achievement and polish requirements from the original directives without dropping them.

**M6 — Verification and release readiness**

Complete compatibility, regression, gameplay, asset, save/load, performance, presentation and release testing. Prepare the documented GitHub/release state as required by the directives.

Do not claim a milestone is finished merely because its documentation has been written.

## 12. Verification is mandatory

Compile the actual canonical `.yyp` project.

Verify that the compiler is using the intended project file, the intended runtime and the correct source resources. Temporary output directories do not count as the source project.

Test the actual game, not only individual scripts or isolated prototypes.

At minimum verify:

- Project opens correctly in the GameMaker IDE.
- Project compiles without errors.
- No new warnings are introduced without explanation.
- Player movement and banking match the measured reference behavior.
- Shooting cadence, projectile placement and muzzle effects are correct.
- Collision, damage, invulnerability and death work.
- Enemies, formations and boss phases behave correctly.
- Encounter timing is deterministic in Story and Remix.
- Altitude transitions are correct.
- Missile defusal behaves correctly under movement, damage and timeout conditions.
- Story Pitstops and automatic safe saves restore the intended state.
- Remix contains no checkpoints and uses the intended seed policy.
- Save data is validated and corrupted or incompatible data fails safely.
- Audio and animation timing are correct.
- HUD, menus, camera and resolution are correct.
- Replacing an asset using the documented workflow does not break references.
- Existing features have not regressed.

For each test, report the exact test procedure and observed outcome. Distinguish automated tests, manual tests, static inspection and unverified assumptions.

If the reference executable cannot be launched or a test cannot be performed, mark the affected item `BLOCKED` or `NOT TESTED` and state exactly why.

Never fabricate screenshots, measurements, build success, test results or parity claims.

## 13. Work execution rules

Work in small, logically grouped, verifiable batches.

After each batch:

1. Inspect the changed files.
2. Check references and dependencies.
3. Compile the canonical project where appropriate.
4. Run relevant tests.
5. Review the Git diff.
6. Update the implementation tracker and execution state.
7. Record any unresolved issue.

Do not stop after producing a plan, folder structure, set of empty scripts or attractive UI shell. Continue to actual implementation within the available environment and the safe scope of the current work.

Do not invent missing source assets, reference behavior, measurements or requirements. Record missing inputs and work around them only where a safe, clearly documented implementation is possible.

Do not ask for approval for routine, reversible implementation work. Do not perform destructive or irreversible operations without explicit authorization.

Prioritize correctness and preservation over speed. Avoid unnecessary rewrites and duplicated systems.

## 14. Final handoff requirements

At every execution checkpoint, report:

1. Which directive files were actually found and completely read.
2. Which requirements were reconciled and where they are documented.
3. Which existing implementation was preserved.
4. Which files, resources and systems were changed.
5. Which reference-game behaviors were measured and reproduced.
6. Which original assets were imported and mapped.
7. Which systems are implemented and which are genuinely tested.
8. The exact build and test results.
9. Remaining blockers and missing inputs.
10. The next concrete implementation task.

The final handoff must include evidence for mission counts, encounter schedules, trigger IDs, altitude schedules, formation presets, emitter presets, Pitstop/save anchors, deterministic behavior, asset mappings, banking animations, audio mappings and reference-game parity.

**The objective is one coherent, professionally engineered, editable GameMaker OUTGUN project whose implementation, documentation, original assets and actual gameplay all agree.**

Begin with M0 now. Inspect first, establish the real baseline, reconcile all available directives, and then continue implementation without sacrificing existing work.
