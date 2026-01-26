# StrangleThorn Vale Architecture Documentation

**Timeline**: January 2026 - Ongoing  
**Current Phase**: Data Layer Analysis  
**Zone ID**: N/A - See architectural constraint below  
**Map**: 0 (Eastern Kingdoms)

## Status

| Phase | Status | Completion Date | Notes |
|-------|--------|-----------------|-------|
| Data Layer Analysis | 🚧 In Progress | - | Zone identification complete (01/26/26) |
| Spatial Layer Mapping | ⏳ Planned | - | After data layer |
| Behavioral System Inventory | ⏳ Planned | - | After spatial layer |
| Performance Baseline | ⏳ Planned | - | Final phase |

## Overview
StrangleThorn Vale is a contested territory zone in the Eastern Kingdoms, spanning levels 30-45 with diverse content including Zul'Gurub raid, Booty Bay neutral city, and extensive PvP areas. This zone was selected as the first case study for game world architecture documentation because it contains representative systems found throughout the game world while remaining manageable in scope.

The zone serves as the foundation for the Port Gurubashi custom city project, making thorough understanding of its architecture essential. By documenting STV's patterns, I'll establish reusable methodologies for analyzing other zones and designing custom content that integrates cleanly with existing systems.

This documentation will use arc42 methodology adapted for game architecture, progressing through three analysis layers:
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

### Zone Identification ✅ (Completed: January 27, 2026)

Successfully identified StrangleThorn Vale boundaries through coordinate-based analysis after discovering that zone ID fields are unpopulated in this AzerothCore installation.

**Coordinate Boundaries Established:**
- X Range: -14596.7 to -10301
- Y Range: -4196.7 to 1599.5
- Z Range: -99.29 to 122.43
- Total Area: ~4,300 × 5,800 units

**Entity Count:**
- 4,160 creature spawns identified

**Key Architectural Finding:**
Discovered that `creature.zoneId` column is unpopulated (all values = 0 for 29,448 creatures on Eastern Kingdoms). This means zone identification must be coordinate-based rather than ID-based, affecting all spatial queries and requiring coordinate range constants.

**Verification Method:**
Confirmed zone identity through creature name sampling - identified characteristic STV NPCs including Booty Bay pirates, Nesingwary expedition members, and coastal wildlife.

**Technical Artifacts:**
- [Zone Identification Queries](https://github.com/KyleDGorilla/vanilla-gorilla/blob/main/docs/architecture/zones/stranglethorn-vale/queries/01-zone-identification.sql)
- [Full Technical Documentation](https://github.com/KyleDGorilla/vanilla-gorilla/blob/main/docs/architecture/zones/stranglethorn-vale/architecture.md)

**Impact on Project:**
This constraint applies to Port Gurubashi design - custom content must also be identified by coordinates. Will need to maintain coordinate constants for all custom areas.

## Work In Progress

### Creature Distribution Analysis (Current Focus)

Currently analyzing creature spawn patterns, distribution, and relationships to creature templates.

**Planned Activities:**
- Analyze creature distribution by type and level
- Document creature_template relationships
- Identify spawn group patterns
- Count creatures by category

**Next**: Gameobject analysis, then quest chains

## Work Plan

### Phase 2: Spatial Layer Mapping (Upcoming)
**Objective**: Document terrain features, navigation meshes, and zone boundaries.

**Planned Activities:**
- Use NoggIt editor to explore zone geography
- Map zone boundaries and coordinate ranges
- Document sub-zones: Booty Bay, Rebel Camp, Nesingwary's Camp, Gurubashi Arena, Zul'Gurub exterior
- Analyze navigation mesh characteristics in different terrain types
- Identify terrain constraints (cliffs, water, impassable areas)
- Document transportation: Booty Bay docks, flight paths
- Map potential locations for Port Gurubashi custom city
- Capture reference screenshots from multiple angles

**Expected Deliverables:**
- Screenshot collection of key areas and landmarks
- Sub-zone mapping with boundaries
- Navigation mesh analysis for different terrain types
- Spatial section in technical architecture.md
- Site analysis for Port Gurubashi location

### Phase 3: Behavioral Layer Analysis (Future)
**Objective**: Inventory AI scripts, event systems, patrol routes, and game mechanics.

**Planned Activities:**
- Catalog smart_scripts for STV NPCs (pirates, beasts, trolls)
- Document Gurubashi Arena event mechanics and timing
- Map NPC patrol routes (especially in Booty Bay and camps)
- Analyze faction mechanics (contested territory, guard behaviors)
- Document quest trigger mechanisms and scripted events
- Identify spawn timing patterns and respawn rules
- Analyze rare spawn mechanics (Bloodsail Admiral, etc.)

**Expected Deliverables:**
- Smart script inventory organized by NPC type
- Gurubashi Arena event documentation (for reference in Port Gurubashi design)
- Patrol route maps showing NPC movement patterns
- Behavioral patterns documentation
- Behavioral section in technical architecture.md

### Phase 4: Performance Baseline (Future)
**Objective**: Establish performance metrics for the zone under various load conditions.

**Planned Activities:**
- Measure server FPS with STV empty
- Measure server FPS with typical player load (10, 25, 50 players)
- Analyze memory consumption specific to STV entities
- Identify query performance bottlenecks for zone-related operations
- Document spawn overhead and initial load times
- Test with playerbots to simulate realistic population
- Measure impact of Gurubashi Arena event on server performance

**Expected Deliverables:**
- Performance metrics documentation with baseline values
- Identified bottlenecks and optimization opportunities
- Load testing results at different player counts
- Performance section in technical architecture.md
- Performance budget for Port Gurubashi additions

## Why StrangleThorn Vale?
This zone was selected as the first documentation target because:

1. **Architectural Complexity**: STV contains diverse systems (instanced raid content, neutral city, contested territory, event systems) making it representative of game-wide architectural patterns

2. **Foundation for Port Gurubashi**: Understanding STV's architecture is essential for designing Port Gurubashi custom city, particularly the Gurubashi Arena conversion and contested territory mechanics

3. **Manageable Scope**: Large enough to be meaningful, small enough to document thoroughly as a first zone analysis project

4. **Well-Defined Boundaries**: Clear geographic boundaries and distinct sub-areas make it easier to scope analysis work

5. **Interesting Event Systems**: The Gurubashi Arena timed event provides a model for Port Gurubashi's PvP riot mechanics

## Skills Demonstrated (To Date)

### Technical Skills
- Database analysis and SQL query writing
- Coordinate-based spatial queries
- Legacy system reverse engineering without source code
- Problem-solving when expected data sources are unavailable
- Adapting analysis approach based on system constraints

### Architectural Skills  
- arc42 documentation framework application to game systems
- Identifying and documenting architectural constraints
- Alternative approach development when primary methods fail
- System analysis methodology development

### Tools & Technologies
- MySQL/MariaDB database analysis
- HeidiSQL for database exploration
- Git for version-controlled documentation workflows

### Process Skills
- Systematic approach to understanding complex legacy systems
- Iterative discovery and adaptation
- Technical writing documenting both successes and constraints
- Documentation-as-code practices

## Architectural Insights

### Constraint: Unpopulated Zone IDs
**Discovery**: The `creature.zoneId` column contains value 0 for all 29,448 creatures on Eastern Kingdoms map. Similarly, DBC tables like `areatable_dbc` exist but are unpopulated.

**Impact**: 
- Zone identification must be coordinate-based
- All spatial queries require coordinate range WHERE clauses
- Cannot use zone ID for filtering or grouping
- Requires maintaining coordinate constants for each zone

**Implication for Port Gurubashi**:
Custom content must also be managed via coordinates. Consider creating a constants file or database view with zone boundaries for easier querying.

## Related Work
- [MMORPG AWS Infrastructure Project](../../mmorpg-aws/) - System-level architecture and hybrid cloud design
- [ADR-001: Hybrid Cloud Architecture](../../mmorpg-aws/adrs/ADR-001-hybrid-cloud-architecture.md)
- [ADR-003: Windows Mini PC Worldserver](../../mmorpg-aws/adrs/ADR-003-windows-mini-pc-worldserver.md)
- Vanilla Gorilla Repository - [github.com/KyleDGorilla/vanilla-gorilla](https://github.com/KyleDGorilla/vanilla-gorilla)

## Next Steps
1. ✅ ~~Set up documentation structure in vanilla-gorilla repository~~
2. ✅ ~~Identify zone boundaries through coordinate analysis~~
3. 🚧 Analyze creature distribution patterns and spawn groups (current)
4. ⏳ Document gameobject spawns and types
5. ⏳ Analyze quest chains and dependencies
6. ⏳ Create entity-relationship diagrams
7. ⏳ Begin spatial layer analysis with NoggIt

---

*Created: January 25, 2026*  
*Last Updated: January 26, 2026*

