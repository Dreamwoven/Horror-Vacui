# Analyzing Repeated Functions for Global Variables

## Scope Analysis
I need to identify patterns in the Horror Vacui B27.json where:
- Similar mutator logic chains appear multiple times
- Values are recalculated in multiple places
- The same conceptual "state" is checked repeatedly

Let me examine the file systematically.

## Deep Reasoning

### 1. ENUMERATE Pattern Categories

**A. Mission State Checks**
- `DuringGenericSwarm` with specific timing windows
- `DuringMission` with StartingAt/StoppingAfter
- Mission type detection via `ByDNA`

**B. Enemy Counting**
- `EnemyCount` for specific descriptors
- `EnemiesRecentlySpawned` with time windows
- Total enemy count thresholds

**C. Player State**
- `DwarvesHealth` thresholds
- `ByPlayerCount` scaling
- Dwarf down states

**D. Time-based Accumulation**
- `Accumulate` patterns with mission time gates
- Progressive difficulty scaling

**E. Cooldown Management**
- `EnemyCooldown` patterns
- `TriggerCooldown` with specific durations

### 2. EXPLORE Each Pattern

**Pattern A: Swarm State Detection**
```
"Mutate": "DuringGenericSwarm",
"StartingAt": 5,
"StoppingAfter": 110
```
This appears **8+ times** across various wavespawners. Creates boolean: "is active swarm happening right now?"

**Pattern B: Enemy Count Thresholds**
```
"Mutate": "IfFloat",
"Value": {"Mutate": "EnemyCount"},
"<" or ">": [various thresholds like 30, 36, 70, 80]
```
Appears in multiple rarities and spawn conditions. Could be: `IsEnemyCapReached`, `IsLowEnemyCount`

**Pattern C: Recent Bulk Spawns**
```
"Mutate": "EnemiesRecentlySpawned",
"Seconds": 60,
"EDs": ["ED_Spider_ExploderTank", "ED_Bulk_Base", "VED_Reaper"]
```
Used for rarity limiting. This is conceptually "BulkSpawnPressure"

**Pattern D: Immortal Count Check**
```
"Mutate": "Clamp",
"Value": {"Mutate": "EnemyCount", "ED": "XED_Immortal"},
"Min": 0,
"Max": 2
```
Failsafe logic checking if Immortal exists

**Pattern E: Mission Progression Gates**
The accumulate patterns with `DuringMission` StartingAt checks for different mission types - these represent "mission intensity over time"

**Pattern F: Defense Event Detection**
```
"Mutate": "BySalvagePhase"
"Mutate": "ByEscortPhase"  
"Mutate": "DuringDefend"
```
These check for high-intensity defense scenarios

### 3. EVALUATE Viability

**High Value Candidates:**
1. **`IsActiveSwarm`** - Boolean for the common 5-110s swarm window
2. **`IsEnemyCapHigh`** - Boolean for enemy count > 70-80
3. **`IsEnemyCapLow`** - Boolean for enemy count < 30-36
4. **`BulkSpawnPressure`** - Float representing recent bulk-type spawns
5. **`ImmortalExists`** - Boolean for Immortal presence
6. **`IsDefenseEvent`** - Boolean for any defense scenario
7. **`MissionIntensity`** - Float from 0-2 representing time-based difficulty ramp
8. **`TeamHealthCritical`** - Boolean for DwarvesHealth < 0.3-0.4

**Medium Value Candidates:**
9. **`ArchitectCooldownActive`** - Boolean for Architect spawn limiter
10. **`ProxyTrigger`** - The normalwave proxy detection logic
11. **`IsMissionLate`** - Boolean for mission time > 400-500s

### 4. PROCEED with Detailed Analysis

**Priority 1: `IsActiveSwarm`**
Currently appears in:
- Immortal spawn/behavior switching (Scale, Materials, Movement)
- Firecracker controller negation
- Multiple pool modifications
- EnemyCountModifier adjustments
- Brawler spawning

**Implementation:**
```
"IsActiveSwarm": {
  "Mutate": "DuringGenericSwarm",
  "StartingAt": 5,
  "StoppingAfter": 110
}
```

**Impact:** Would replace 12+ instances, simplifying Immortal definition significantly.

---

**Priority 2: Enemy Count State Variables**

Currently enemy count checks appear in:
- Rarity adjustments (Stalker, Stingtail, Exploder_Base, Spider_RapidShooter)
- Immortal orb spawning
- EnemyCountModifier logic
- MacGrabber rarity

**Implementation:**
```
"EnemyCountHigh": {
  "Mutate": "IfFloat",
  "Value": {"Mutate": "EnemyCount"},
  ">": 70,
  "Then": true,
  "Else": false
}

"EnemyCountLow": {
  "Mutate": "IfFloat", 
  "Value": {"Mutate": "EnemyCount"},
  "<": 35,
  "Then": true,
  "Else": false
}
```

**Impact:** Would replace 8+ instances, make rarity logic more readable.

---

**Priority 3: `BulkTypeSpawnPressure`**

Currently the exact same logic appears twice:
- ED_Spider_ExploderTank rarity
- ED_Bulk_Base inherits this

**Implementation:**
```
"BulkTypeSpawnPressure": {
  "Mutate": "Multiply",
  "Value": 15,
  "Key": {
    "Mutate": "EnemiesRecentlySpawned",
    "Seconds": 60,
    "EDs": ["ED_Spider_ExploderTank", "ED_Bulk_Base", "VED_Reaper"]
  }
}
```

**Impact:** Centralizes the "how recently did we spawn dangerous enemies" logic, making it easier to tune the 60s window or the multiplier.

---

**Priority 4: `DefenseEventActive`**

Appears in:
- EnemyCountModifier (Salvage phases, Escort phases)
- Multiple wavespawner Enable conditions
- Immortal behavior

**Implementation:**
```
"DefenseEventActive": {
  "Mutate": "OR",
  "A": {"Mutate": "DuringDefend"},
  "B": {
    "Mutate": "BySalvagePhase",
    "Default": false,
    "Uplink": true,
    "Refuel": true
  }
}
```

**Impact:** Simplifies defense detection, though mission-specific scaling might need to remain local.

---

**Priority 5: `MissionLateGame`**

The concept of "mission has been running long enough for intensity ramp" appears in:
- Firecracker controller
- Egg Grunt controller  
- Fester Nexus spawner
- Various accumulate patterns

**Implementation:**
```
"MissionLateGame": {
  "Mutate": "DuringMission",
  "StartingAt": 400
}
```

**Impact:** Provides semantic clarity - "is this late in the mission?" vs raw time checks.

## Identifying Limitations

**Confidence: 85%**

**Assumptions made:**
1. Global variables can be referenced in mutator chains without circular dependency issues
2. The CD2 implementation will allow globals to be computed once per tick, not multiple times
3. Mission-specific variations (ByDNA) may still need to be local in some cases

**What would change this analysis:**
- If globals cannot contain mutator logic (only static values)
- If there's significant performance cost to global variable lookup
- If the new CD2 version allows globals to be parameterized (e.g., `IsSwarmActive(startTime, stopTime)`)

**Additional considerations:**
- Some patterns like `RandomChoicePerMission` create mission-specific variation - these probably shouldn't be global
- The Proxy trigger system for NormalWaves is complex enough that it might benefit from globals but needs careful handling
- Time-based accumulation patterns are mission-type specific, so full globalization may not make sense

## Conclusion

**Top 5 Repeated Functions for Global Variables:**

1. **`IsActiveSwarm`** - The "5-110s swarm window" boolean (~12 uses)
2. **`EnemyCountHigh` / `EnemyCountLow`** - Boolean thresholds for enemy density (~8 uses)
3. **`BulkTypeSpawnPressure`** - Recent spawn tracker for bulk-class enemies (2 identical uses, but conceptually appears more)
4. **`ImmortalIsAlive`** - Clamped count check for failsafe logic (~3 uses)
5. **`MissionLateGame`** - Boolean for mission time > ~400s (~6 uses with variation)

**Secondary candidates:**
- `TeamHealthCritical` - DwarvesHealth < 0.3-0.4 threshold
- `DefenseEventActive` - Unified defense phase detection
- `IsEggMission`, `IsPEMission`, etc. - Mission type booleans

The most impactful would be **`IsActiveSwarm`** as it appears most frequently and drives major behavior changes (Immortal scaling, spawn logic, pool modifications). Consolidating this would make the Immortal enemy definition alone 30-40% shorter and more maintainable.