# Quadcopter

Declarative mod `0sfs/quadcopter`. It has no aircraft and no flight code.

Every quadcopter mod depends on `0SFS/Quadcopter`. The manifest's `quadcopter` object is the shared contract:

- The stick deadzone, the position-hold gain `kPos` and the position-hold speed cap.
- 30% image overlap (`gridOverlap` 0.7) for a spherical still grid.
- Physics at 120 Hz while one of these aircraft is selected, and the gimbal slew rate.
- The names of the JSBSim properties the rotors' wrench is written to. These are `multirotor/force-x-lbs`, `force-y-lbs` and `force-z-lbs` (body axes, at the CG), `multirotor/moment-l-ftlbf`, `moment-m-ftlbf` and `moment-n-ftlbf`, and `multirotor/armed`.

`sdk.run()` stays the integrator. The rotors come from `@0sfs/multirotor`'s rotor plant and flight controller, simulated down to the spinning propeller with no motor, ESC or battery model. Wind is the host's weather wind, applied by JSBSim.

Each airframe mod supplies its own data in its manifest's `quad` section:

- flight modes;
- the propeller and the rotor layout;
- the CG and the controller gains.

Its JSBSim file holds the mass, inertia, four feet and body drag, and applies the wrench at the CG. The flight controller and `drone-camera` WebGPU capture/optical flow load only when a quadcopter is applied. Camera models are separate dependencies of their airframes.

Do not publish this repository from a local task. Topics, when published: `0sfs-mod`, `0sfs-plugin`.

```sh
node scripts/validate-mod.mjs
```

Presentation components: `0SFS/rotating-prop` and `0SFS/camera-gimbal`. Airframes declare camera follow defaults (`momentum` for quadcopters), rotor pivots, blade counts, radii and rotation directions. The props spin at the rotor speed the simulation computes. Shared components run in the host, not in remote mod code.
