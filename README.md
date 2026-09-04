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
| 0 | BASE | 4 | SYM  | 8  | MSC |
| 1 | GAME | 5 | FNCL | 9  | MSE |
| 2 | NUML | 6 | FNCR | 10 | MISC (5+6 tri-layer) |
| 3 | NUMR | 7 | NAV (2+3 tri-layer) | 11 | ADJ |

Two constraints on this ordering, both easy to break by reordering layers in the keymap editor:

- **GAME must sit below every layer it activates.** ZMK resolves a key from the highest active
  layer down, and GAME is opaque (no `&trans`), so any layer with a lower index than GAME is
  masked while GAME is on. That is why GAME is index 1.
- **Layer indices are hardcoded outside the keymap.** `is_touching_processor` (MSE) and the
  `trackpad_listener` scroller (MSC/SYM) in
  [boards/shields/toucan/toucan.dtsi](boards/shields/toucan/toucan.dtsi) reference indices
  directly and are not updated by the keymap editor.

## Trackpad behaviour

Two independent scroll paths, both in [boards/shields/toucan/toucan.dtsi](boards/shields/toucan/toucan.dtsi)
and [toucan_right.overlay](boards/shields/toucan/toucan_right.overlay):

- **Layer-drag scroll** — hold MSC (8) or SYM (4) and drag one finger. Uses
  `zip_scroll_transform INPUT_TRANSFORM_Y_INVERT` to keep the same vertical direction as the
  original Toucan (upstream Toucan2 ships `X_INVERT`, which flips vertical relative to that).
- **Two-finger scroll** — native to the Azoteq TPS43 driver, direction set by `invert-scroll-y`
  on the `tps43_trackpad` node. Remove that property to flip it.

Touching the trackpad momentarily activates MSE (layer 9) via `is_touching_processor`, turning the
thumb keys into mouse clicks.

# License

The code in this repo is available under the MIT license.

The included shield nice_view_gem is modified from https://github.com/M165437/nice-view-gem licensed under the MIT License.

The linked trackpad module is based on https://github.com/geeksville/zmk_driver_azoteq

ZMK code snippets are taken from the ZMK documentation under the MIT license.

The embedded font QuinqueFive is designed by GGBotNet, licensed under under the SIL Open Font License, Version 1.1.
