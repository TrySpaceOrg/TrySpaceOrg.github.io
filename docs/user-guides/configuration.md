# Configuration

This page documents the orchestrator and configuration files used by TrySpace Lab.

## Primary config files (in `cfg/`)
* `tryspace-config.yaml` - global configuration that lists available missions and global defaults.
* Mission config files - referenced from `tryspace-config.yaml` (each mission entry contains a `config_file` and `scenarios`).
* Scenario config files - referenced from a mission config and provide scenario-specific settings and optional `overrides`.
* `active.yaml` - the current selection of `mission` and `scenario` (created automatically with defaults if missing).
* `build.yaml` - a merged, human-readable snapshot written by the orchestrator for inspection.

## Orchestrator (`cfg/tryspace-orchestrator.py`)
* Loads `active.yaml`, `tryspace-config.yaml`, the selected mission, and the selected scenario.
* If `active.yaml` does not exist, it will create one using the first mission and scenario found in `tryspace-config.yaml` / mission config.
* Merges configuration layers and writes the merged result to `cfg/build.yaml`.
* For each component listed under the mission's `components` key the orchestrator performs a cascading merge and attempts to render component templates.

## Merge/override precedence (applied in this order, later items win)
* Fallback component defaults: `comp/<component>/support/device_config.yaml` (if present)
* Global config (`tryspace-config.yaml`) for the component
* Mission-level config for the component
* Scenario-level config for the component
* Scenario-level `overrides` (flat dict applied last)

## Template rendering
* The orchestrator looks for a Jinja2 template at `comp/<component>/support/device_config.j2`
* If a template and merged config exist, it renders the template with `config` and writes the output to `comp/<component>/shared/device_cfg.h` (creates directories as needed)

## Running the orchestrator
Run via make:

```bash
make cfg
```

Also can run directly:

```bash
python3 cfg/tryspace-orchestrator.py
```

## Changing the active mission/scenario
Edit `cfg/active.yaml` and set the desired `mission` and `scenario`, then run the orchestrator to regenerate `cfg/build.yaml` and component configs.

Example `cfg/active.yaml`:

```yaml
mission: drm
scenario: nominal
```

## Notes & tips
* Inspect `cfg/build.yaml` after a run to verify the merged configuration
* Component templates expect a flattened config in the `config` context — place component-specific keys under the component's top-level key in global/mission/scenario files
