# Home Assistant config

Reference copies of config that lives in Home Assistant. The live versions are
edited in the HA UI; these files are here so the work isn't lost.

| File | What it is | Where it lives in HA |
|---|---|---|
| `master_bedroom_blinds.yaml` | Blinds schedule: halfway 8:30 PM, close 10:15 PM, open 9 AM. Closing steps skip if any blind is in an in-between position (tolerance 10). | Automation *Master Bedroom Blinds Schedule* |
| `lifx_spot_zones_shell_command.yaml` | `shell_command.lifx_spot_zones` — sets zone 1/2/3 of every LIFX Spot to exact colours. HA's built-in LIFX integration only scatters a palette across Spot zones; this calls `set_matrix_colors` directly via the `lifx` library HA ships. | Bottom of `configuration.yaml` (needs a restart the first time `shell_command:` is added) |

## Garden LIFX zone controls (UI-created, not in files)

- Helpers: `input_select.garden_zone_{1,2,3}_color`, `input_select.garden_all_zones_color`,
  `input_number.garden_zones_brightness`, `input_select.garden_gradient_picker`,
  `input_number.garden_gradient_hue`, `input_number.garden_gradient_end_hue`, `light.garden_spots` (group).
- Scripts: *Garden: Paint Zone Colors* (named colours → zones), *Garden: Paint Gradient*
  (LIFX-app-style Contrast/Adjacent/Bold/Complementary/Tints/Custom from a base hue).
- Automations: *Garden: Apply Zone Colors*, *Garden: All Zones Color*, *Garden: Apply Gradient*
  repaint the spots whenever a helper changes.
- Dashboard: *Master Bedroom* → Garden Lights section.

Gotcha: values passed to the shell command are wrapped in single quotes in the scripts,
because HA otherwise parses `"185,100,100,3500"` into a list and the arguments split.
