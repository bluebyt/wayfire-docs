# Bindings available by default

The following bindings are available by default in 0.8.0, i.e. without needing to specify them in `wayfire.ini`. Capital-letter representations of alphabetical keys are used for clarity; use of ++shift++ is not implied nor required.

## Core

### Close focused window.

* `core.close_top_view` = ++super++ + ++q++ | ++alt++ + ++f4++

### Focus window with mouse.

* `core.focus_buttons` = `mouse-left` | `mouse-middle` | `mouse-right`

## Plugins

The following bindings become available by default once the relevant plugin is enabled in `wayfire.ini`.

### Change opacity by scrolling with Super + Alt.

* `alpha.modifier` = ++super++ + ++alt++

### Show the current workspace row as a cube.

* `cube.activate` = ++ctrl++ + ++alt++ + `mouse-left`

### Show an overview of all workspaces.

* `expo.toggle` = ++super++

### Select a workspace.

Workspaces are arranged into a grid of 3 × 3. The numbering is left to right, line by line:

```
⇱ k ⇲
h ⏎ l
⇱ j ⇲
```

See `core.vwidth` and `core.vheight` for configuring the grid.

* `expo.select_workspace_1` = ++1++
* `expo.select_workspace_2` = ++2++
* `expo.select_workspace_3` = ++3++
* `expo.select_workspace_4` = ++4++
* `expo.select_workspace_5` = ++5++
* `expo.select_workspace_6` = ++6++
* `expo.select_workspace_7` = ++7++
* `expo.select_workspace_8` = ++8++
* `expo.select_workspace_9` = ++9++

### Simple active window switcher.

* `fast-switcher.activate` = ++alt++ + ++esc++
* `fast-switcher.activate_backward` = ++alt++ + ++shift++ + ++esc++

### Fisheye effect.

* `fisheye.toggle` = ++super++ + ++ctrl++ + ++f++

### Grid.

Position the windows in certain regions of the output:

```
⇱ ↑ ⇲   │ 7 8 9
← f →   │ 4 5 6
⇱ ↓ ⇲ d │ 1 2 3 0
```

* `grid.slot_bl` = ++super++ + `kp1`
* `grid.slot_b` = ++super++ + `kp2`
* `grid.slot_br` = ++super++ + `kp3`
* `grid.slot_l` = ++super++ + ++left++ | ++super++ + `kp4`
* `grid.slot_c` = ++super++ + ++up++ | ++super++ + `kp5`
* `grid.slot_r` = ++super++ + ++down++ | ++super++ + `kp6`
* `grid.slot_tl` = ++super++ + `kp7`
* `grid.slot_t` = ++super++ + `kp8`
* `grid.slot_tr` = ++super++ + `kp9`

Restore default.

* `grid.restore` = ++super++ + ++down++ | ++super++ + ++kp0++

### Invert the colors of the whole output.

* `invert.toggle` = ++super++ + ++i++

### Drag windows by holding down Super and left mouse button

* `move.activate` = ++super++ + `mouse-left`

### Change focused output.

Switch to the next output.

* `oswitch.next_output` = ++super++ + ++o++

Same with the window.

* `oswitch.next_output_with_win` = ++super++ + ++shift++ + ++o++

### Resize window.

* `resize.activate` = ++super++ + `mouse-left`

### Toggle scaling.

* `scale.toggle` = ++super++ + ++p++

### Sway-inspired tiling.

Toggle tiling mode.

* `simple-tile.key_toggle` = ++super++ + ++t++

Move window.

* `simple-tile.button_move` = ++super++ + `mouse-left`

Resize window.

* `simple-tile.button_resize` = ++super++ + `mouse-right`

Move focus to window on left.

* `simple-tile.key_focus_left` = ++super++ + ++h++

Move focus to window on right.

* `simple-tile.key_focus_right` = ++super++ + ++l++

Move focus to window above.

* `simple-tile.key_focus_above` = ++super++ + ++k++

Move focus to window below.

* `simple-tile.key_focus_below` = ++super++ + ++j++

### Change active window with an animation.

* `switcher.next_view` = ++alt++ + ++tab++
* `switcher.prev_view` = ++alt++ + ++shift++ + ++tab++

### Switch to workspace.

* `vswitch.binding_left` = ++ctrl++ + ++super++ + ++left++
* `vswitch.binding_down` = ++ctrl++ + ++super++ + ++down++
* `vswitch.binding_up` = ++ctrl++ + ++super++ + ++up++
* `vswitch.binding_right` = ++ctrl++ + ++super++ + ++right++

Move the focused window with the same key-bindings as above, but with Shift.

* `vswitch.with_win_left` = ++ctrl++ + ++super++ + ++shift++ + ++left++
* `vswitch.with_win_down` = ++ctrl++ + ++super++ + ++shift++ + ++down++
* `vswitch.with_win_up` = ++ctrl++ + ++super++ + ++shift++ + ++up++
* `vswitch.with_win_right` = ++ctrl++ + ++super++ + ++shift++ + ++right++

### Rotate windows with the mouse.

2D rotation.

* `wrot.activate` = ++super++ + ++ctrl++ + `mouse-right`

3D rotation.

* `wrot.activate-3d` = ++super++ + ++shift++ + `mouse-right`

Remove rotation of current view.

* `wrot.reset-one` = ++super++ + ++r++

Remove rotation of all views.

* `wrot.reset` = ++super++ + ++ctrl++ + ++r++

### Zoom in the desktop by scrolling + Super.

* `zoom.modifier` = ++super++
