# ZMK config for beekeeb Toucan2 Keyboard (fork)

[The beekeeb Toucan2 Keyboard](https://beekeeb.com/introducing-toucan2/) is a wireless split 42-key column‑stagger keyboard that a display and a trackpad, with an aggressive stagger on the pinky columns.

# Customizations

- **Keymap**: [config/toucan.keymap](config/toucan.keymap) — 12-layer layout ported from
  [zmk-keyboard-toucan](https://github.com/ALPHAvibe/zmk-keyboard-toucan) (home-row mods, tri-layer
  NAV/MISC, mouse layer, macOS nav macros).
- **General configs**: [boards/shields/toucan/toucan_left.conf](boards/shields/toucan/toucan_left.conf) and [boards/shields/toucan/toucan_right.conf](boards/shields/toucan/toucan_right.conf)
- **Swipe shortcuts**: the `swipe_button_mapper` node in [boards/shields/toucan/toucan.dtsi](boards/shields/toucan/toucan.dtsi)
- **Invert scroll / trackpad settings**: the `tps43_trackpad` node in [boards/shields/toucan/toucan_right.overlay](boards/shields/toucan/toucan_right.overlay)

## Layers

| # | Name | # | Name | # | Name |
|---|------|---|------|---|------|
| 0 | BASE | 4 | ADJ  | 8  | NAV (1+2 tri-layer, i.e. 6+7) |
| 1 | GAME | 5 | MSE  | 9  | FNCL |
| 2 | MSC  | 6 | NUML | 10 | FNCR |
| 3 | SYM  | 7 | NUMR | 11 | MISC (9+10 tri-layer) |

## Trackpad behaviour

Two independent scroll paths, both in [boards/shields/toucan/toucan.dtsi](boards/shields/toucan/toucan.dtsi)
and [toucan_right.overlay](boards/shields/toucan/toucan_right.overlay):

- **Layer-drag scroll** — hold MSC (2) or SYM (3) and drag one finger. Uses
  `zip_scroll_transform INPUT_TRANSFORM_Y_INVERT` to keep the same vertical direction as the
  original Toucan (upstream Toucan2 ships `X_INVERT`, which flips vertical relative to that).
- **Two-finger scroll** — native to the Azoteq TPS43 driver, direction set by `invert-scroll-y`
  on the `tps43_trackpad` node. Remove that property to flip it.

Touching the trackpad momentarily activates MSE (layer 5) via `is_touching_processor`, turning the
thumb keys into mouse clicks.

# License

The code in this repo is available under the MIT license.

The included shield nice_view_gem is modified from https://github.com/M165437/nice-view-gem licensed under the MIT License.

The linked trackpad module is based on https://github.com/geeksville/zmk_driver_azoteq

ZMK code snippets are taken from the ZMK documentation under the MIT license.

The embedded font QuinqueFive is designed by GGBotNet, licensed under under the SIL Open Font License, Version 1.1.
