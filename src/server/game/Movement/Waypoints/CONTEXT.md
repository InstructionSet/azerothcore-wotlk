# Scripted Paths

Stores designer-authored patrol routes as named sequences of world positions loaded from the database at startup.

## Language

**Waypoint Path**:
A named, ordered collection of Waypoint Nodes that defines a patrol route. Identified by a numeric ID and loaded by `WaypointMgr` at startup.
_Avoid_: patrol path, movement path

**Waypoint Node**:
A single point in a Waypoint Path, carrying a world position, optional facing orientation, move type, delay, and an optional event reference.
_Avoid_: waypoint, path node

**Waypoint Move Type**:
The locomotion mode used when travelling to a Waypoint Node: walk, run, land, or takeoff.
_Avoid_: move type, locomotion

**Waypoint Event**:
A script event fired on arrival at a Waypoint Node, identified by an EventId and gated by an EventChance percentage.
_Avoid_: node event, arrival event
