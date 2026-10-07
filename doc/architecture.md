# xagna architecture

Chemzs owns the xAGNA mod and authors all animations (OMF) and configs (DLTX). Damian provides the hosting and the coding. This repo is the script layer only: it wires Chemzs's motions into the game additively and never replaces a vanilla file.

![stack](img/xagna-architecture.png)

## Files

- `xagna_core.script` — data only. The role-to-motion map, the PDA state list, the MCM defaults, and a capability-probe helper.
- `xagna_mcm.script` — `on_mcm_load`. The per-role "Replace X animations" switches and the PDA scope.
- `zzz_xagna_overrides.script` — `on_game_start`. Adds Chemzs's new animation entries with `copy_table`, mutates his changed `state_lib` fields, applies the role restores per MCM, probes his OMF-only motions before wiring them, and monkey-patches the wounded exit. The `zzz_` prefix makes it run last so its edits win.

## Patterns

- Additive merge: `copy_table` into `state_mgr_animation_list.animations`.
- Field mutation: `state_lib.states[k].field = value`, never a table replace.
- Monkey-patch: wrap `action_wounded.initialize` at `on_game_start`.
- Capability probe: check a motion exists before wiring an OMF-only entry.

## Compatibility

The vanilla role motions and `free_facer` exist in vanilla, so the restores resolve on any human OMF. `norm_torso_pda` and `uni_anim` are xAGNA-only, so the PDA entries wire only behind the probe. No full-file override, so the layer coexists with other mods and survives a base-game update.
