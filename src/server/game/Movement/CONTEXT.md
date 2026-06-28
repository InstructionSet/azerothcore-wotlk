# Movement Behaviour

Selects and drives a movement strategy for a Unit each tick. Generators express *why* a unit moves — not how a walkable route is physically calculated.

## Language

**Motion Master**:
The three-slot priority controller that owns the active generator stack and switches generators as AI commands arrive or generators complete.
_Avoid_: motion stack, movement controller

**Generator**:
A strategy object that drives a Unit toward a specific behavioral goal for as long as it remains active.
_Avoid_: MovementGenerator, motion type

**Movement Slot**:
One of three priority levels — Idle, Active, or Controlled — that determine which Generator is currently executing. Higher slots preempt lower ones.
_Avoid_: motion slot

**Behavioral Role**:
The movement intent category that a Generator serves: ambient (Idle, Random), scripted (Waypoint, Escort, Point), combat (Chase, Flee, Confused), or reactive (Home, Assistance).
_Avoid_: movement type, generator type

**Follower**:
A Generator that continuously tracks a target Unit, adjusting its path as the target moves and maintaining a configured range.
_Avoid_: follow target, AbstractFollower
