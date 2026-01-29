# StrangleThorn Vale Architecture Documentation

**Timeline**: January 2026 - Ongoing  
**Current Phase**: Data Layer Analysis  
**Zone ID**: N/A - Coordinate-based identification required  
**Map**: 0 (Eastern Kingdoms)

## Status

| Phase | Status | Completion Date | Notes |
|-------|--------|-----------------|-------|
| Data Layer Analysis | ✅ Complete | January 27, 2026 | All core entities documented |
| Spatial Layer Mapping | ⏳ Planned | - | After data layer |
| Behavioral System Inventory | ⏳ Planned | - | After spatial layer |
| Performance Baseline | ⏳ Planned | - | Final phase |

## Overview

StrangleThorn Vale is a contested territory zone in the Eastern Kingdoms, spanning levels 30-45 with diverse content including Zul'Gurub raid, Booty Bay neutral city, and extensive PvP areas. This zone was selected as the first case study for game world architecture documentation because it contains representative systems found throughout the game world while remaining manageable in scope.

The zone serves as the foundation for the Port Gurubashi custom city project, making thorough understanding of its architecture essential. By documenting STV's patterns, I'll establish reusable methodologies for analyzing other zones and designing custom content that integrates cleanly with existing systems.

This documentation uses arc42 methodology adapted for game architecture, progressing through three analysis layers:
1. **Data Layer** - Database schema and entity relationships
2. **Spatial Layer** - Terrain, navigation, and boundaries  
3. **Behavioral Layer** - AI scripts, events, and game systems

## Technical Documentation Location

All technical artifacts live in the system repository:
- **[Full Architecture Documentation](https://github.com/KyleDGorilla/vanilla-gorilla/tree/main/docs/architecture/zones/stranglethorn-vale/architecture.md)**
- **[SQL Analysis Queries](https://github.com/KyleDGorilla/vanilla-gorilla/tree/main/docs/architecture/zones/stranglethorn-vale/queries)**
- **[Architecture Diagrams](https://github.com/KyleDGorilla/vanilla-gorilla/tree/main/docs/architecture/zones/stranglethorn-vale/diagrams)** *(coming soon)*
- **[Screenshots](https://github.com/KyleDGorilla/vanilla-gorilla/tree/main/docs/architecture/zones/stranglethorn-vale/screenshots)** *(coming soon)*

## Work Completed

### Zone Boundary Discovery ✅ (Completed: January 26, 2026)

Successfully identified true StrangleThorn Vale boundaries through landmark-based discovery after initial coordinate assumptions proved incorrect.

**Initial Challenge**: Approximate coordinates (-14600 to -10300 X, -4200 to 1600 Y) captured 4,160 creatures from multiple zones including Blasted Lands and Westfall.

**Discovery Method**:
- Located known STV landmarks: Booty Bay Bruiser, Stranglethorn Tigress, Jungle Stalker
- Analyzed coordinate distributions of confirmed STV creatures
- Built boundaries from actual data rather than assumptions

**Verified Boundaries**:
- X Range: -14500 to -11500 (~3,000 units wide)
- Y Range: -1100 to 1300 (~2,400 units tall)
- Result: 1,572 creatures (all verified as pure STV content)

**Key Architectural Finding**: AzerothCore `creature.zoneId` column is unpopulated (all values = 0). Zone identification must be coordinate-based rather than ID-based, affecting all spatial queries and requiring coordinate range constants for all zone work.

**Lesson Learned**: Ground truth via landmarks > coordinate assumptions. When initial data doesn't match expectations, validate with known entities before proceeding.

**Technical Artifacts**:
- [Zone Identification Queries](https://github.com/KyleDGorilla/vanilla-gorilla/blob/main/docs/architecture/zones/stranglethorn-vale/queries/01-zone-idenfication.sql)
- [Boundary Refinement Process](https://github.com/KyleDGorilla/vanilla-gorilla/blob/main/docs/architecture/zones/stranglethorn-vale/queries/03-boundary-refinement.sql)

---

### Creature Distribution Analysis ✅ (Completed: January 26, 2026)

Analyzed 1,572 creature spawns revealing diverse population strategy and elite distribution patterns.

**Population Characteristics**:
- Top spawner: Drunken Bruiser (62 spawns) - Booty Bay event NPC
- Typical spawns: 18-54 per creature type
- No single type dominates (unlike starter zones with 300+ identical mobs)
- Balanced distribution indicates multiple themed sub-zones

**Creature Families Identified**:
- **Troll Tribes**: Bloodscalp (6 types), Skullsplitter (2 types)
- **Jungle Wildlife**: Tigers, Panthers, Raptors, Gorillas, Basilisks, Crocolisks
- **Humanoid Factions**: Venture Co. goblins, Kurzen humans, Naga
- **City NPCs**: Booty Bay guards (level 77), citizens, vendors

**Elite Distribution Pattern**:
- Rank 0 (Normal): 18-62 spawns per type, distributed throughout
- Rank 1 (Elite): 1-42 spawns per type, specific locations
- Rank 2 (Rare Elite): 4-9 spawns, special encounters
- Rank 3 (World Boss): 1 spawn (roaming)

**Architectural Principle Discovered**: Rarity inversely correlates with spawn density. This pattern applies across all mob tiers.

**Implications for Port Gurubashi**:
- Elite guard NPCs should use rank 1 with 5-10 spawns (not high density)
- Level 77 guards effective for neutral territory PvP enforcement
- Custom content coordinates must avoid existing dense spawn areas

**Technical Artifacts**:
- [Creature Analysis Queries](https://github.com/KyleDGorilla/vanilla-gorilla/blob/main/docs/architecture/zones/stranglethorn-vale/queries/02-creature-analysis.sql)

---

### Gameobject Analysis ✅ (Completed: January 27, 2026)

Analyzed 1,979 gameobjects revealing resource-rich zone design with nearly 1:1 object-to-creature ratio (highly unusual).

**Resource Node Distribution** (994 total - 50% of all gameobjects):

**Mining** (434 spawns):
- Gold Vein: 116 (top resource spawn)
- Silver Vein: 94
- Iron Deposit: 86
- Focus: Mid-to-high tier ores (level 30-45 appropriate)

**Herbalism** (560 spawns):
- Goldthorn: 93
- Kingsblood: 78
- Khadgar's Whisker: 77
- Stranglekelp: 67 (underwater)
- Wide tier range supporting diverse gathering skill levels

**Special Resources**:
- Giant Clam: 80 (underwater gathering)
- Fishing Pools: 126 across 5 types (coastal gameplay)

**Tiered Distribution Pattern**: Spawn count inversely correlates with resource tier
- Low-tier: Scarce (8 tin veins = 1.8%)
- Mid-tier: Abundant (296 gold/silver/iron = 68%)
- High-tier: Present but rare (60 mithril/truesilver = 14%)

**Multi-Environment Design**:
- Land: Standard ore and herb nodes
- Underwater: 147 nodes (stranglekelp, giant clam)
- Coastal: 126 fishing pools
- Demonstrates zone supports diverse gameplay styles

**Event Overlay System**: 132 seasonal objects (6.7% of total)
- Brewfest, Midsummer Fire Festival, Halloween decorations
- Events augment rather than replace permanent content
- Architectural pattern: Layered content system

**Physical Quest Objects**: Books, containers, landmarks, trophies
- Encourages 3D spatial exploration over dialogue-only quests
- Examples: "Fall of Gurubashi" (book), The Holy Spring (landmark), Kurzen Supplies (container)

**Implications for Port Gurubashi**:
- Consider unique custom resource spawns for player engagement
- Neutral territory gathering rules possible (PvP-safe nodes during events?)
- Physical quest objects for custom quest chains
- Resource tier should match zone level (30-45 items)

**Technical Artifacts**:
- [Gameobject Analysis Queries](https://github.com/KyleDGorilla/vanilla-gorilla/blob/main/docs/architecture/zones/stranglethorn-vale/queries/04-gameobject-analysis.sql)

---

### Quest Analysis ✅ (Completed: January 27, 2026)

Analyzed 210 quests revealing dual-purpose design (leveling + raid hub) with unusually high elite difficulty (61%).

**Quest Distribution**:
- Leveling (28-45): 140 quests (67%)
- Raid (58-60): 57 quests (27%)
- Events: 13 quests (6%)

**Critical Finding**: 61% of quests are Elite/Dungeon difficulty - significantly higher than typical zones (30%). This design encourages group play and PvP encounters.

**Iconic Content**:
- Nesingwary Hunting Expedition (Tiger, Panther, Raptor mastery series)
- Zul'Gurub raid quests (57 quests for class-specific gear)
- Faction-specific storylines (35 Alliance, 35 Horde quests)

**Quest Chain System**: Uses `RewardNextQuest` for sequential progression, creating interconnected story arcs throughout zone.

**Technical Details**: [View queries and complete analysis](https://github.com/KyleDGorilla/vanilla-gorilla/blob/main/docs/architecture/zones/stranglethorn-vale/queries/05-quest-analysis.sql)

---

## Work Plan

### Phase 2: Spatial Layer Mapping (Upcoming)

**Objective**: Document terrain features, navigation meshes, and zone boundaries using NoggIt editor.

**Planned Activities**:
- Map sub-zones: Booty Bay, Rebel Camp, Nesingwary's Camp, Gurubashi Arena, Zul'Gurub exterior
- Analyze navigation mesh characteristics in different terrain types
- Identify terrain constraints (cliffs, water, impassable areas)
- Document transportation: Booty Bay docks, flight paths
- Map potential locations for Port Gurubashi custom city
- Capture reference screenshots from multiple angles

**Expected Deliverables**:
- Screenshot collection of key areas and landmarks
- Sub-zone mapping with coordinate boundaries
- Navigation mesh analysis
- Spatial section in technical architecture.md
- Site analysis for Port Gurubashi location

### Phase 3: Behavioral Layer Analysis (Future)

**Objective**: Inventory AI scripts, event systems, patrol routes, and game mechanics.

**Planned Activities**:
- Catalog smart_scripts for STV NPCs (pirates, beasts, trolls)
- Document Gurubashi Arena event mechanics and timing
- Map NPC patrol routes (especially in Booty Bay and camps)
- Analyze faction mechanics (contested territory, guard behaviors)
- Document quest trigger mechanisms and scripted events
- Identify spawn timing patterns and respawn rules

**Expected Deliverables**:
- Smart script inventory organized by NPC type
- Gurubashi Arena event documentation (reference for Port Gurubashi design)
- Patrol route maps showing NPC movement patterns
- Behavioral patterns documentation
- Behavioral section in technical architecture.md

### Phase 4: Performance Baseline (Future)

**Objective**: Establish performance metrics for the zone under various load conditions.

**Planned Activities**:
- Measure server FPS with STV empty vs. populated
- Analyze memory consumption specific to STV entities
- Identify query performance bottlenecks
- Document spawn overhead and load times
- Test with playerbots to simulate realistic population

**Expected Deliverables**:
- Performance metrics documentation with baseline values
- Identified bottlenecks and optimization opportunities
- Load testing results at different player counts
- Performance budget for Port Gurubashi additions

---

## Why StrangleThorn Vale?

This zone was selected as the first documentation target because:

1. **Architectural Complexity**: Diverse systems (instanced raid, neutral city, contested territory, event systems) representative of game-wide patterns

2. **Foundation for Port Gurubashi**: Understanding STV's architecture is essential for designing Port Gurubashi custom city, particularly arena conversion and contested territory mechanics

3. **Manageable Scope**: Large enough to be meaningful, small enough to document thoroughly as initial zone analysis

4. **Well-Defined Boundaries**: Clear geographic boundaries and distinct sub-areas (after refinement)

5. **Event System Integration**: Gurubashi Arena timed event provides model for Port Gurubashi's PvP riot mechanics

---

## Skills Demonstrated (To Date)

### Technical Skills
- Database reverse engineering and SQL query optimization
- Coordinate-based spatial queries and analysis
- Legacy system analysis without source code access
- Problem-solving when expected data sources unavailable
- Iterative refinement based on data validation

### Architectural Skills  
- arc42 documentation framework application to game systems
- Identifying and documenting architectural constraints
- Alternative approach development when primary methods fail
- Ground-truth validation methodology
- Pattern recognition across entity types

### Tools & Technologies
- MySQL/MariaDB database analysis and complex queries
- HeidiSQL for database exploration
- Git for version-controlled documentation workflows
- Markdown for technical documentation

### Process Skills
- Systematic approach to understanding complex legacy systems
- Iterative discovery and adaptation when assumptions fail
- Technical writing documenting both successes and constraints
- Documentation-as-code practices
- Pivot strategy when initial approaches prove incorrect

---

## Architectural Insights

### Constraint: Unpopulated Zone IDs

**Discovery**: `creature.zoneId` column contains value 0 for all Eastern Kingdoms creatures. DBC tables like `areatable_dbc` exist but are completely unpopulated.

**Impact**: 
- Zone identification must be coordinate-based
- All spatial queries require coordinate range WHERE clauses
- Cannot use zone IDs for filtering or grouping operations
- Requires maintaining coordinate constants for each zone
- Complicates cross-zone analysis

**Implication for Port Gurubashi**: Custom content must be managed via coordinates. Consider creating constants file or database view with zone boundaries for easier querying.

### Pattern: Inverse Correlation (Rarity vs. Density)

**Observed across multiple entity types**:
- **Creatures**: Normal mobs (18-62 spawns) > Elites (1-42) > Rare Elites (4-9) > Bosses (1)
- **Resources**: Low-tier (8 tin) < Mid-tier (296 gold/silver/iron) > High-tier (60 mithril/truesilver)
- **Quest Objects**: Common containers (multiple) > Unique landmarks (single spawn)

**Architectural Principle**: Spawn density inversely correlates with rarity/tier/difficulty across all game systems.

### Pattern: Resource-Rich Zone Design

**STV prioritizes gathering over pure combat**:
- Nearly 1:1 ratio of objects to creatures (1,979 vs 1,572)
- 50% of gameobjects are resource nodes
- Multiple profession paths (mining, herbalism, fishing)
- Multi-environment support (land, underwater, coastal)

**Design Philosophy**: Mid-level zones balance combat with economic gameplay to support player progression in multiple dimensions.

---

## Related Work

- [MMORPG AWS Infrastructure Project](../../mmorpg-aws/) - System-level architecture and hybrid cloud design
- [ADR-001: Hybrid Cloud Architecture](../../mmorpg-aws/adrs/ADR-001-hybrid-cloud-architecture.md)
- [ADR-003: Windows Mini PC Worldserver](../../mmorpg-aws/adrs/ADR-003-windows-mini-pc-worldserver.md)
- Vanilla Gorilla Repository - [github.com/KyleDGorilla/vanilla-gorilla](https://github.com/KyleDGorilla/vanilla-gorilla)

---

## Next Steps

1. ✅ ~~Set up documentation structure~~
2. ✅ ~~Identify zone boundaries through coordinate analysis~~
3. ✅ ~~Analyze creature distribution patterns~~
4. ✅ ~~Document gameobject distribution and resource nodes~~
5. ✅ Analyze quest chains and dependencies (current)
6. ⏳ Create entity-relationship diagrams
7. ⏳ Begin spatial layer analysis with NoggIt

---

*Created: January 26, 2026*  
*Last Updated: January 27, 2026*

