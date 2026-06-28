# Context Map

## Movement

### Contexts

- [Movement Behaviour](./src/server/game/Movement/CONTEXT.md) — selects and drives a movement strategy for a `Unit` each tick via a priority stack of generators
- [Pathfinding](./src/server/game/Movement/Spline/CONTEXT.md) — resolves a destination into a walkable point array and executes it as a client-visible spline
- [Scripted Paths](./src/server/game/Movement/Waypoints/CONTEXT.md) — stores designer-authored patrol paths as named sequences of world positions loaded from the database

### Relationships

- **AI Scripting → Movement Behaviour**: commands movement via `MotionMaster` (e.g. `MoveChase`, `MovePoint`, `MoveFollow`, `MoveWaypoint`)
- **Movement Behaviour → AI Scripting**: notifies on completion via `MovementInform(type, id)` so AI can react to arrival
- **Movement Behaviour → Scripted Paths**: pulls a path by numeric ID at generator initialisation (`sWaypointMgr->GetPath(id)`)
- **AI Scripting → Scripted Paths**: SmartAI publishes its own waypoint paths as a second source of the same concept (`PathSource::SMART_WAYPOINT_MGR`)
- **Movement Behaviour → Pathfinding**: constructs a `PathGenerator` (the seam) and calls `CalculatePath()` when a walkable route is needed
- **Pathfinding → Map**: `PathGenerator` reads navmesh data from the owning `Map` at construction time (`GetMapCollisionData().GetMMapData()`)
