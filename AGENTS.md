# AGENTS.md · assetto-corsa-nos-purge

Guide for coding agents. What it is, how it is installed and how it is configured is in `README.md` (and `README.es.md`).

## What it is

A single Lua script (`nos_purge.lua`, 23 lines) that adds NOS Purge-style smoke to an Assetto Corsa car. It uses CSP's (Custom Shaders Patch) particle system and is activated with the `ExtraE` button in Content Manager. There is no build, no dependencies and no tests.

## Structure

- `nos_purge.lua`: the script. It defines the properties of `ac.Particles.Smoke` and, in `script.update`, emits two columns of smoke when `car.extraE` is `true`.
- `README.md` / `README.es.md`: installation, `ext_config.ini` configuration, properties table and the copied script.
- `assets/`: image for the README header.
- `LICENSE` (MIT).

## Commands

There are no build, test or lint scripts. The only possible checks are:

- Reviewing the Lua syntax by hand or with a Lua linter, if the environment has one. None is configured in the repo.
- Testing in the game: copy `nos_purge.lua` to the `extension` folder of a car, add the `[SCRIPT_0]` entry to its `ext_config.ini`, activate `ExtraE` in Content Manager and drive. This is the only real verification.

## How to test a change

- There are no automated tests. Document in the PR or commit what was tested in the game: car, CSP version and whether the smoke was seen.
- A change that only touches values (`color`, `thickness`, `life`, `size`, `spreadK`, `growK`, `targetYVelocity`) must be checked visually; it cannot be verified from the repo.

## Conventions

- Lua with the CSP API (`ac.Particles.Smoke`, `script.update`, `car.extraE`, `rgbm`, `vec3`). That API depends on the CSP version; do not assume it without checking its documentation.
- Comments and property names are in English, as they are in the script. The README documentation is in English and Spanish (`README.md` and `README.es.md`); change both at the same time.
- No version or CHANGELOG in the repo.

## Decisions and pitfalls

- **Fixed coordinates.** The two smoke columns are emitted from `vec3(-0.68, 0.95, 0.7)` and `vec3(0.68, 0.95, 0.7)`, with fixed velocities. They are hard-coded for the bodywork of a specific model [inference: adjusted to an example car]. Changing car requires recalculating them.
- **README values in sync with the script.** The README properties table reflects the values in `nos_purge.lua`. If you change one, change the table and the code block in both READMEs.
- **Activation.** The script only emits if `car.extraE` is `true`. Without the `ExtraE` key active in Content Manager nothing appears, even if the script is correct.
- **`ext_config.ini`.** The `[SCRIPT_0]` entry has `ACTIVE_FOR_NEAREST=4` and `ACTIVE_FOR_UNFOCUSED=1` according to the README; changing them affects which cars run the script.
- **Performance.** The README describes it as lightweight. There is no measurement in the repo: do not claim it without testing.

## Maintenance

- This file and the `README.md` are updated in the same change that makes them outdated.
- Only publishable content goes here: no local paths, machine ports, IPs, secret profiles or personal data.
