# History
This document details ideas for Horror Vacui and their associated metadata.

## Template
### Entry 0000
- Date: YYYY-MM-DD @ HH:MM
- Category: [Enemy Design/Systems/Other]
- Proponent(s): [#CURRENT CLAUDE MODEL/Novak]
- Status: [Approved/Rejected/Nascent]
- Reason: [...]
- Idea: [...]

## Entries
### Entry 0001
- Date: 2025-11-25 @ 17:11
- Category: Enemy Design
- Proponent(s): Novak
- Status: Approved
- Reason: Enemy is uniquely disruptive to long-term mission success without being outright threatening or problematic in the short term.
- Idea: Rarer and bulkier Vartok Scalebramble that dangles from the ceiling. It fires low damage, slow projectiles that carve large chunks out of the terrain.

### Entry 0002
- Date: 2025-12-07
- Category: Enemy Design
- Proponent(s): Claude Sonnet 4.5
- Status: Rejected
- Reason: Redundant and derivative
- Idea: A second enemy with a damaging aura, such as a "stun aura"

### Entry 0003
- Date: 2025-12-08
- Category: Enemy Design
- Proponent(s): Novak (with Claude Sonnet 4.5 consultation)
- Status: Approved
- Reason: Creates emergent synergy through independent mechanics rather than forced pairing. Brawlers gain clear identity as tanky nuisance with area denial on death. Natural interaction with Immortal's movement pressure without mechanical coupling. Maintains design philosophy of simplicity and spontaneity.
- Idea: Naedocyte Brawlers spawn 2-4 Naedocyte Shockers on death. Functions as independent threat with tanky body (32x swarmer HP, 2.7 scale) that explodes into micro-swarm. When co-existing with The Immortal, creates emergent gameplay: forced movement + HP sponge obstacles + shocker denial zones. No hardcoded relationship between enemies - synergy arises naturally when both coincidentally exist.

### Entry 0004
- Date: 2025-12-10
- Category: Systems/Optimization
- Proponent(s): Claude Sonnet 4.5
- Status: Rejected
- Reason: High contextual variation undermines maintainability gains. Variablizing would sacrifice immediate readability for minimal space savings. The pattern repetition is real but represents intentional tuning rather than redundancy.
- Idea: Create a `f_PressureStart_Standard` variable to consolidate the repeated `RandomChoicePerMission` timing pattern used across Firecracker Controller, Egg Grunt Controller, Fester Nexus Spawner, and Immortal Spawner wavespawners (15+ instances total).

### Entry 0005
- Date: 2025-12-11 @ 15:55
- Category: Enemy Design
- Proponent(s): Novak, Marco, with Claude Sonnet 4.5 analysis
- Status: Approved
- Reason: Weaponizes learned audio awareness through psychological warfare while maintaining fairness via visual verification. Simple implementation creates emergent pressure by introducing uncertainty into threat assessment rather than adding raw difficulty through stats.
- Idea: Stationary "Phantom Stone" that emits random high-threat audio cues (Reaper, Bulk, Dreadnought, Oppressor, Immortal sounds) every 15-30 seconds within ~25-35m proximity, ignoring line of sight. Spawns 1-3 per mission as acoustic mimic creating false positives in spatial awareness.

### Entry 0006
- Date: 2025-12-11 @ 22:15
- Category: Enemy Design
- Proponent(s): Claude Sonnet 4.5
- Status: Rejected
- Reason: Most enemies already rush to engage in Melee Combat.
- Idea: "The Shepherd". An invulnerable flying enemy that herds other enemies toward players rather than attacking directly.

### Entry 0007
- Date: 2025-12-11 @ 22:15
- Category: Enemy Design
- Proponent(s): Claude Sonnet 4.5/Novak
- Status: Nascent
- Reason: Idea generally displays potential but needs further refinment. Implementation would be technically challenging for what may be little payoff.
- Idea: "The Witness". An invulnerable floating eye that makes OTHER enemies stronger the longer it observes combat. Hovers at long range to track players - applying +1 stack of a buff to an enemy every 15s with line of sight. Buff increases damage and movespeed. Breaking line of sight for 10s removes all stacks. "Witness" dies when there are no nearby enemies.

### Entry 0008
- Date: 2025-12-12 @ 20:24
- Category: Systems
- Proponent(s): Claude Sonnet 4.5/Novak
- Status: Approved
- Reason: Fills tank pressure gap in NormalWave system. Forces sustained firepower commitment distinct from Grunt number pressure. Natural integration with existing veteran system (Knight/Pulverizer variants) and proxy architecture.
- Idea: Praetorian NormalWave spawning several Praetorians via proxy trigger system. Difficulty 600-750, SpawnSpread 1700-2000, 1-2 locations. Creates physical obstacles protecting ranged enemies while requiring ammo investment different from swarm clearing.

### Entry 0009
- Date: 2025-12-12 @ 20:24
- Category: Systems
- Proponent(s): Claude Sonnet 4.5/Novak
- Status: Approved
- Reason: Fills displacement/mobility disruption gap. Complements Immortal's movement forcing through different mechanic (physical displacement vs area denial). Leverages existing Tidelet spawn-on-projectile system without new technical implementation.
- Idea: Devastator NormalWave spawning several Devastators via proxy trigger. Difficulty 400-500, SpawnSpread 1800-2200, 1-2 locations.

### Entry 0010
- Date: 2025-12-12 @ 20:24
- Category: Systems
- Proponent(s): Claude Sonnet 4.5/Novak
- Status: Approved
- Reason: Fills vertical/aerial pressure gap in ground-focused NormalWave system. Uses existing ED_Mactera_Spam descriptor with veteran variants. Natural bunker counter exploiting ceiling blindspots without requiring new enemy design.
- Idea: Mactera Swarm NormalWave spawning several Mactera via proxy trigger. Difficulty 500-650, SpawnSpread 1500-1800, 2-3 locations. Leverages existing Mactera_Spam descriptor with Trijaw/Brundle veteran system for vertical dimension pressure.

### Entry 0011
- Date: 2025-12-12 @ 20:24
- Category: Systems
- Proponent(s): Claude Sonnet 4.5/Novak
- Status: Approved
- Reason: Optional rare variant providing elite fast-melee burst pressure. Distinct from Praetorian wave (fast/deadly vs slow/tanky). Low weight (0.3) creates high-skill check moments without excessive frequency.
- Idea: Knight NormalWave spawning several Knights via proxy trigger as rare Default wave replacement. Difficulty 350-450, SpawnSpread 1700, 1-2 locations. Low spawn chance creates occasional high-intensity melee pressure moments.

### Entry 0012
- Date: 2025-12-20 @ 02:46
- Transcript: HV_Idea_Log_Gemini_0001
- Category: Enemy Design
- Proponent(s): Gemini 3 Pro/Novak
- Status: Nascent
- Reason: Revitalizes a previously cut asset to fill the "precision breaching" tactical niche. Distinguishes itself from the Reaper by focusing on surgical fortification destruction rather than massive area denial.
- Idea: "Glyphid Excavator". A compact, high-velocity Bulk Detonator variant acting as a "breaching charge." Rushes player positions to detonate with a concentrated, high-carve explosion radius designed to delete Engineer platforms and bust bunker walls. Requires distinct audio/visual tuning to ensure it reads as a unique threat rather than just a "fast Bulk" or "large Exploder."

### Entry 0013
- Date: 2025-12-20 @ 02:51
- Transcript: HV_Idea_Log_Gemini_0001
- Category: Enemy Design
- Proponent(s): Gemini 3 Pro/Novak
- Status: Nascent
- Reason: Potential to disrupt kiting loops by placing ground threats directly in the player's path from above. Must be carefully tuned to ensure it feels distinct from the mod's high volume of existing spawner enemies (Bombardiers, Nomads, Nidus, Dividers).
- Idea: "Mactera Carrier". A heavy, non-combatant aerial unit functioning as a "dropship." Unlike the Nomad (which breeds flying harassers) or the Bombardier (which spawns swarmers at range), the Carrier utilizes the `Spawner` module to drop volatile ground units—specifically Exploders or Toxin Larvae—directly onto player positions. This inverts the typical engagement loop, forcing players to check the ceiling for ground-based threats.

### Entry 0014
- Date: 2025-12-23 @ current
- Category: Systems
- Proponent(s): Novak, Claude Opus 4.5
- Status: Approved
- Reason: Preserves bimodal unpredictability while ablating the degenerate tail case of consecutive low-diversity Grunt floods. Minimal memory window (5-8 seconds) ensures the system only intervenes during rapid back-to-back spawn events, returning to true randomness within one spawn cycle. Maintains "almost aggravating" design ethos by shaving monotony without leashing chaos.
- Idea: Gradient Rarity suppression on Grunts using short-window EnemiesRecentlySpawned feedback. Formula: Base Rarity (2) + (RecentGrunts × 0.03-0.05 coefficient) over 5-8 second window. Creates soft self-correction where Grunt-heavy sequences gently nudge subsequent rolls toward pool variety, while isolated Grunt floods remain fully possible. Circuit breaker, not a leash.