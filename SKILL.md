---
name: edem-modeling-simulation
description: Automate Altair EDEM modeling, simulation runs, and post-processing. Use when creating or modifying EDEM decks, running solver cases or parameter studies, extracting EDEMpy results, or producing reproducible DEM reports; do not invent material/contact calibration parameters or engineering acceptance criteria.
---

# EDEM Modeling, Simulation, And Post-Processing

Use this skill for repeatable Altair EDEM work that spans model/deck preparation, solver execution, and result extraction. Prefer official EDEM tools and version-matched Python APIs over manual file edits. Keep every workflow reproducible: record software version, input deck, changed parameters, solver settings, output paths, and assumptions.

## Scope

Use this skill for:

- Creating or modifying EDEM decks when geometry, particles, factories, materials, contact models, solver settings, and output requirements are supplied by the user or an existing project.
- Running single EDEM simulations, batch runs, or parameter sweeps.
- Extracting particle, geometry, contact, collision, custom property, and time-history data with EDEMpy.
- Exporting CSV/Excel tables, plots, screenshots, animations, and concise engineering summaries from completed runs.
- Checking solver logs and result files for completion, warnings, missing datasets, or unit consistency.

Do not use this skill to invent calibrated DEM inputs. Material properties, contact parameters, rolling/static friction, restitution, cohesion, particle-size distributions, factory rates, timestep limits, and validation thresholds must come from the user, a cited source, lab calibration, or an existing deck. If they are absent, create a clearly labeled template or demonstration model and mark all unverified values as placeholders.

## First Checks

Before mutating or running anything, identify:

1. EDEM version and install path.
2. Whether the task uses an existing `.dem` deck, a new deck, or a batch template.
3. Required units and coordinate conventions.
4. Geometry sources such as STL/CAD files, primitive shapes, factory positions, and motion definitions.
5. Material/contact model data source and whether parameters are calibrated.
6. Solver end time, timestep or auto timestep setting, save interval, CPU/GPU preference, and license constraints.
7. Requested output variables, time range, spatial regions, and report format.

If any required engineering fact is missing, proceed only when a reasonable non-destructive action remains, such as reading an existing deck, preparing a script scaffold, or building a clearly labeled demo model. Ask the user for missing facts before presenting unverified DEM results as engineering conclusions.

## Modeling Workflow

- Use EDEM Creator, EDEMpy, the EDEM API, or supported command-line tools that match the installed EDEM version. APIs differ between versions, so inspect local examples or documentation when signatures are uncertain.
- For an existing deck, open it read-only first and save modified copies to a new run folder unless the user explicitly asks to overwrite the original.
- For a new model, create a parameter manifest that lists geometry dimensions, material names, contact model, particle distribution, factory flow, gravity, boundaries, timestep, and output interval.
- Import CAD/STL geometry from user-provided paths. Preserve original geometry files and write transformed/exported copies into the run folder.
- Use meaningful names for materials, geometries, particle types, factories, and analysis regions so post-processing scripts can address them reliably.
- Record every automated edit in a plain text, Markdown, JSON, or CSV summary next to the generated deck.

## Simulation Workflow

- Put each run in its own folder containing the deck copy, run manifest, solver log, and generated results. Never overwrite previous solver outputs silently.
- For parameter sweeps, create one folder per case and write a master table with case id, varied values, fixed settings, run status, elapsed time, and output files.
- Launch only the requested number of solver cases. Before large sweeps, verify one representative case can start and produce expected outputs.
- Capture the exact solver executable, command arguments, EDEM version, GPU/CPU setting, thread count if relevant, and license-related constraints.
- Treat a simulation as complete only after checking solver exit state and logs or result metadata. Flag incomplete runs, excessive overlaps, energy/contact warnings, missing saves, or empty result files.

## Post-Processing Workflow

- Use EDEMpy for `.dem` result extraction whenever available. Use the EDEM-bundled Python environment or a project virtual environment created from it; do not modify the installation-wide Python environment.
- Extract only the variables needed for the requested analysis. Common outputs include particle count, mass flow, velocity, residence time, force, torque, collision energy, contact count, wear indices, segregation metrics, and geometry loads.
- Keep units explicit in every CSV, plot axis, and report table. When EDEM returns SI or model units, state the convention used.
- Save post-processing scripts, exported tables, plots, and summaries next to the corresponding run folder.
- When comparing cases, report both raw outputs and derived metrics, and distinguish solver output from analytical calculations or assumptions.

## Deliverables

For normal tasks, return the most useful subset of:

- Generated or modified `.dem` deck path.
- Run folder path and solver log path.
- Parameter manifest or case table.
- CSV/XLSX result tables.
- Plots or screenshots with clear labels and units.
- A short summary of completed cases, failed cases, key metrics, software version, and assumptions requiring engineering review.

## Safety Boundaries

- Do not delete or overwrite user decks, CAD files, result databases, or calibration files unless the user explicitly requests it.
- Do not change global EDEM installation files, license settings, or shared material libraries unless the user explicitly requests that exact action.
- Do not claim a model is validated merely because the solver finished. Validation requires user-provided experimental data, acceptance criteria, or a cited benchmark.
- If EDEM is unavailable, create scripts/manifests that can run on the target machine and clearly state that the solver execution was not performed.

