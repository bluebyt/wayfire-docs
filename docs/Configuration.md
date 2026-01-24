# Configuration file and options

**Table of contents**

- [Overview](#overview)
  - [Location](#location)
- [Syntax](#syntax)
- [Values](#values)
- [Options](#options)
  - [Accessibility](#accessibility)
    - [Invert](#invert)
    - [Zoom](#zoom)
  - [Desktop](#desktop)
    - [Alpha](#alpha)
    - [Cube](#cube)
    - [Expo](#expo)
    - [Idle](#idle)
    - [Output switcher](#output-switcher)
    - [Scale](#scale)
    - [Window rules](#window-rules)
    - [Viewport switcher](#viewport-switcher)
    - [Viewport swipe](#viewport-swipe)
    - [Workspace sets](#workspace-sets)
  - [Effects](#effects)
    - [Animate](#animate)
    - [Blur](#blur)
    - [Decoration](#decoration)
    - [Fisheye](#fisheye)
    - [Wobbly](#wobbly)
    - [Window rotate](#window-rotate)
  - [General](#general)
    - [Autostart](#autostart)
    - [Command](#command)
    - [Core](#core)
    - [Input](#input)
    - [Output](#output)
    - [Workarounds](#workarounds)
  - [Utility](#utility)
    - [Session lock protocol](#session-lock-protocol)
    - [XDG Activation protocol](#xdg-activation-protocol)
    - [Input Methods protocol](#input-method-protocols)
  - [Window management](#window-management)
    - [Fast switcher](#fast-switcher)
    - [Grid](#grid)
    - [Move](#move)
    - [Place](#place)
    - [Preserve output](#preserve-output)
    - [Simple tile](#simple-tile)
    - [Switcher](#switcher)
    - [Resize](#resize)
    - [WM-Actions](#wm-actions)

## Overview

The Wayfire configuration file uses an [INI] file format, similar to [Git].
It contains a number of options that affect the Wayland compositor’s behavior.

[Example](https://github.com/WayfireWM/wayfire/blob/master/wayfire.ini)

[INI]: https://en.wikipedia.org/wiki/INI_file
[Git]: https://git-scm.com

#### Notation

The variables are divided into sections, wherein the fully qualified variable name of the variable itself is the last dot-separated segment and the section name is everything before the last dot.

### Location

Wayfire searches for a config file in the following locations, in this order:

1. $WAYFIRE_CONFIG_FILE
2. $XDG_CONFIG_HOME/wayfire.ini
3. ~/.config/wayfire.ini

If unset, $XDG_CONFIG_HOME defaults to ~/.config.
After wayfire is started, it will write the current configuration path to the environment variable WAYFIRE_CONFIG_FILE.

If you need to customize the configuration path， set the WAYFIRE_CONFIG_FILE variable before wayfire starts.

## Syntax

The syntax is fairly flexible and permissive; whitespaces are mostly ignored.
The `#` characters begin comments to the end of line, blank lines are ignored.

The file consists of sections and variables.
A section begins with the name of the section in square brackets and continues until the next section begins.
Each variable must belong to some section, which means that there must be a section header before the first setting of a variable.

All the other lines (and the remainder of the line after the section header) are recognized as setting variables, in the form `name = value`.

If a `value` contains the `#` character it should be escaped using `\`.

## Values

Values of many variables are treated as a simple string, but there are variables that take values of specific types and there are rules as to how to spell them.

### `boolean`

When a variable is said to take a boolean value, `true` or `false`.

#### `integer`

The value for numbers.

#### `double`

The value for floating point numbers.

#### `color`

The value for a variable that takes a color is a tuple of four floating point numbers (between 0 and 1) in [RGBA] format.

[RGBA]: https://en.wikipedia.org/wiki/RGBA_color_model

#### `key` and `button`

The value represents a key combo, also called chord.
A key combo is a key sequence in which the keys are pressed at the same time.

Keys are space-separated and can be:

- Modifier
    - `<shift>`
    - `<ctrl>`
    - `<alt>`
    - `<super>`
- Key
- Button

See [Keys] for a complete reference of key and button names.
The document is based on [`input-event-codes.h`] from the [Linux] kernel.

[Keys]: https://github.com/alexherbo2/wayfire-resources/blob/master/docs/keys.txt
[Linux]: https://kernel.org
[`input-event-codes.h`]: https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/tree/include/uapi/linux/input-event-codes.h

[`key`]: #key
[`button`]: #button

#### `gesture`

The value represents a touchscreen gesture, as follows:

```
pinch [in|out] <finger-count>
swipe [up|down|left|right] <finger-count>
edge-swipe [up|down|left|right] <finger-count>
```

Note: e.g. edge-swipe right means swiping from left to right.

[`gesture`]: #gesture

Defined in:

- [`wf-config/src/types.cpp`](https://github.com/WayfireWM/wf-config/blob/master/src/types.cpp)


Set input.gesture_sensitivity to change the sensitivity of the built-in gesture detection. Higher values mean that less finger movement is required to trigger the gesture. Default 1.0, min 0.1, max 5.0. Changing requires restart. Defined in:

- [`metadata/input.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/input.xml)

#### `hotspot`
It is also possible to activate items by a "hot corner"

Example: `hotspot top-left 100x10 1000`

where 100x10 is the width x height of the corner

and the 1000 being the time the input (mouse or touch) needs to be inside that area.

[`hotspot`]: #hotspot

#### `activator`

The value represents a group of bindings which can all be used for the same action.

The bindings can be [`key`]s, [`button`]s and [`gesture`]s, [`hotspot`]s, separated by a `|`.

Example:

`<super> KEY_E | <ctrl> <alt> BTN_EXTRA | pinch in 4`

#### `criteria`

The value represents command criteria to affect certain windows, as follows:

```
title [is|contains] "<string>"
app_id [is|contains] "<string>"
type is ["toplevel"|"x-or"|"unmanaged"|"background"|"panel"|"overlay"|"unknown"]
focusable is [true|false]
role is ["TOPLEVEL"|"UNMANAGED"|"DESKTOP_ENVIRONMENT"]
fullscreen is [true|false]
activated is [true|false]
minimized is [true|false]
maximized is [true|false]
floating is [true|false]
tiled-left is [true|false]
tiled-right is [true|false]
tiled-top is [true|false]
tiled-bottom is [true|false]
```

These basic criteria can be combined arbitrarily with parenthesis `(...)` and logical operators `!`, `&`, `|`.
Note that to chain three criteria together with a logical and, you need parenthesis because there is no order of the operators (so it will be `((critA & critB) & critC)`).
In addition, there are the special values `all` and `none`, which are always evaluated to `true` and `false` respectively.

Defined in:
- [`src/api/wayfire/view-access-interface.hpp`](https://github.com/WayfireWM/wayfire/blob/master/src/api/wayfire/view-access-interface.hpp)
- [`src/api/wayfire/matcher.hpp`](https://github.com/WayfireWM/wayfire/blob/master/src/api/wayfire/matcher.hpp)

[`criteria`]: #criteria

## Options

### Accessibility

#### Invert

A plugin to invert the colors of the whole output.

Defined in:

- [`metadata/invert.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/invert.xml)
- [`plugins/single_plugins/invert.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/invert.cpp)

#### `invert.toggle`

Inverts the colors of the whole output with the specified activator.

#### Zoom

A plugin to zoom in the desktop with the mouse.

Defined in:

- [`metadata/zoom.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/zoom.xml)
- [`plugins/single_plugins/zoom.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/zoom.cpp)

#### `zoom.modifier`

Scrolls with the specified modifier to zoom in and out.

#### `zoom.smoothing_duration`

Sets the smoothing duration in milliseconds.
The default is **300**.

#### `zoom.speed`

Sets the speed factor for zooming.
The default is **0.01**.

### Desktop

#### Alpha

A plugin to change the opacity of windows.

Defined in:

- [`metadata/alpha.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/alpha.xml)
- [`plugins/single_plugins/alpha.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/alpha.cpp)

#### `alpha.min_value`

Sets the minimum opacity of the window between 0 (completely transparent) and 1 (completely opaque).
The default is **0.1**.

#### `alpha.modifier`

When the modifier key is held down, you can scroll down and up to adjust the opacity of the window.

#### Cube

A plugin to show the current workspace row as a cube.

![cube](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/cube/2020-02-18_18:23:53.gif)

Defined in:

- [`metadata/cube.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/cube.xml)
- [`plugins/cube/cube.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/cube/cube.cpp)
- [`plugins/cube/cube.hpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/cube/cube.hpp)
- [`plugins/cube/cubemap.hpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/cube/cubemap.hpp)
- [`plugins/cube/simple-background.hpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/cube/simple-background.hpp)
- [`plugins/cube/skydome.hpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/cube/skydome.hpp)

#### `cube.activate`

Activates the cube with the specified button.

#### `cube.background`

Sets the background color for the simple background mode.
The default is `0.1 0.1 0.1 1.0`.

#### `cube.background_mode`

Sets the background mode.

Possible values are:

- `simple`
- `skydome`
- `cubemap`

The default is `simple`.

See [Cube mapping] for details.

[Cube mapping]: https://en.wikipedia.org/wiki/Cube_mapping

#### `cube.cubemap_image`

Loads the specified image for the cubemap background mode.

#### `cube.deform`

Specifies the deformation to be used.

Possible values are:

- 0 → None
- 1 → Cylinder
- 2 → Star

The default is **0**.

Deform values **1** and **2** are supported only with [OpenGL ES] 3.2 support.

[OpenGL ES]: https://khronos.org/opengles/

#### `cube.initial_animation`

Sets the initial animation duration in milliseconds.
The default is **350**.

#### `cube.light`

Specifies whether to use lighting.
The default is **true**.

#### `cube.rotate_left`

Rotates left with the specified activator.

#### `cube.rotate_right`

Rotates right with the specified activator.

#### `cube.skydome_mirror`

Specifies whether to mirror skydome.
The default is **true**.

#### `cube.skydome_texture`

Loads the specified texture for the skydome background mode.

#### `cube.speed_spin_horiz`

Sets the velocity of horizontal spinning.
The default is **0.02**.

#### `cube.speed_spin_vert`

Sets the velocity of vertical spinning.
The default is **0.02**.

#### `cube.speed_zoom`

Sets the speed factor for zooming.
The default is **0.07**.

#### `cube.zoom`

Sets the level of zoom out.
Setting the value to **0.0** means to not zoom out.
The default is **0.1**.

#### Expo

A plugin to show an overview of all workspaces.

![expo](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/expo/2020-02-18_18:43:52.gif)

Defined in:

- [`metadata/expo.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/expo.xml)
- [`plugins/single_plugins/expo.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/expo.cpp)

#### `expo.background`

Sets the background color.
The default is `0.1 0.1 0.1 1.0`.

#### `expo.duration`

Sets the zoom duration in milliseconds.
The default is **300**.

#### `expo.offset`

Sets the delimiter offset (in pixels) between workspaces.
The default is **10.0**.

#### `expo.select_workspace_<number>`

Selects the given workspace with the specified key.
The numbering is left to right, line by line.

#### `expo.toggle`

Shows an overview of all workspaces with the specified activator.
Pressing again exits.

#### Idle

A plugin for idle settings, such as the screensaver and [DPMS].

[DPMS]: https://en.wikipedia.org/wiki/VESA_Display_Power_Management_Signaling

Defined in:

- [`metadata/idle.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/idle.xml)
- [`plugins/single_plugins/idle.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/idle.cpp)

#### `idle.cube_max_zoom`

Sets the maximum zoom level.
The default is **1.5**.

#### `idle.cube_rotate_speed`

Sets the speed rotation of the cube.
The default is **1.0**.

#### `idle.cube_zoom_speed`

Sets the zoom speed.
The speed means how long it will take from the time the screensaver starts to the time the cube reaches `idle.cube_max_zoom`.
The default is **1000**.

#### `idle.disable_on_fullscreen`

Disables idle on fullscreen.
The default is **true**.

#### `idle.dpms_timeout`

Enters power saving mode after the specified seconds of inactivity.
Setting the value to **-1** disables power saving.
The default is **-1**.

#### `idle.screensaver_timeout`

Displays the screensaver after the specified seconds of inactivity.
Setting the value to **-1** disables the screensaver.
The default is **-1**.

#### `idle.toggle`

Disables the compositor going idle with the specified activator.
Pressing again reactivates.

#### Output switcher

A plugin to change the focused output.

Defined in:

- [`metadata/oswitch.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/oswitch.xml)
- [`plugins/single_plugins/oswitch.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/oswitch.cpp)

#### `oswitch.next_output`

Moves focus to the next output with the specified activator.

#### `oswitch.next_output_with_win`

Moves focus to the next output with the focused window with the specified activator.

#### Scale

A plugin which shows all the windows on the current or on all workspaces, similar to GNOME's Overview.

Defined in:

- [`metadata/scale.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/scale.xml)
- [`plugins/scale/scale.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/scale/scale.cpp)

---

#### `scale.toggle`

Toggle scale for the windows on the current workspace.
Default is **<super> KEY_P**.

---

#### `scale.toggle_all`

Toggle scale for the windows on all workspaces.
**disabled** by default.

---

#### `scale.spacing`

The spacing between adjacent windows when scaled.
Default is **50** pixels.

---

#### `scale.duration`

The duration of the scale initial animation.
Default is **750** milliseconds.

---

#### `scale.interact`

Whether scale is started in interactive mode.
Interactive mode allows the user to actually type or click in the scaled windows, and can only be exited by using the toggle bindings.

If interactive mode is disabled:
- A view may be selected by clicking on it.
- The arrow keys can be used to change the currently selected view.
- `KEY_ENTER` will exit scale and focus the selected view.
- `KEY_ESC` will exit scale without changing focus.

Default is **false**, i.e. not interactive.

---

#### `scale.inactive_alpha`

Controls the opacity of windows that are not selected.
Default is **0.75** (75% opacity).

---

#### `scale.allow_zoom`

Controls whether views may be bigger than their regular size when scaled.
Default is **false**.

---

#### `scale.middle_click_close`

Make middle click on a window close that window in both interactive and non-interactive mode.
Default is **false**.

---

#### `scale.include_minimized`

Whether minimized windows should also be shown in scale.
Default is **false**.

---

#### `scale.minimized_alpha`

Controls the opacity of minimized windows when they are shown in scale.
Default is **0.45**.

---

#### `scale.close_on_new_view`

Automatically closes scale mode if a new window is opened.
Default is **false**.

---

#### `scale.outer_margin`

The margin between scaled windows and the screen edges.
Default is **0** pixels.

---

#### `scale.title_overlay`

Controls whether and how window titles are displayed as overlays in scale.
Possible values:
- `never`: never show titles.
- `mouse`: only show the title for the window under the pointer.
- `all`: show titles for all windows.

Default is **all**.

---

#### `scale.title_font_size`

Font size (in pixels) for the title overlays.
Default is **16**.

---

#### `scale.title_position`

Controls the position of the window title overlay.
Possible values:
- `top`
- `center`
- `bottom`

Default is **center**.

---

#### `scale.bg_color`

Background color of the window title overlay text, given as RGBA values.
Default is **0.1 0.1 0.1 0.9**.

---

#### `scale.text_color`

Text color of the window title overlay, given as RGBA values.
Default is **0.8 0.8 0.8 1.0**.

---


#### Window rules

A plugin to apply specific commands to windows by using various criteria.

Defined in:

- [`metadata/window-rules.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/window-rules.xml)
- [`plugins/window-rules/window-rules.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/window-rules/window-rules.cpp)

Rules syntax:

```
rule_name = on <event> if <criteria> then <if_command> [else <else_command>]
rule_name = on <event> then <command>
```

See [`criteria`] for details on the available conditions.

Note 1: `if <criteria>` can be omitted but special care must be taken, because the rules apply even for panel and background windows.
Note 2: `rule_name` can take any value that is acceptable for a key of the ini format. It would be beneficial to have names that describe the action, like `make_term_sticky` or `<app>_maximize`, etc.

Available events:

- `created`: A new window has been opened.
- `(un)maximized`: A window has been maximized/unmaximized.
- `minimized`: A window has been minimized.
- `fullscreened`: A window has been fullscreened.

Available commands:

- `set alpha <alpha>`: Set window opacity in range `[0, 1]`.
- `set geometry X Y W H`: Set window geometry.
- `set always_on_top`: Sets window to be always-on-top.
- `(un)maximize`: Maximize or unmaximize the window.
- `(un)minimize`: Minimize or restore the window.
- `move X Y`: Move the window to `(X,Y)`.
- `resize W H`: Resize the window to size `(W, H)`.
- `snap <slot>`: Snap the view to a particular slot using the grid plugin. Available slots: `top_left`, `top`, `top_right`, `right`, `bottom_right`, `bottom`, `bottom_left`, `left`, `center`(same as maximize).
- `assign_workspace X Y`: Move the view to workspace with coordinates `(X,Y)`. The top-left workspace has coordinate (0,0).
- `sticky`: sticky across all workspaces.
- `start_on_output`: start window on specific output. example: `start_on_output "HDMI-A-1"`

**Example** – Maximize each new alacritty window and set its opacity to 50%:

``` ini
[window-rules]
rule_1 = on created if app_id is "Alacritty" then maximize
rule_2 = on created if app_id is "Alacritty" then set alpha 0.5
```

#### Viewport switcher

A plugin to switch workspaces in a grid.

![switch-to-workspace](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/switch-to-workspace/2020-02-18_17:59:00.gif)

Defined in:

- [`metadata/vswitch.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/vswitch.xml)
- [`plugins/single_plugins/vswitch.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/vswitch.cpp)

#### `vswitch.binding_down`

Switches to workspace down with the specified activator.

#### `vswitch.binding_left`

Switches to workspace left with the specified activator.

#### `vswitch.binding_right`

Switches to workspace right with the specified activator.

#### `vswitch.binding_up`

Switches to workspace up with the specified activator.

#### `vswitch.binding_last`

Switches to last active workspace with the specified activator.

#### `vswitch.with_win_down`

Switches to workspace down with the focused window with the specified activator. For Wayfire version 0.7.X and earlier, this was called `vswitch.binding_win_down`

#### `vswitch.with_win_left`

Switches to workspace left with the focused window with the specified activator. For Wayfire version 0.7.X and earlier, this was called `vswitch.binding_win_left`

#### `vswitch.with_win_right`

Switches to workspace right with the focused window with the specified activator. For Wayfire version 0.7.X and earlier, this was called `vswitch.binding_win_right`

#### `vswitch.with_win_up`

Switches to workspace up with the focused window with the specified activator. For Wayfire version 0.7.X and earlier, this was called `vswitch.binding_win_up`

#### `vswitch.send_win_down`

Send the focused window to the workspace below. Since Wayfire 0.8.0.

#### `vswitch.send_win_left`

Send the focused window to the workspace on the left. Since Wayfire 0.8.0.

#### `vswitch.send_win_right`

Send the focused window to the workspace on the right. Since Wayfire 0.8.0.

#### `vswitch.send_win_up`

Send the focused window to the workspace above. Since Wayfire 0.8.0.

#### `vswitch.binding_<N>`

Sets the binding to go to workspace N. Since Wayfire 0.8.0.

#### `vswitch.with_win_<N>`

Sets the binding to go to workspace N with currently focused window. Since Wayfire 0.8.0.

#### `vswitch.send_win_<N>`

Sets the binding to move focused window to workspace N. Since Wayfire 0.8.0.

#### `vswitch.duration`

Sets the duration of the workspace switching animation in milliseconds.
The default is **300**.

#### `vswitch.gap`

Sets the gap between workspaces when animating the transition between them.
The default is **20**.

#### `vswitch.background`

Sets the background color of gaps.
The default is **0.1 0.1 0.1 1.0**.

#### `vswitch.wraparound`

Whether to wrap around when switching workspaces and there is no workspace in the desired direction.
Default is **false**.

#### Viewport swipe

A plugin to swipe workspaces in a grid.

Defined in:

- [`metadata/vswipe.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/vswipe.xml)
- [`plugins/single_plugins/vswipe.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/vswipe.cpp)

#### `vswipe.background`

Sets the background color.
The default is `0.1 0.1 0.1 1.0`.

#### `vswipe.delta_threshold`

Sets the delta threshold.
The default is **24.0**.

#### `vswipe.duration`

Sets the duration of the animation in milliseconds.
The default is **180**.

#### `vswipe.enable_horizontal`

Enables or disables horizontal swiping.
The default is **true**.

#### `vswipe.enable_smooth_transition`

Enables or disables smooth transition.
The default is **false**.

#### `vswipe.enable_vertical`

Enables or disables vertical swiping.
The default is **true**.

#### `vswipe.fingers`

Specifies the number of fingers to swipe.
The default is **4**.

#### `vswipe.gap`

Changes the gap in pixels.
The default is **32.0**.

#### `vswipe.speed_cap`

Sets the speed cap.
The default is **0.05**.

#### `vswipe.speed_factor`

Sets the speed factor.
The default is **256.0**.

#### `vswipe.threshold`

Sets the threshold.
The default is **0.35**.

#### Workspace sets

A plugin which provides multiple workspace grids which can be moved between outputs. See the [demo on Youtube](https://youtu.be/QhGqlLK8Elo).

#### `wsets.wset_<N>`

An activator binding to switch to workspace set `N`.
Disabled by default.

#### `wsets.send_to_wset_<N>`

An activator binding to send the currently focused window to workspace set `N`.
Disabled by default.

#### `wsets.label_duration`

The duration for which the notification showing the current workspace name should be shown after using a `wset_<N>` binding.
Default is **2000** ms.

### Effects

#### Animate

A plugin which provides animations when a window is opened or closed.

![open-close-zoom](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/open-close-zoom/2020-02-18_16:42:23.gif)

![open-close-fade](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/open-close-fade/2020-02-18_16:40:32.gif)

![open-close-fire](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/open-close-fire/2020-02-18_16:34:01.gif)

Defined in:

- [`metadata/animate.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/animate.xml)
- [`plugins/animate/animate.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/animate/animate.cpp)
- [`plugins/animate/fire/fire.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/animate/fire/fire.cpp)

#### `animate.close_animation`

Specifies the type of animation when closing a window.

Possible values are:

- `none`
- `fade`
- `zoom`
- `fire`

The default is `zoom`.

#### `animate.duration`

Sets the duration of the animation in milliseconds.
The default is **500**.

#### `animate.enabled_for`

Specifies the window types to be animated.
The default is `(type is "toplevel" | (type is "x-or" & focusable is true))`.

See [`criteria`] for details.

#### `animate.fade_duration`

Sets the duration for the _fade_ animation in milliseconds.
Only applies for windows matched by `animate.fade_enabled_for`.
The default is **400**.

#### `animate.fade_enabled_for`

Specifies the window types to be animated with a fade effect.
The default is `type is "overlay"`.

See [`criteria`] for details.

#### `animate.fire_duration`

Sets the duration for the _fire_ animation in milliseconds.
Only applies for windows matched by `animate.fire_enabled_for`.
The default is **300**.

#### `animate.fire_enabled_for`

Specifies the window types to be animated with a fire effect.
The default is `none`.

See [`criteria`] for details.

#### `animate.fire_particles`

Sets the number of fire particles.
The default is **2000**.

#### `animate.fire_particle_size`

Sets the size of the fire particles in pixels.
The default is **16.0**.

#### `animate.fire_color`

Sets the color of the fire effects, alpha is ignored.
The default is `0.7 0.14 0.01 1.0`.

#### `animate.random_fire_color`

Sets whether to use one solid color or select a random color for each particle.
The default is `false`.

#### `animate.open_animation`

Specifies the type of animation when opening a window.

Possible values are:

- `none`
- `fade`
- `zoom`
- `fire`

The default is `zoom`.

#### `animate.startup_duration`

Sets the duration of fading (in milliseconds) when Wayfire starts.
The default is **600**.

#### `animate.zoom_duration`

Sets the duration for the _zoom_ animation in milliseconds.
Only applies for windows matched by `animate.zoom_enabled_for`.
The default is **500**.

#### `animate.zoom_enabled_for`

Specifies the window types to be animated with a zoom effect.
The default is `none`.

See [`criteria`] for details.

#### Blur

A plugin to blur windows.

Supported methods:

- [Bokeh](https://en.wikipedia.org/wiki/Bokeh)
- [Box](https://en.wikipedia.org/wiki/Box_blur)
- [Gaussian](https://en.wikipedia.org/wiki/Gaussian_blur)
- Kawase

Each method has its own set of options: `offset`, `degrade` and `iterations`.
Different combinations result in various levels of blur strength, performance and visual appearance.

Defined in:

- [`metadata/blur.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/blur.xml)
- [`plugins/blur/blur-base.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/blur/blur-base.cpp)
- [`plugins/blur/blur.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/blur/blur.cpp)
- [`plugins/blur/bokeh.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/blur/bokeh.cpp)
- [`plugins/blur/box.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/blur/box.cpp)
- [`plugins/blur/gaussian.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/blur/gaussian.cpp)
- [`plugins/blur/kawase.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/blur/kawase.cpp)

#### `blur.bokeh_degrade`

Sets the degrade value for the bokeh method.
The value must be between **1** and **5**.
The default is **1**.

#### `blur.bokeh_iterations`

Sets the iterations for the bokeh method.
The value must be between **0** and **250**.
The default is **15**.

#### `blur.bokeh_offset`

Sets the offset value for the bokeh method.
The value must be between **0** and **50**.
The default is **5**.

#### `blur.box_degrade`

Sets the degrade value for the box method.
The value must be between **1** and **5**.
The default is **1**.

#### `blur.box_iterations`

Sets the iterations for the box method.
The value must be between **0** and **25**.
The default is **2**.

#### `blur.box_offset`

Sets the offset value for the box method.
The value must be between **0** and **25**.
The default is **2**.

#### `blur.gaussian_degrade`

Sets the degrade value for the gaussian method.
The value must be between **1** and **5**.
The default is **1**.

#### `blur.gaussian_iterations`

Sets the iterations for the gaussian method.
The value must be between **0** and **25**.
The default is **2**.

#### `blur.gaussian_offset`

Sets the offset value for the gaussian method.
The value must be between **0** and **25**.
The default is **2**.

#### `blur.kawase_degrade`

Sets the degrade value for the kawase method.
The value must be between **1** and **5**.
The default is **1**.

#### `blur.kawase_iterations`

Sets the iterations for the kawase method.
The value must be between **0** and **10**.
The default is **2**.

#### `blur.kawase_offset`

Sets the offset value for the kawase method.
The value must be between **0** and **25**.
The default is **5**.

#### `blur.method`

Chooses a blur algorithm.  The variants are as follows:

- `bokeh`
- `box`
- `gaussian`
- `kawase`

The default is `kawase`.

#### `blur.mode`

Specifies whether blurring should be automatic or manual.
Possible values are `normal` and `toggle`.
`normal` means automatic and `toggle` manual.
The default is `normal`.

#### `blur.toggle`

Sets the shortcut to toggle blurring for a specific window.

#### Decoration

Default decorations around Wayland and [XWayland] windows.

Defined in:

- [`metadata/decoration.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/decoration.xml)
- [`plugins/decor/deco-layout.hpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/decor/deco-layout.hpp)
- [`plugins/decor/deco-theme.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/decor/deco-theme.cpp)
- [`plugins/decor/deco-theme.hpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/decor/deco-theme.hpp)

[XWayland]: https://wayland.freedesktop.org/xserver.html

#### `decoration.active_color`

Sets the color when the window is active.
The default is `0.6 0.6 0.6 1.0`.

For its counterpart, see `decoration.inactive_color`.

#### `decoration.border_size`

Sets the border size in pixels.
The default is **4**.

#### `decoration.button_order`

Sets the order of the window buttons.
The default is `minimize maximize close`.

#### `decoration.font`

Sets the font to use for the title bars, via a Pango font description string (e.g. `Gentium Plus 12`).
The default is `sans-serif`.

#### `decoration.inactive_color`

Sets the color when the window is inactive.
The default is `0.3 0.3 0.3 1.0`.

For its counterpart, see `decoration.active_color`.

#### `decoration.title_height`

Sets the height of the title bars in pixels.
The default is **30**.

#### `decoration.ignore_views`

Disables decoration for windows matching the specified criteria.
The default is `none`.

See [`criteria`] for details.

#### Fisheye

A plugin which provides fisheye effect.

Defined in:

- [`metadata/fisheye.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/fisheye.xml)
- [`plugins/single_plugins/fisheye.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/fisheye.cpp)

#### `fisheye.radius`

Sets the border radius in pixels.
The default is **450.0**.

#### `fisheye.toggle`

Toggles fisheye with the specified key.

#### `fisheye.zoom`

Sets the zoom factor.
The default is **7.0**.

#### Wobbly

A plugin to get wobbly windows.

Defined in:

- [`metadata/wobbly.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/wobbly.xml)
- [`plugins/wobbly/wobbly.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/wobbly/wobbly.cpp)

#### `wobbly.friction`

Sets the friction value.
The default is **3.0**.

#### `wobbly.spring_k`

Sets the spring constant.
The default is **8.0**.

#### `wobbly.grid_resolution`

Sets the grid resolution.
The default is **6**.

#### Window rotate

A plugin to rotate windows with the mouse.
Move the mouse cursor to the center of the window to reset the rotation.

![rotate-window](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/rotate-window/2020-02-18_16:44:54.gif)

Defined in:

- [`metadata/wrot.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/wrot.xml)
- [`plugins/single_plugins/wrot.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/wrot.cpp)

#### `wrot.activate`

Rotates the window with the specified button.

### General

#### Autostart

A plugin to run shell commands on startup.

Defined in:

- [`metadata/autostart.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/autostart.xml)
- [`plugins/single_plugins/autostart.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/autostart.cpp)

#### `autostart.<program-id> <shell-command>`

Executes shell command with `sh` on startup.
The program ID does not matter, but must be different for distinct commands.

#### `autostart.autostart_wf_shell`

Executes wf-panel and wf-background if they are not already in the autostart command list.
The default is true which means that when no configuration file is preset, wayfire will try to run wf-shell.

**Example**

`~/.config/wayfire.ini`

``` ini
[autostart]
background = wf-background
panel = wf-panel
dock = wf-dock
```

#### Command

A plugin to bind key combo (key, button, touch) to execute shell commands.

**Example** – Start a terminal:

`~/.config/wayfire.ini`

``` ini
[command]
binding_terminal = <super> KEY_ENTER
command_terminal = alacritty
```

**Example** – Volume controls:

`~/.config/wayfire.ini`

``` ini
[command]
repeatable_binding_volume_up = KEY_VOLUMEUP
command_volume_up = amixer set Master 5%+
repeatable_binding_volume_down = KEY_VOLUMEDOWN
command_volume_down = amixer set Master 5%-
```

See [`key`] for more information on the syntax.

Defined in:

- [`metadata/command.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/command.xml)
- [`plugins/single_plugins/command.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/command.cpp)

#### `command.binding_<id> <key>`

Associates `binding_<id>` to `command_<id>` when pressing the specified key.

See also `command.repeatable_binding_<id>`.

#### `command.command_<id> <shell-command>`

Executes shell command with `sh` on associated `binding_<id>`.

#### `command.repeatable_binding_<id> <key>`

Same as `command.binding_<id>`, but command is repeatable by holding the key.

#### Core

General options.

Defined in:

- [`metadata/core.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/core.xml)
- [`src/core/core.cpp`](https://github.com/WayfireWM/wayfire/blob/master/src/core/core.cpp)
- [`src/core/wm.cpp`](https://github.com/WayfireWM/wayfire/blob/master/src/core/wm.cpp)
- [`src/output/plugin-loader.cpp`](https://github.com/WayfireWM/wayfire/blob/master/src/output/plugin-loader.cpp)
- [`src/output/render-manager.cpp`](https://github.com/WayfireWM/wayfire/blob/master/src/output/render-manager.cpp)
- [`src/output/workspace-impl.cpp`](https://github.com/WayfireWM/wayfire/blob/master/src/output/workspace-impl.cpp)
- [`src/output/workspace-impl.cpp`](https://github.com/WayfireWM/wayfire/blob/master/src/output/workspace-impl.cpp)

#### `core.background_color`

Sets the background color of workspaces.
Visible when nothing is drawing the background.
The default is `0.1 0.1 0.1 1.0`.

#### `core.close_top_view`

Closes the currently focused window with the specified key.

#### `core.plugins`

Loads the specified plugins, space-separated list.

#### `core.preferred_decoration_mode`

Sets the preferred [window decoration] mode.
Possible values are `client` and `server`.
`client` allows the client to draw its own decorations.
The default is `client`.

[Window decoration]: https://en.wikipedia.org/wiki/Window_decoration

#### `core.vheight`

Sets the number of vertical workspaces.
Currently, cannot be changed at runtime.
The default is **3**.

#### `core.vwidth`

Sets the number of horizontal workspaces.
Currently, cannot be changed at runtime.
The default is **3**.

#### `core.xwayland`

Enables or disables [XWayland] support, which allows X11 applications to be used.
The default is **true**.

[XWayland]: https://wayland.freedesktop.org/xserver.html

#### `core.xwayland_startup_script`

Specifies the path to an executable script or binary, which is executed at the end of the XWayland startup process. A suggested example script for HiDPI with a scale factor of 2, and support for importing .Xresources from your home directory:

```
#!/bin/sh

if [ -r "${HOME}/.Xresources" ] && command -v xrdb >/dev/null 2>&1; then
	xrdb -merge "${HOME}/.Xresources"
fi

xprop -root -f _XWAYLAND_GLOBAL_OUTPUT_SCALE 32c -set _XWAYLAND_GLOBAL_OUTPUT_SCALE 2
```

#### `core.max_render_time`

Controls a latency optimization. Lower values mean that Wayfire will reserve less time for redrawing an output, which leaves more time for clients to update themselves. Values that are too low however will lead to Wayfire missing a frame and lowering the refresh rate.
Set to `-1` to disable.

#### `core.focus_buttons`

A list of buttons which will focus the view currently under the cursor and bring it to the front. Default is **`BTN_LEFT | BTN_RIGHT | BTN_MIDDLE`**.


#### `core.focus_button_with_modifiers`

If set to `true`, the focus action described by `core.focus_buttons` will work even when modifiers are pressed. Default is **`false`**.

#### `core.focus_buttons_passthrough`

Whether to send the actual button event to the client under the cursor for clicks handled by `core.focus_buttons`. Default is **`true`**.

#### Input

On a typical setup, the input configuration is managed in the config section `[input]`.

The options in `[input]` are defined in [`metadata/input.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/input.xml).

On some setups, this simple configuration (where keys apply to all input devices) is not expressive enough, and wayfire can be configured using `[input:<device_name>]` and `[input-device:<device_name>]` configurations, see [Input device-specific options](#input-device-specific-options) for more info.

##### Keyboard configuration

Defined in:

- [`src/core/seat/keyboard.cpp`](https://github.com/WayfireWM/wayfire/blob/master/src/core/seat/keyboard.cpp)

#### `input.kb_repeat_delay`

Sets the amount of time a key must be held before it starts repeating.
The default is **400**.

#### `input.kb_repeat_rate`

Sets the frequency of key repeats once the `input.kb_repeat_delay` has passed.
The default is **40**.

#### `input.modifier_binding_timeout`

Cancels modifier actions (like ++super++ for the [`expo`](#expo) plugin) when held for the specified timeout (in milliseconds).
Setting the value to **0** never cancels.
The default is **400**.

#### `input.kb_numlock_default_state`

Default numlock state when Wayfire starts.
The default is **false**.

#### `input.kb_capslock_default_state`

Default capslock state when Wayfire starts.
The default is **false**.

#### `input.xkb_layout`

Sets the layout of the keyboard, like `us` or `fr`.
Multiple layouts can be specified by separating them with commas.
The default is `us`.

#### `input.xkb_model`

Sets the model of the keyboard.
This has an influence for some extra keys your keyboard might have.

#### `input.xkb_options`

Sets extra XKB configuration options for the keyboard, such as keyboard shortcuts to [switch layouts](https://man.archlinux.org/man/xkeyboard-config.7#Switching_to_another_layout).
Multiple options can be specified by separating them with commas.

#### `input.xkb_rules`

Sets files of rules to be used for keyboard mapping composition.
The default is `evdev`.

#### `input.xkb_variant`

Sets the variant of the keyboard, like `dvorak` or `colemak`.

##### libinput configuration

See [libinput documentation] for details.

[libinput documentation]: https://wayland.freedesktop.org/libinput/doc/latest/

Defined in:

- [`src/core/seat/pointing-device.cpp`](https://github.com/WayfireWM/wayfire/blob/master/src/core/seat/pointing-device.cpp)

#### `input.left_handed_mode`

Enables or disables left-handed mode.
The default is **false**.

See [Left-handed Mode] for details.

[Left-handed Mode]: https://wayland.freedesktop.org/libinput/doc/latest/configuration.html#left-handed-mode

#### `input.middle_emulation`

Enables or disables middle button emulation. Middle button emulation converts a simultaneous left and right button click into a middle button event. Enabling this option effectively disables the hardware middle button.
The default is **false**.

See [Middle Button Emulation] for details.

[Middle Button Emulation]: https://wayland.freedesktop.org/libinput/doc/latest/configuration.html#middle-button-emulation

#### `input.mouse_accel_profile`

Sets the pointer acceleration profile.

Possible values are:

- `default`
- `none`
- `adaptive`
- `flat`

The default is `default`.

See [Pointer acceleration] for details.

[Pointer acceleration]: https://wayland.freedesktop.org/libinput/doc/latest/pointer-acceleration.html

#### `input.touchpad_accel_profile`

Sets the touchpad acceleration profile.

Possible values are:

- `default`
- `none`
- `adaptive`
- `flat`

The default is `default`.

See [Pointer acceleration] for details.

[Pointer acceleration]: https://wayland.freedesktop.org/libinput/doc/latest/pointer-acceleration.html

#### `input.tap_to_click`

Enables or disables tap-to-click.
The default is **true**.

See [Tap-to-click behaviour] for details.

[Tap-to-click behaviour]: https://wayland.freedesktop.org/libinput/doc/latest/tapping.html

#### `input.drag_lock`

Enables or disables drag lock.
The default is **false**.

See [Tap-and-drag] for details.

[Tap-and-drag]: https://wayland.freedesktop.org/libinput/doc/latest/tapping.html#tap-and-drag

#### `input.click_method`

Changes the click method.

Possible values are:

- `default`
- `none`
- `button-areas`
- `clickfinger`

The default is `default`.

See [Clickpad software button behavior] for details.

[Clickpad software button behavior]: https://wayland.freedesktop.org/libinput/doc/latest/clickpad-softbuttons.html

#### `input.scroll_method`

Changes the scroll method.

Possible values are:

- `default`
- `none`
- `two-finger`
- `edge`
- `on-button-down`

The default is `default`.

See [Scrolling] for details.

[Scrolling]: https://wayland.freedesktop.org/libinput/doc/latest/scrolling.html

#### `input.disable_touchpad_while_typing`

Disables the touchpad while typing.
The default is **false**.

#### `input.disable_touchpad_while_mouse`

Disables the touchpad while using the mouse.
The default is **false**.

#### `input.natural_scroll`

Enables or disables natural (inverted) scrolling.
The default is **false**.

#### `input.mouse_cursor_speed`

Changes the pointer acceleration.
The value must be between **-1.0** and **1.00**.
The default is **0.0**.

#### `input.touchpad_cursor_speed`

Changes the touchpad acceleration.
The value must be between **-1.0** and **1.00**.
The default is **0.0**.

#### `input.mouse_scroll_speed`

Changes the mouse scroll factor.
Scroll speed will be scaled by the given value, which must be non-negative.
The default is **1.0**.

#### `input.touchpad_scroll_speed`

Changes the touchpad scroll factor.
Scroll speed will be scaled by the given value, which must be non-negative.
The default is **1.0**.

#### `input.tablet_motion_mode`

Changes the tablet (example, by Wacom) motion mode.

Possible values are:

- `absolute`
- `relative`

The default is **absolute**.

##### Cursor configuration

Defined in:

- [`src/core/seat/cursor.cpp`](https://github.com/WayfireWM/wayfire/blob/master/src/core/seat/cursor.cpp)

#### `input.cursor_theme`

Overrides the system default `XCursor` theme.
The default is `default`.

#### `input.cursor_size`

Overrides the system default `XCursor` size.
The default is **24**.

##### Input device-specific options

The `[input]` section contains input configuration options which apply to all devices.
If needed, the options can be configured for each device separately by using the `[input:<device_name>]` sections.
These per-device sections support the exact same options as the generic `[input]` section, but apply only to the specific device.

> [!IMPORTANT]
> If a device-specific section exists, it is used exclusively instead of the common section;
> do not rely on a fallback to the common section for options not specified in the device-specific section,
> but explicitly set all the options on the given device`[input:<device_name>]` section.

The device name can most easily be found by looking at Wayfire's log.
By default, you will see lines `handle new input: <device_name>`.
You can also run Wayfire with `-d input-devices` to see more information about which config section names can be used for which devices,
as the device name can also be a UDEV identifier.

The following example configures a keyboard to use German layout (while the other keyboard-like devices would use others); this use case can arise when using a bar-code reader which would only support English layout.

```ini
[input:platform-i8042-serio-0] # or [input:AT Translated Set 2 keyboard]
xkb_layout = de
```

Last but not least, there are also `input-device:<device_name>` sections. These include additional options that can be set only **per-device**.

`[input-device:<device-name>]` is defined in [`metadata/input-device.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/input-device.xml).


#### `input-device.output`

You can map a specific input device to a specific output (useful for touchscreens).

```ini
[input-device:Wacom Pen and multitouch sensor Finger]
output = eDP-1
```

Replace `Wacom Pen and multitouch sensor Finger` with the name of your device and `eDP-1` with the name of the output you want to bind the input device to.

#### `input-device.calibration`

Can be used to set a 2x3 calibration matrix for touchscreen devices.

#### Output

Output configuration.

**Note**: You can get the names of your outputs with [wlr-randr], or look at the output of Wayfire (`new output: <name>`)

[wlr-randr]: https://github.com/emersion/wlr-randr

**Example**:

`~/.config/wayfire.ini`

``` ini
[output:eDP-1]
mode = 1920x1080@60000
position = 0,0
transform = normal
scale = 1.000000
```

#### `<output>.position`

Places the specified output at the specific position in the global coordinate space.
The format is `<x>,<y>` in pixels.

#### `<output>.mode`

Configures the specified output to use the given mode.
Modes are a combination of width and height (in pixels) and a refresh rate (in hertz) that your display can be configured to use.
The format is `<width>x<height>@<rate>`, or `off`, which turns the output off.

Mode can also be `mirror <output_name>`, where `<output_name>` is the name of another output. In this case, the output will simply mirror the contents of the given output.

#### `<output>.custom_mode_N`

Can be used to create a new mode for the output, even if the hardware does not report it. Valid values are modelines, can be generated with cvt. For example, if you want to add a `1280x720` mode:

Output of `cvt 1280 720`:
```
# 1280x720 59.86 Hz (CVT 0.92M9) hsync: 44.77 kHz; pclk: 74.50 MHz
Modeline "1280x720_60.00"   74.50  1280 1344 1472 1664  720 723 728 748 -hsync +vsync
```

Option value in config file:
```
custom_mode_1 = 74.50  1280 1344 1472 1664  720 723 728 748 -hsync +vsync
```

Optionally append "interlaced" at the end of the modeline to enable interlaced scan.

After that, you can use the new mode in the `<output>.mode` option or via kanshi.

#### `<output>.scale`

Scales the specified output by the specified scale factor.

#### `<output>.transform`

Sets the background transform for the specified output. Possible values are any members of the `wl_output::transform` enum, viewable [here](https://wayland.app/protocols/wayland#wl_output:enum:transform).

#### Workarounds

Some hacks that might be required to make things work.

Defined in:

- [`metadata/workarounds.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/workarounds.xml)

#### `workarounds.app_id_mode`

Application ID mode.

Specifies the application ID mode.

Possible values are:

- `stock`
- `gtk-shell`
- `full`

The default is `stock`.

#### `workarounds.all_dialogs_modal`

Make all dialogs modal.

If false, the main window can be focused even if it has a dialog. The dialog is nevertheless kept on top of the main window. Kwin defaults to false.

The default is `true`.

#### `workarounds.dynamic_repaint_delay`

Allow dynamic repaint delay.

If true, allows Wayfire to dynamically recalculate its max_render_time, i.e allow render time higher than max_render_time.

The default is `true`.

#### `workarounds.use_external_output_configuration`

Use external output configuration instead of Wayfire's own.

If true, Wayfire will not handle any configuration options for outputs in the config file once an
external daemon like [kansi](https://github.com/emersion/kanshi) sets the output configuration via the wlr-output-management protocol.
Exceptions are made for options not available via wlr-output-management, like output mirroring and custom modelines.

The default is `false`.

#### `workarounds.remove_output_limits`

Allow views to overlap between multiple outputs. Many of the core plugins will not behave properly with this option set!

The default is `false`.

#### `workarounds.force_preferred_decoration_mode`

Force xdg-decoration clients to use the compositor-preferred decoration mode regardless of the client's preference.

The default is `false`.

#### `workarounds.enable_so_unloading`

Enable calling dlclose() when a plugin is unloaded. Note that this may not work well with all plugins.

The default is `false`.

#### `workarounds.discard_command_output`

Discard output from commands invoked by Wayfire, so that they don't end up in the logs.

The default is `true`.

#### `workarounds.enable_input_method_v2`

Enable support for the newer input-method-v2 protocol. Note that the input-method-v1 protocol works better in many cases.

The default is `false`.

#### `workarounds.enable_opaque_region_damage_optimizations`

Enable certain damage optimizations which are based on a surfaces' opaque regions. In some cases, this optimization might give unexpected results (i.e background app stops updating) even though this is fine according to Wayland's protocol.

The default is `false`.

#### `workarounds.force_frame_sync`

This option can be used to workaround driver bugs that cause rendering artifacts, though can cause more resource usage. Leave disabled if unsure.

The default is `false`.

### Utility

#### Session lock protocol

An implementation of the ext-session-lock-v1 protocol. Provides more secure screen locking.

Defined in:

- [`metadata/session-lock.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/session-lock.xml)
- [`plugins/protocols/session-lock.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/protocols/session-lock.cpp)

#### XDG Activation protocol

An implementation of the [XDG Activation protocol](https://wayland.app/protocols/xdg-activation-v1). This allows already open views to be "activated" (focused) in response to some user action, e.g. a text editor might use this to request focus when you open a document from a file manager. Enable this by adding `xdg-activation` to `core.plugins`.

Note: newly opened views are currently always focused by Wayfire; this protocol is only concerned with allowing to focus already open views.

Defined in:

- [`metadata/xdg-activation.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/xdg-activation.xml)
- [`plugins/protocols/xdg-activation.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/protocols/xdg-activation.cpp)

By default, activation requests are granted for most cases, including for "self-generated" activation tokens. This could in theory allow apps to "steal" focus in unexpected ways. The following options make the handling of activation requests more strict:

##### `xdg-activation.check_surface`

If set to true, an activation request must come from the currently focused view and in response to some user interaction. This restricts activation requests for the case when the currently focused view explicitly "passes" focus to a newly activated one (e.g. opening a file from a file manager). If set to false, any app can use this protocol to request focus, even without user interaction. Note that this is currently required for focusing an already open view from the command line (e.g. opening a file from the terminal in a text editor that has an aleady open view).

##### `xdg-activation.only_last_request`

If set to true, a view can only be activated using the last generated activation token (in response to the latest user interaction). This is to avoid race conditions when multiple tokens are generated in a short time in response to multiple user actions.

##### `xdg-activation.timeout`

Timeout (in seconds) for activation requests to expire. Default is 30s.

#### Input Method Protocols

Implementations of the [Input method v2 protocol](https://wayland.app/protocols/input-method-unstable-v2) and the [Text input protocol](https://wayland.app/protocols/text-input-unstable-v3). This allows fcitx5 to work properly with Chromium without XWayland, GTK IM Module and QT IM Module.

Note: Configure `~/.config/chromium-flags.conf` if you are using Chromium:
```bash
$ vim ~/.config/chromium-flags.conf
--enable-wayland-ime --wayland-text-input-version=3
```

Defined in:
- [`metadata/input-method-v1.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/input-method-v1.xml)
- [`plugins/protocols/input-method-v1.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/protocols/input-method-v1.cpp)
- [`plugins/protocols/text-input-v1-v3.hpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/protocols/text-input-v1-v3.hpp)


### Window management

#### Fast switcher

A plugin to switch active window.

Defined in:

- [`metadata/fast-switcher.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/fast-switcher.xml)
- [`plugins/single_plugins/fast-switcher.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/fast-switcher.cpp)

#### `fast-switcher.activate`

Activates fast switcher, switching between windows in forward order, using the specified key.

#### `fast-switcher.activate_backward`

Activates fast switcher, switching between windows in reverse order, using the specified key.

#### Grid

A plugin to position the windows in certain regions of the output.

![grid](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/grid/2020-02-18_16:21:37.gif)

Defined in:

- [`metadata/grid.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/grid.xml)
- [`plugins/single_plugins/grid.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/grid.cpp)

#### `grid.duration`

Sets the duration of the animation in milliseconds.
The default is **300**.

#### `grid.restore`

Restores the window to its original size and position with the specified activator.

#### `grid.slot_b`

Positions the window in the bottom edge of the screen with the specified activator.
The window takes half of the screen.

#### `grid.slot_bl`

Positions the window in the bottom left corner of the screen with the specified activator.
The window takes 1/4 of the screen.

#### `grid.slot_br`

Positions the window in the bottom right corner of the screen with the specified activator.
The window takes 1/4 of the screen.

#### `grid.slot_c`

Positions the window in the center of the screen with the specified activator.
The window is maximized.

#### `grid.slot_l`

Positions the window in the left edge of the screen with the specified activator.
The window takes half of the screen.

#### `grid.slot_r`

Positions the window in the right edge of the screen with the specified activator.
The window takes half of the screen.

#### `grid.slot_t`

Positions the window in the top edge of the screen with the specified activator.
The window takes half of the screen.

#### `grid.slot_tl`

Positions the window in the top left corner of the screen with the specified activator.
The window takes 1/4 of the screen.

#### `grid.slot_tr`

Positions the window in the top right corner of the screen with the specified activator.
The window takes 1/4 of the screen.

#### `grid.type`

Sets the type of the animation.

Possible values are:

- `none`
- `crossfade`
- `wobbly`

The default is `crossfade`.

#### Move

A plugin to move windows around by dragging them from any part (not just the title bar).

![drag-window](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/drag-window/2020-02-18_16:49:54.gif)

Defined in:

- [`metadata/move.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/move.xml)
- [`plugins/single_plugins/move.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/move.cpp)
- [`plugins/single_plugins/move-snap-helper.hpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/move-snap-helper.hpp)

#### `move.activate`

When the specified button is held down, you can drag windows to move them.

#### `move.enable_snap`

Enables or disables snapping the window being moved to an edge of the screen.
The default is **true**. Requires the `grid` plugin to also be enabled.

#### `move.enable_snap_off`

Enables or disables snapping off the window being grabbed from an edge of the screen.
The default is **true**.

#### `move.join_views`

Disallows independently moving dialogues.
The default is **false**.

#### `move.snap_off_threshold`

When attempting to move a snapped window, this option requires the user to move at least the specified amount of pixels before the window actually moves.
This only takes effect with `move.enable_snap_off`.
The default is **10**.

#### `move.snap_threshold`

Sets the amount of pixels from the edge of the screen for which aero-snap gets triggered.
The default is **10**.

#### Place

A plugin to position newly opened windows.

Defined in:

- [`metadata/place.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/place.xml)
- [`plugins/single_plugins/place.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/place.cpp)

#### `place.mode`

Specifies how to position newly opened windows.

Possible values are:

- `center`
- `cascade`
- `random`

The default is `center`.

#### Preserve output

Some monitors and/or GPUs disconnect the outputs during DPMS, which can result in all of your windows ending up on one output after waking them from sleep. This plugin prevents that from happening.

#### `preserve-output.last_output_focus_timeout`

When an output is destroyed, if it is focused then it is remembered as the focused output, unless another output was already remembered as the focused output in the last X milliseconds. This option sets that number of milliseconds.

The default is **10000** (10 seconds).

#### Simple tile

A plugin which provides some tiling features, inspired by [Sway].
The plugin is meant to contain only the basics.

[Sway]: https://swaywm.org

Defined in:

- [`metadata/simple-tile.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/simple-tile.xml)
- [`plugins/tile/tile-plugin.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/tile/tile-plugin.cpp)

#### `simple-tile.button_move`

When the specified button is held down, you can drag and drop tiled windows to reorder them and to change the tiling direction.

#### `simple-tile.button_resize`

When the specified button is held down, you can drag tiled windows to resize them.

#### `simple-tile.keep_fullscreen_on_adjacent`

Specifies whether to keep fullscreen state when changing the focus.
If **true**, the next focused window will also get fullscreen.
If **false**, leaves fullscreen.
The default is **true**.

#### `simple-tile.key_focus_above`

Moves focus to the window above with the specified key.

#### `simple-tile.key_focus_below`

Moves focus to the window below with the specified key.

#### `simple-tile.key_focus_left`

Moves focus to the window left with the specified key.

#### `simple-tile.key_focus_right`

Moves focus to the window right with the specified key.

#### `simple-tile.key_toggle`

Toggles tiling mode with the specified key.

#### `simple-tile.tile_by_default`

Enables tiling for windows matching the specified criteria.
The default is `all`.

See [`criteria`] for details.

Example: Excluding a Specific Window from Tiling
To exclude a window from tiling based on its title, you can use the following syntax:

```
[simple-tile]
tile_by_default = !(title is "ExactWindowTitle")
```

or, to exclude any window whose title contains a specific phrase:

```
[simple-tile]
tile_by_default = !(title contains "PartialTitle")
```

For example, if you want to prevent tiling for any window with the title "Floating Notes", you would use:
```
[simple-tile]
tile_by_default = !(title is "Floating Notes")
```

This will ensure that any window with the exact title "Floating Notes" remains floating while all others are tiled.

#### `simple-tile.inner_gap_size`

Controls the gap size (free space) between adjacent tiled views. Default is 5 pixels.

#### `simple-tile.outer_horiz_gap_size`

Controls the gap size on the left and the right edge of the workspace. Default is 0 pixels.

#### `simple-tile.outer_vert_gap_size`

Controls the gap size on the top and the bottom edge of the workspace. Default is 0 pixels.

#### Switcher

A plugin to change active window with an animation.

![switch-between-windows](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/switch-between-windows/2020-02-18_17:24:41.gif)

Defined in:

- [`metadata/switcher.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/switcher.xml)
- [`plugins/single_plugins/switcher.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/switcher.cpp)
- [`wf-config/src/types.cpp`](https://github.com/WayfireWM/wf-config/blob/master/src/types.cpp)

#### `switcher.gesture_toggle`

Toggles the switcher with the specified gesture.
The default is `edge-swipe down 3`.

#### `switcher.next_view`

Switches to the next view with the specified key.

#### `switcher.prev_view`

Switches to the previous view with the specified key.

#### `switcher.speed`

Sets the duration of the animation in milliseconds.
The default is **500**.

#### `switcher.touch_sensitivity`

Sets the touch sensitivity.
The default is **1.0**.

#### `switcher.view_thumbnail_scale`

Sets the thumbnail size.
The default is **1.0**.

#### Resize

A plugin to resize windows by dragging them from any part (not just the edges).

![resize-window](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/resize-window/2020-02-18_17:44:25.gif)

Defined in:

- [`metadata/resize.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/resize.xml)
- [`plugins/single_plugins/resize.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/single_plugins/resize.cpp)

#### `resize.activate`

When the specified button is held down, you can drag windows to resize them.

#### WM-Actions

A plugin providing various window management functions.
- [`metadata/wm-actions.xml`](https://github.com/WayfireWM/wayfire/blob/master/metadata/wm-actions.xml)
- [`plugins/wm-actions.cpp`](https://github.com/WayfireWM/wayfire/blob/master/plugins/wm-actions/wm-actions.cpp)

#### `wm-actions.toggle_fullscreen`

Makes focused view fullscreen with the specified key.

#### `wm-actions.toggle_always_on_top`

Toggles the always-on-top state of the focused view (as a keybinding) or the view clicked on (as a button binding).

#### `wm-actions.toggle_sticky`

Toggle the sticky state of a view.

#### `wm-actions.toggle_maximize`

Toggle the maximize state of the active view.

#### `wm-actions.minimize`

Minimize the active view.

#### `wm-actions.toggle_showdesktop`

Show the desktop.

#### `wm-actions.send_to_back`

Send the active view to behind all other views.
