# Home Assistant config

Reference copies of config that lives in Home Assistant. The live versions are
edited in the HA UI; these files are here so the work isn't lost.

| File | What it is | Where it lives in HA |
|---|---|---|
| `master_bedroom_blinds.yaml` | Evening blinds schedule: 8:30 PM halfway (Slider 62%, blinds already down left alone), 10:15 PM close. Both skip while a blind is at an odd position (±15%; Back Right counts as open from 80%). | Automation *Master Bedroom Blinds Schedule* |
| `master_bedroom_blinds_morning.yaml` | 9:00 AM open, no conditions, with a retry 90 s later for any blind that stopped short. | Automation *Master Bedroom Blinds Morning Open* |
| `slider_close_prompt.yaml` | Hourly 10 PM–5 AM phone prompt (iPhone 18 Pro Max) to close the Slider if it's open; closes it anyway after 15 minutes with no answer. | Automation *Slider Close Prompt* |
| `rheem_emergency_heat.yaml` | Emergency Heat switch for the Rheem (writes thermostat STATMODE=5 via the esphome-econet `write_enum` action; turning it off returns the system to the mode it was in before) and the automation that clears its flag when the mode changes elsewhere. | Switch *Rheem Emergency Heat*, automation *Rheem Emergency Heat Ended Elsewhere* |
| `rheem_compressor_started_alert.yaml` | Logbook entry + quiet iPhone note every time the heat pump compressor starts; time-sensitive if it starts during Emergency Heat. | Automation *Rheem Compressor Started Alert* |
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

## Slider keep-open flag (UI-created)

- `input_boolean.slider_keep_open_tonight`: turned on by choosing *Leave open* on the
  phone prompt; the 10:15 PM close skips the Slider while it's on. Cleared when the
  prompt closes the Slider and by the 9 AM open.

Gotcha: values passed to the shell command are wrapped in single quotes in the scripts,
because HA otherwise parses `"185,100,100,3500"` into a list and the arguments split.
