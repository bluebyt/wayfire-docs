# Bindings available by default

The following bindings are available by default in 0.8.0, i.e. without needing to specify them in `wayfire.ini`. Capital-letter representations of alphabetical keys are used for clarity; use of <kbd>&lt;shift&gt;</kbd> is not implied nor required.

## Core

### Close focused window.

* `core.close_top_view` = <kbd>&lt;super&gt;</kbd> + <kbd>Q</kbd> | <kbd>&lt;alt&gt;</kbd> + <kbd>&lt;f4&gt;</kbd>

### Focus window with mouse.

* `core.focus_buttons` = <kbd>&lt;mouse-left&gt;</kbd> | <kbd>&lt;mouse-middle&gt;</kbd> | <kbd>&lt;mouse-right&gt;</kbd>

## Plugins

The following bindings become available by default once the relevant plugin is enabled in `wayfire.ini`.

### Change opacity by scrolling with Super + Alt.

* `alpha.modifier` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;alt&gt;</kbd>

### Show the current workspace row as a cube.

* `cube.activate` = <kbd>&lt;ctrl&gt;</kbd> + <kbd>&lt;alt&gt;</kbd> + <kbd>&lt;mouse-left&gt;</kbd>

### Show an overview of all workspaces.

* `expo.toggle` = <kbd>&lt;super&gt;</kbd>

### Select a workspace.

Workspaces are arranged into a grid of 3 × 3. The numbering is left to right, line by line:

```
⇱ k ⇲
h ⏎ l
⇱ j ⇲
```

See `core.vwidth` and `core.vheight` for configuring the grid.
 
* `expo.select_workspace_1` = <kbd>1</kbd>
* `expo.select_workspace_2` = <kbd>2</kbd>
* `expo.select_workspace_3` = <kbd>3</kbd>
* `expo.select_workspace_4` = <kbd>4</kbd>
* `expo.select_workspace_5` = <kbd>5</kbd>
* `expo.select_workspace_6` = <kbd>6</kbd>
* `expo.select_workspace_7` = <kbd>7</kbd>
* `expo.select_workspace_8` = <kbd>8</kbd>
* `expo.select_workspace_9` = <kbd>9</kbd>

### Simple active window switcher.

* `fast-switcher.activate` = <kbd>&lt;alt&gt;</kbd> + <kbd>&lt;esc&gt;</kbd>
* `fast-switcher.activate_backward` = <kbd>&lt;alt&gt;</kbd> + <kbd>&lt;shift&gt;</kbd> + <kbd>&lt;esc&gt;</kbd>

### Fisheye effect.

* `fisheye.toggle` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;ctrl&gt;</kbd> + <kbd>F</kbd>

### Grid.

Position the windows in certain regions of the output:

```
⇱ ↑ ⇲   │ 7 8 9
← f →   │ 4 5 6
⇱ ↓ ⇲ d │ 1 2 3 0
```

* `grid.slot_bl` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;kp1&gt;</kbd>
* `grid.slot_b` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;kp2&gt;</kbd>
* `grid.slot_br` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;kp3&gt;</kbd>
* `grid.slot_l` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;left&gt;</kbd> | <kbd>&lt;super&gt;</kbd> + <kbd>&lt;kp4&gt;</kbd>
* `grid.slot_c` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;up&gt;</kbd> | <kbd>&lt;super&gt;</kbd> + <kbd>&lt;kp5&gt;</kbd>
* `grid.slot_r` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;down&gt;</kbd> | <kbd>&lt;super&gt;</kbd> + <kbd>&lt;kp6&gt;</kbd>
* `grid.slot_tl` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;kp7&gt;</kbd>
* `grid.slot_t` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;kp8&gt;</kbd>
* `grid.slot_tr` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;kp9&gt;</kbd>

Restore default.

* `grid.restore` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;down&gt;</kbd> | <kbd>&lt;super&gt;</kbd> + <kbd>&lt;kp0&gt;</kbd>

### Invert the colors of the whole output.

* `invert.toggle` = <kbd>&lt;super&gt;</kbd> + <kbd>I</kbd>

### Drag windows by holding down Super and left mouse button

* `move.activate` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;mouse-left&gt;</kbd>

### Change focused output.

Switch to the next output.

* `oswitch.next_output` = <kbd>&lt;super&gt;</kbd> + <kbd>O</kbd>

Same with the window.

* `oswitch.next_output_with_win` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;shift&gt;</kbd> + <kbd>O</kbd>

### Resize window.

* `resize.activate` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;mouse-left&gt;</kbd>

### Toggle scaling.

* `scale.toggle` = <kbd>&lt;super&gt;</kbd> + <kbd>P</kbd>

### Sway-inspired tiling.

Toggle tiling mode.

* `simple-tile.key_toggle` = <kbd>&lt;super&gt;</kbd> + <kbd>T</kbd>

Move window.

* `simple-tile.button_move` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;mouse-left&gt;</kbd>

Resize window.

* `simple-tile.button_resize` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;mouse-right&gt;</kbd>

Move focus to window on left.

* `simple-tile.key_focus_left` = <kbd>&lt;super&gt;</kbd> + <kbd>H</kbd>

Move focus to window on right.

* `simple-tile.key_focus_right` = <kbd>&lt;super&gt;</kbd> + <kbd>L</kbd>

Move focus to window above.

* `simple-tile.key_focus_above` = <kbd>&lt;super&gt;</kbd> + <kbd>K</kbd>

Move focus to window below.

* `simple-tile.key_focus_below` = <kbd>&lt;super&gt;</kbd> + <kbd>J</kbd>

### Change active window with an animation.

* `switcher.next_view` = <kbd>&lt;alt&gt;</kbd> + <kbd>&lt;tab&gt;</kbd>
* `switcher.prev_view` = <kbd>&lt;alt&gt;</kbd> + <kbd>&lt;shift&gt;</kbd> + <kbd>&lt;tab&gt;</kbd>

### Switch to workspace.

* `vswitch.binding_left` = <kbd>&lt;ctrl&gt;</kbd> + <kbd>&lt;super&gt;</kbd> + <kbd>&lt;left&gt;</kbd>
* `vswitch.binding_down` = <kbd>&lt;ctrl&gt;</kbd> + <kbd>&lt;super&gt;</kbd> + <kbd>&lt;down&gt;</kbd>
* `vswitch.binding_up` = <kbd>&lt;ctrl&gt;</kbd> + <kbd>&lt;super&gt;</kbd> + <kbd>&lt;up&gt;</kbd>
* `vswitch.binding_right` = <kbd>&lt;ctrl&gt;</kbd> + <kbd>&lt;super&gt;</kbd> + <kbd>&lt;right&gt;</kbd>

Move the focused window with the same key-bindings as above, but with Shift.

* `vswitch.with_win_left` = <kbd>&lt;ctrl&gt;</kbd> + <kbd>&lt;super&gt;</kbd> + <kbd>&lt;shift&gt;</kbd> + <kbd>&lt;left&gt;</kbd>
* `vswitch.with_win_down` = <kbd>&lt;ctrl&gt;</kbd> + <kbd>&lt;super&gt;</kbd> + <kbd>&lt;shift&gt;</kbd> + <kbd>&lt;down&gt;</kbd>
* `vswitch.with_win_up` = <kbd>&lt;ctrl&gt;</kbd> + <kbd>&lt;super&gt;</kbd> + <kbd>&lt;shift&gt;</kbd> + <kbd>&lt;up&gt;</kbd>
* `vswitch.with_win_right` = <kbd>&lt;ctrl&gt;</kbd> + <kbd>&lt;super&gt;</kbd> + <kbd>&lt;shift&gt;</kbd> + <kbd>&lt;right&gt;</kbd>

### Rotate windows with the mouse.

2D rotation.

* `wrot.activate` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;ctrl&gt;</kbd> + <kbd>&lt;mouse-right&gt;</kbd>

3D rotation.

* `wrot.activate-3d` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;shift&gt;</kbd> + <kbd>&lt;mouse-right&gt;</kbd>

Remove rotation of current view.

* `wrot.reset-one` = <kbd>&lt;super&gt;</kbd> + <kbd>R</kbd>

Remove rotation of all views.

* `wrot.reset` = <kbd>&lt;super&gt;</kbd> + <kbd>&lt;ctrl&gt;</kbd> + <kbd>R</kbd>

### Zoom in the desktop by scrolling + Super.

* `zoom.modifier` = <kbd>&lt;super&gt;</kbd>
