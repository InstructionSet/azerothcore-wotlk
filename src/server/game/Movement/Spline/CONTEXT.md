# Pathfinding

Resolves a destination into a sequence of walkable world positions and executes that sequence as a client-visible movement spline.

## Language

**Path**:
A sequence of world positions computed from a source position to a destination by querying the navmesh. May be complete, partial, or a straight line when navmesh is unavailable.
_Avoid_: route, movement path

**Path Generator**:
The boundary object between Movement Behaviour and Pathfinding. Owns a navmesh query handle and resolves a requested destination into a Path.
_Avoid_: PathGenerator, pathfinder

**Spline**:
The interpolated movement curve built from a Path and executed server-side. Broadcast to clients as `SMSG_MONSTER_MOVE` so they animate the unit's motion.
_Avoid_: MoveSpline, movement spline

**Navmesh**:
The precomputed walkable geometry for a map, loaded from MMAP files at startup and owned by the Map. Queried at runtime by the Path Generator.
_Avoid_: MMAP, movement map, collision mesh
