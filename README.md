# **[mod-playerbots-auctioneer-pvp-life](https://github.com/electrobutterfly/mod-playerbots-auctioneer-pvp-life)**

![](assets/icon.png)  

# PvP Life (Comprehensive Autonomous World PvP & Duel System) — FORK

![PvP Life Banner](assets/banner.png)

> ### ⚠ FORK NOTICE ⚠
>
> **This is a fork of [mod-playerbots-pvp-life](https://github.com/Ildourol/mod-playerbots-pvp-life), maintained for compatibility with
> the [azerothcore-wotlk-playerbots](https://github.com/electrobutterfly/azerothcore-wotlk-playerbots) fork and its [mod-playerbots-auctioneer](https://github.com/electrobutterfly/mod-playerbots-auctioneer)
> companion module. Do not use it with the upstream `mod-playerbots` repository.**
>
> The playerbot fork that this module is built against has diverged from upstream
> in several interfaces PvP Life depends on — `TravelMgr`, `PlayerbotAIConfig`,
> `NewRpgInfo`, `ItemUsageValue`, and `PlayerbotAI` itself (new fields, new
> methods, and a handful of changed signatures). Building upstream PvP Life
> against this fork either fails to compile or silently misbehaves.
>
> In addition, `mod-playerbots-auctioneer` does not participate in the shared
> `PlayerbotsLife` reservation registry that PvP Life uses to avoid stealing
> bots from other modules. When both modules are installed, a bot that is in the
> middle of an Auction House or vendor visit can currently be selected by PvP
> Life and teleported away. This fork exists partly to be the integration point
> for any future changes needed to keep the two modules peaceful.
>
> **As upstream `mod-playerbots-pvp-life`, the `azerothcore-wotlk-playerbots`
> fork, and `mod-playerbots-auctioneer` continue to evolve, this fork will need
> periodic re-syncing. See the "Keeping This Fork in Sync" section below.**

Repository/module name: `mod-playerbots-auctioneer-pvp-life`  
Original author: **iCore**. Incorporates 100% of capabilities from `mod-playerbots-world-pvp`.  
Fork maintainer: [Electrobutterfly](https://github.com/electrobutterfly?tab=repositories).

An AzerothCore + mod-playerbots-auctioneer module focused on making the open world feel alive through persistent, configurable bot PvP, world skirmishes, reactive raid defenses, and capital gate duel rituals.

**Single Installed Module Requirement:** Users install only `mod-playerbots-auctioneer-pvp-life` alongside [AzerothCore](https://github.com/electrobutterfly/azerothcore-wotlk-playerbots) and [mod-playerbots-auctioneer](). `mod-playerbots-world-pvp` is completely unnecessary and should not be installed or enabled.

---

### Key Subsystems & Features

- **Persistent World PvP & Frontlines:** Keeps outdoor PvP skirmishes actively running across 35+ configured hotspots throughout Eastern Kingdoms, Kalimdor, Outland, and Northrend.
- **Reactive Raid Defense:** Automatically detects qualifying outdoor player-led invasion raids (20+ members, PvP-flagged leader), staging an opposing faction defense force 120 yards away in 9 formation lanes at 4-yard spacing before advancing 25 yards to engage.
- **Unified Capital Gate Duel Arenas:** Dynamic, natural bot-vs-bot duel activity outside capital gates (Stormwind, Orgrimmar, Ironforge, Undercity, Dalaran Underbelly). Dueling is unified and centralized exclusively under `mod-playerbots-pvp-life`.
- **Natural Duel Movement & Approach:** Challengers actively move toward opponents taking varied paths with angular flank offsets rather than rigid straight lines. Idle bots remain anchored in the duel grounds after victory/defeat, eliminating restless map-wide running back-and-forth.
- **Proximity-Prioritized Partner Selection:** Duel scheduling prioritizes nearby contenders within the duel arena, eliminating erratic crossing across the duel grounds.
- **Bot-to-Player Duel Challenges:** Bots in duel arenas challenge nearby real players with configurable level-difference checks and strict anti-spam cooldowns.
- **For the Horde & For the Alliance Campaigns:** Faction pressure pushes towards enemy capitals with organic defender yells and world chat reactions.
- **Mutual Anti-Stealing Reservation Barrier:** Standalone-safe, thread-safe shared reservation registry (`PlayerbotsLife::ReservationRegistry`) ensuring bots borrowed by City Life are never stolen or commanded by PvP Life, and vice versa.
- **Conservative Balanced Population Budgets:** Default borrowing budget of 10% / 120 bots max (configured for a 1,000-bot server) guarantees that ≥ 80% (800+ bots) of random bots remain completely free to roam, quest, level, and run dungeons.
- **Zero Bot Tampering Invariant:** Bots are borrowed strictly AS-IS from the natural Playerbot pool with zero artificial level boosts, zero talent changes, and zero stat inflation.
- **Zero GM Spam Protection:** Game Masters and administrators are strictly exempt from unsolicited duel challenges, yells, or world announcements.
- **Silent-by-Default Chatter & Announcements:** All automated bot chatter, yells, and server-wide announcements default to OFF (`0`) in config for a clean, natural player atmosphere.
- **Instant Disengagement on Invite or Logout:** Combatants and duelers immediately drop PvP Life activities if invited to a player group or logged out.
- **Shared Duel Staging Lattice:** 20-yard minimum separation and varied pair staging orientations ensure bots never clip or overlap during dueling.
- **Asynchronous Non-Blocking Audit Logger:** Dedicated worker thread writing to `Logs/pvplife_audit.log` tracking `MOVE`, `DUEL`, `START`, `MEMBER`, `DUEL_QUEUE`, `END`, `RETURN`, `REACTIVE_START`, and `CONFIG` events with zero world-thread blocking I/O.
- **100% Backward-Compatible `.wpvp` GM Command Aliases:** All 14 `.wpvp` commands from the legacy world PvP module work natively as aliases within PvP Life.

---

### Fork-Specific Notes

- **Compilation warnings cleanup.** Two private fields on `PvPLife::Manager` (`_startupDelayLogged` and `_nextAssignmentId`) were declared but never read or written. Both were removed. No runtime behaviour change.
- **No other source modifications** as of the current fork revision. The module's runtime behaviour is identical to upstream.
- **Cross-module interaction with `mod-playerbots-auctioneer`:** The auctioneer subsystem does not participate in the shared `PlayerbotsLife` reservation registry. In practice, on a 1,500-bot server with default settings, the overlap between an AH visit and a PvP Life recruitment window has not been observed to cause a broken AH visit. If the auctioneer is later extended to use the shared registry, this fork is where the corresponding PvP Life-side changes will land.

---

## Requirements

- [`azerothcore-wotlk-playerbots` fork](https://github.com/electrobutterfly/mod-playerbots-auctioneer)
  *(Do **not** clone `mod-playerbots/azerothcore-wotlk` — the Playerbots branch alone is not compatible with this fork.)*
- The playerbot module from the same fork *(installed as `modules/mod-playerbots-auctioneer`)*
- C++17-capable build environment (used by AzerothCore).
- MySQL database (AzerothCore baseline).

---

## Installation & Setup

1. Place `mod-playerbots-pvp-life` into your `modules/` folder:
   ```bash
   azerothcore-wotlk-playerbots/modules/mod-playerbots-auctioneer-pvp-life

1. Apply the schema and seed data to your **WORLD** database:

   ```sql
   mysql -u root -p acore_world < data/sql/manual/world_pvp_life.sql
   ```

   

   *(If upgrading from customized legacy `playerbot_world_pvp_hotspot` data, run `data/sql/manual/import_legacy_world_pvp_hotspots.sql`).*

2. Re-run CMake and rebuild the server. Current AzerothCore automatically discovers `mod-playerbots-auctioneer-pvp-life` and its `.conf.dist`.

3. Copy `conf/mod_playerbots_pvp_life.conf.dist` to `mod_playerbots_pvp_life.conf` and adjust settings as desired.

4. Start worldserver and look for `>> Loaded mod-playerbots-pvp-life` in your startup log.

------

## Coexistence with `mod-playerbots-city-life`

`mod-playerbots-pvp-life` and `mod-playerbots-city-life` are architected for seamless, zero-conflict simultaneous execution:

- **Zero Feature Suppression:** Neither module disables any feature for the other.
- **Concurrent Capital Gate Duels:** Both modules host dueling outside city gates simultaneously. Each module pairs only its own reserved bots.
- **Natural Hostility:** Standard core PvP hostility rules apply everywhere.
- **Candidate Scanning Isolation:** During bot recruitment, each module verifies `!PlayerbotsLife::IsReserved(guid)` before claiming participants.
- **Thread-Safe Shared Reservations:** `src/LifeBotReservation.h` provides atomic module flags and mutex-guarded ownership checks that resolve to the same process-wide singleton whether compiled together or standalone.

------

## Coexistence with `mod-playerbots-auctioneer`

The auctioneer subsystem (auction house and vendor automation) does **not** use the shared `PlayerbotsLife` reservation registry. PvP Life's candidate selection is therefore unaware of bots that are currently mid-AH-visit or mid-vendor-visit.

On the target fork this has not been observed to cause broken AH visits in production. The two subsystems operate on different enough cycles that the overlap window is small, and the AH visit is tolerant of being interrupted (it will simply queue fewer items on that visit than it otherwise would have). If you observe an AH visit that starts, builds its sell queue, and then never lists anything, check whether PvP Life moved the same bot in the intervening period:

```bash
grep '<botname>' logs/pvplife_audit.log
grep '<botname>' logs/Playerbots.log | grep 'AH-TRACE'
```

------

## GM Commands Reference

Both `.pvplife` and `.wpvp` roots are fully supported:

### Status & Diagnostics

- `.pvplife status` (or `.wpvp status`): Overview of active events, queues, budgets, and participant counts.
- `.pvplife scan [minLvl] [maxLvl]` (or `.wpvp scan`): Scans online bot population, faction distribution, and hotspot readiness.
- `.pvplife compat` (or `.wpvp compat`): Shows detected active Life modules and reservation statistics.
- `.pvplife reservations`: Breakdown of reserved bots by owning module (`CityLife`, `PvPLife`, `Artisans`).
- `.pvplife reload`: Reloads configuration, zone cache, and chat tables.

### Reactive Defense Management

- `.pvplife response status`: Shows pending raid responses, targets, and staging countdowns.
- `.pvplife response stop <id|all>`: Cancels pending reactive defenses.

### Hotspot & Event Management

- `.pvplife start <zoneName>` (or `.wpvp start <spot>`): Force-starts a configured hotspot.
- `.pvplife stop <id|all>` (or `.wpvp stop <id|all>`): Stops one or all active events and returns bots.
- `.pvplife zone list` (or `.wpvp spot list`): Lists all configured hotspots.
- `.pvplife zone create <name> <type> <attacker> <defender> <minLvl> <maxLvl>` (or `.wpvp spot create`): Creates a hotspot at current GM position.
- `.pvplife zone rally <name>` (or `.wpvp spot rally`): Saves current GM position as attacker rally point.
- `.pvplife zone target <name>` (or `.wpvp spot target`): Saves current GM position as defender target point.
- `.pvplife zone counts <name> <attMin> <attMax> <defMin> <defMax>` (or `.wpvp spot counts`): Tunes participant counts.
- `.pvplife zone duration <name> <minM> <maxM>` (or `.wpvp spot duration`): Tunes event duration.
- `.pvplife zone enable <name> <0|1>` (or `.wpvp spot enable`): Enables or disables a hotspot.

------

## Credits & Attribution

- **Original module: PvP Life Core & Architecture** — created by **iCore**.
- **Original module: World PvP Hotspot Seeds & Reactive Defense Logic** — ported and consolidated from `mod-playerbots-world-pvp`.
- **Cross-Module Reservation Protocol** — designed for the Playerbots Life ecosystem (`mod-playerbots-city-life`, `mod-playerbots-pvp-life`, `mod-playerbots-artisans`).
- **Fork maintenance, compatibility fixes, and updates to track the `azerothcore-wotlk-playerbots` and `mod-playerbots-auctioneer`fork** — maintained by *[Electrobutterfly](https://github.com/electrobutterfly)*.
- **Companion module `mod-playerbots-auctioneer`** — the auction house and vendor automation subsystem that this fork is designed to coexist with.

All original code, documentation, and design remain the intellectual property of **iCore** and the upstream contributors. This fork redistributes them under the same licence as upstream (`SPDX-License-Identifier: GPL-2.0-or-later` per file headers) with the specific compat changes documented above.
