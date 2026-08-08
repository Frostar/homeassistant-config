# Home Assistant Configuration

Home Assistant configuration for the Frostar homelab, running on a dedicated
Raspberry Pi (Home Assistant OS). This repository is the live `/config`
directory under git.

## Structure

| Path                 | Purpose                                                            |
| -------------------- | ------------------------------------------------------------------ |
| `configuration.yaml` | Core config; loads `packages/` and the split YAML files            |
| `packages/`          | Per-domain packages (logger, lovelace, recorder)                   |
| `automations.yaml`   | UI-managed automations                                             |
| `lovelace/`          | YAML dashboards, one file per view                                 |
| `custom_components/` | HACS integrations — only `manifest.json` tracked (version pinning) |
| `esphome/`           | ESPHome device configs                                             |
| `www/`               | Static assets; HACS-managed `www/community/` is not tracked        |
| `zigbee2mqtt/`       | Zigbee2MQTT add-on data — config is **not** tracked (holds keys)   |

## Secrets

No credential values are committed:

- All sensitive values are referenced via `!secret`; `secrets.yaml` is ignored.
- `.storage/`, `ip_bans.yaml`, `known_devices.yaml` and Zigbee2MQTT
  configuration (MQTT password, Zigbee network key) are ignored.
- The `.gitignore` is whitelist-based: everything is ignored by default and
  tracked paths are explicitly allowed.

## CI

GitHub Actions runs on every push and pull request:

- **yamllint** — style/sanity check of all tracked YAML (`.yamllint`).
- **Home Assistant config check** — validates the configuration against the
  current stable Home Assistant release, using stub secrets from
  `.github/ci-secrets.yaml`.

## Workflow

Changes are made on the Pi (UI or editor) and committed from `/config`.
The Pi has no GitHub credentials; pushes go through a workstation clone.
