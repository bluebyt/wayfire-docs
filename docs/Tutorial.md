# Tutorial

**Table of contents**

- [Default key-bindings](#default-key-bindings)
- [Using Wayfire](#using-wayfire)
  - [Opening terminals and moving around](#opening-terminals-and-moving-around)
  - [Opening other applications](#opening-other-applications)
  - [Closing windows](#closing-windows)
  - [Moving windows](#moving-windows)
  - [Resizing windows](#resizing-windows)
  - [Switching between windows](#switching-between-windows)
  - [Positioning windows](#positioning-windows)
  - [Using workspaces](#using-workspaces)
  - [Moving windows to workspaces](#moving-windows-to-workspaces)
  - [Changing the opacity](#changing-the-opacity)
  - [Reloading the configuration](#reloading-the-configuration)
  - [Exiting Wayfire](#exiting-wayfire)
- [Configuring Wayfire](#configuring-wayfire)
  - [Setting your keyboard layout](#setting-your-keyboard-layout)
  - [Terminal](#terminal)
  - [Application launcher](#application-launcher)
  - [Setting the wallpaper](#setting-the-wallpaper)
  - [Adding a panel and dock](#adding-a-panel-and-dock)
  - [Configuring viewport switcher](#configuring-viewport-switcher)
  - [Configuring your outputs](#configuring-your-outputs)
  - [Notifications](#notifications)
  - [Idle configuration](#idle-configuration)
  - [Screen locker](#screen-locker)
  - [Logout](#logout)
  - [Changing the volume](#changing-the-volume)
  - [Changing the screen brightness](#changing-the-screen-brightness)
  - [Screen color temperature](#screen-color-temperature)
  - [Taking screenshots](#taking-screenshots)
  - [Creating screencasts](#creating-screencasts)
  - [Automatically maximizing windows](#automatically-maximizing-windows)
  - [Remote desktop](#remote-desktop)

## Default key-bindings

Here is an overview (non-exhaustive) of the key-bindings you can find in the [`wayfire.ini`] config file. A comprehensive reference of bindings available by default is available [Bindings available by default](Bindings-available-by-default.md).

[`wayfire.ini`]: https://github.com/WayfireWM/wayfire/blob/master/wayfire.ini

- <kbd>Super</kbd> + <kbd>e</kbd> ⇒ Overview all workspaces.
  - [<kbd>h</kbd><kbd>j</kbd><kbd>k</kbd><kbd>l</kbd><kbd>Enter</kbd><kbd>Home</kbd><kbd>End</kbd>] ⇒ Switch to the specified workspace; add <kbd>Shift</kbd> for bottom.
- <kbd>Super</kbd> + <kbd>Enter</kbd> ⇒ Open a terminal ([Alacritty]).
- <kbd>Super</kbd> + <kbd>Shift</kbd> + <kbd>Enter</kbd> ⇒ Open the launcher ([Wofi]).
- <kbd>Super</kbd> + <kbd>w</kbd> ⇒ Close window.
- <kbd>Super</kbd> + <kbd>Tab</kbd> ⇒ Preview windows and select the next window; add <kbd>Shift</kbd> for previous.
- <kbd>Alt</kbd> + <kbd>Escape</kbd> ⇒ Select the next window.
- <kbd>Super</kbd> + [<kbd>Up</kbd><kbd>Down</kbd><kbd>Left</kbd><kbd>Right</kbd><kbd>Home</kbd><kbd>End</kbd><kbd>f</kbd><kbd>d</kbd>] ⇒ Arrange window into a grid; add <kbd>Shift</kbd> for bottom.
- <kbd>Super</kbd> + [<kbd>h</kbd><kbd>j</kbd><kbd>k</kbd><kbd>l</kbd>] ⇒ Switch to adjacent workspace; add <kbd>Shift</kbd> to move with the focused window.
- <kbd>Super</kbd> + <kbd>Control</kbd> + [<kbd>h</kbd><kbd>l</kbd>] ⇒ Switch to the previous or next workspace, by using the cube.
- <kbd>Super</kbd> + <kbd>o</kbd> ⇒ Move focus to the next output; add <kbd>Shift</kbd> to move with the focused window.

[Alacritty]: https://github.com/alacritty/alacritty
[Wofi]: https://hg.sr.ht/~scoopta/wofi

**Note** that you probably need to adapt the applications, such as the _terminal_ and _launcher_, in your config file.

## Using Wayfire

### Opening terminals and moving around

One very basic operation is opening a new terminal.
By pressing <kbd>Super</kbd> + <kbd>Enter</kbd>, a new terminal will be opened.
By default, the key-binding opens [Alacritty].

[Alacritty]: https://github.com/alacritty/alacritty

![open-terminal](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/open-terminal/2020-02-18_17:31:03.gif)

### Opening other applications

Aside from opening applications from a terminal, you can also use the handy [Wofi] which is opened by pressing <kbd>Super</kbd> + <kbd>Shift</kbd> + <kbd>Enter</kbd> by default.

![open-applications](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/open-applications/2020-02-18_17:39:34.gif)

Additionally, if you have applications you open very frequently, you can create a key-binding for starting the application directly.
See the section [Configuring Wayfire](#configuring-wayfire) for details.

### Closing windows

If an application does not provide a mechanism for closing (most applications provide a menu, the middle mouse button for tabs, the escape key or a shortcut like <kbd>Control</kbd> + <kbd>w</kbd> to close), you can press <kbd>Super</kbd> + <kbd>w</kbd>, <kbd>Super</kbd> and middle mouse button or <kbd>Alt</kbd> + <kbd>F4</kbd> to close a window.

![close-window](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/close-window/2020-02-18_17:41:03.gif)

### Moving windows

By dragging the window’s title bar with your mouse you can move the window around.
You can also drag windows by holding down <kbd>Super</kbd> and left mouse button.

![drag-window](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/drag-window/2020-02-18_16:49:54.gif)

### Resizing windows

By grabbing the borders and moving them you can resize the window.
You can also do that by holding down <kbd>Super</kbd> and right mouse button.

![resize-window](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/resize-window/2020-02-18_17:44:25.gif)

### Switching between windows

By pressing <kbd>Super</kbd> + <kbd>Tab</kbd>, you can preview the windows in the current workspace and select the next window; add <kbd>Shift</kbd> for previous.
To switch without animation, press <kbd>Alt</kbd> + <kbd>Escape</kbd>.

![switch-between-windows](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/switch-between-windows/2020-02-18_17:24:41.gif)

### Positioning windows

Press <kbd>Super</kbd> and the arrow keys for arranging windows into a grid of 2 cells.
Press <kbd>Super</kbd> + [<kbd>Home</kbd><kbd>End</kbd>] for arranging windows into a grid of 4 cells; add <kbd>Shift</kbd> for bottom.
Press <kbd>Super</kbd> + <kbd>f</kbd> to position the window in the center of the screen.
Press <kbd>Super</kbd> + <kbd>d</kbd> to restore the window to its original size and position.
You can also use the keypad, which is laid out exactly to match the slots.

![grid](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/grid/2020-02-18_16:21:37.gif)

### Using workspaces

Workspaces are an easy way to group a set of windows.
Wayfire allows you to arrange your workspaces spatially, rather than linearly.
By default, there is 9 – 3 horizontal and 3 vertical – workspaces, and you are on the first workspace.

To switch to another workspace, press <kbd>Super</kbd> + [HJKL]; add <kbd>Shift</kbd> to move with the focused window.

[HJKL]: https://en.wikipedia.org/wiki/Arrow_keys#HJKL_keys

![switch-to-workspace](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/switch-to-workspace/2020-02-18_17:59:00.gif)

You can also use the cube, by holding <kbd>Super</kbd> + <kbd>Shift</kbd> and left mouse button,
or via the keyboard with <kbd>Super</kbd> + <kbd>Control</kbd> + [<kbd>h</kbd><kbd>l</kbd>].

![cube](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/cube/2020-02-18_18:23:53.gif)

Pressing <kbd>Super</kbd> allows you to preview all workspaces in the current output.
You can use your mouse to select a workspace.
[HJKL], <kbd>Enter</kbd>, <kbd>Home</kbd> and <kbd>End</kbd> can be used to select a workspace with your keyboard; add <kbd>Shift</kbd> for bottom.

![expo](https://github.com/alexherbo2/wayfire-resources/raw/master/demos/expo/2020-02-18_18:43:52.gif)

### Moving windows to workspaces

To move a window to another workspace, simply press <kbd>Super</kbd> + <kbd>Shift</kbd> + [HJKL].

### Changing the opacity

When the modifier key <kbd>Super</kbd> + <kbd>Alt</kbd> is held down, you can scroll down and up to adjust the opacity of the window.

### Reloading the configuration

To reload the configuration, simply save the config file.
Most of the options are automatically reloaded.

### Exiting Wayfire

Press <kbd>Control</kbd> + <kbd>Alt</kbd> + <kbd>Backspace</kbd>.

**TODO**: Add a configuration option.

## Configuring Wayfire

To change the configuration of Wayfire, copy [`wayfire.ini`] to `~/.config/wayfire.ini` and edit it with a text editor.
For a complete reference, see the [Configuration](Configuration.md) document.

[Autostart]: Configuration.md#autostart
[Command]: Configuration.md#command
[Idle]: Configuration.md#idle
[Input]: Configuration.md#input
[Output]: Configuration.md#output
[Window rules]: Configuration.md#window-rules

### Setting your keyboard layout

`~/.config/wayfire.ini`

``` ini
[input]
xkb_layout = us,fr
xkb_variant = dvorak,bepo
```

See [Input] for more information.

### Terminal

Open [Alacritty], [kitty] or another terminal:

`~/.config/wayfire.ini`

``` ini
[command]
binding_terminal = <super> KEY_ENTER
command_terminal = alacritty
```

[Alacritty]: https://github.com/alacritty/alacritty
[kitty]: https://sw.kovidgoyal.net/kitty/

See [Command] for more information.

### Application launcher

**Example** – Run [Wofi], a graphical launcher, similar to [dmenu]:

`~/.config/wayfire.ini`

``` ini
[command]
binding_launcher = <super> <shift> KEY_ENTER
command_launcher = wofi
```

**Note**: Add `mode=run` or `mode=drun` to `~/.config/wofi/config`.
You can also specify the mode with `--show` option.

[dmenu]: https://tools.suckless.org/dmenu/
[Wofi]: https://hg.sr.ht/~scoopta/wofi

You might prefer [fzf] running in a terminal.

**Example** – Run [Launcher] with [fzf] and [Alacritty]:

`~/.config/wayfire.ini`

``` ini
[command]
binding_launcher = <super> <shift> KEY_ENTER
command_launcher = alacritty --command sh -c 'nohup launcher-run $(launcher-list | fzf) > /dev/null'
```

[Launcher]: https://github.com/alexherbo2/launcher
[fzf]: https://github.com/junegunn/fzf
[Alacritty]: https://github.com/alacritty/alacritty

See [Command] for more information.

### Setting the wallpaper

You can use `wf-background`, provided by [wf-shell].

[wf-shell]: https://github.com/WayfireWM/wf-shell

`~/.config/wayfire.ini`

``` ini
[autostart]
background = wf-background
```

See [Autostart] for more information.

If you prefer [swaybg], adapt the command as follows.

**Example** – Simple:

``` sh
swaybg --image /path/to/wallpaper.webp
```

**Example** – Random wallpaper:

``` sh
find -L /path/to/wallpapers -type f | sort -R | head -n 1 | xargs swaybg --image
```

[swaybg]: https://github.com/swaywm/swaybg

### Adding a panel and dock

You can use `wf-panel` and `wf-dock`, provided by [wf-shell].

[wf-shell]: https://github.com/WayfireWM/wf-shell

`~/.config/wayfire.ini`

``` ini
[autostart]
panel = wf-panel
dock = wf-dock
```

You can also use [Waybar] if you prefer it.

[Waybar]: https://github.com/Alexays/Waybar

`~/.config/wayfire.ini`

``` ini
[autostart]
bar = waybar
```

See [Autostart] for more information.

### Configuring viewport switcher

See [Viewport switcher](https://github.com/WayfireWM/wayfire/wiki/Configuration.md#viewport-switcher) for more information.

If you want to switch to another workspace by pressing super+num where num is the number of the workspace you want to use.

Follow the example below.

`~/.config/wayfire.ini`

``` ini
[vswitch]
binding_0 = <super> KEY_0
binding_1 = <super> KEY_1
binding_2 = <super> KEY_2
binding_3 = <super> KEY_3
binding_4 = <super> KEY_4
binding_5 = <super> KEY_5
binding_6 = <super> KEY_6
binding_7 = <super> KEY_7
binding_8 = <super> KEY_8
binding_9 = <super> KEY_9
```

### Configuring your outputs

See [Output](https://github.com/WayfireWM/wayfire/wiki/Configuration.md#output) for more information, setting mirroring, custom modes, etc.

Example configuration statically via Wayfire:

`~/.config/wayfire.ini`

``` ini
[eDP-1]
mode = 1920x1080@60.000000 # or just 1920x1080
layout = 0,0
transform = normal
scale = 1.000000
```

[Kanshi] supports more dynamic configuration, like switching profiles based on connected outputs:

`~/.config/kanshi/config`

```
{
  output eDP-1 mode 1920x1080 position 0,0
}
{
  output eDP-1 mode 1920x1080 position 0,900
  output HDMI-A-1 mode 1440x900 position 0,0
}
```

[kanshi]: https://wayland.emersion.fr/kanshi/

You can get the names of your outputs with [wlr-randr], or by inspecting the log of Wayfire (printed to stdout by default).

[wlr-randr]: https://github.com/emersion/wlr-randr

And start [kanshi] on startup with [Autostart].

### Notifications

Using [mako]:

`~/.config/wayfire.ini`

``` ini
[autostart]
notifications = mako
```

[mako]: https://wayland.emersion.fr/mako/

See [Autostart] for more information.

### Idle configuration

Example configuration with [swayidle] and [swaylock]:

`~/.config/wayfire.ini`

``` ini
[autostart]
idle = swayidle before-sleep swaylock

[idle]
toggle = <super> KEY_Z
screensaver_timeout = 300
dpms_timeout = 600
```

- Disables the compositor going idle with <kbd>Super</kbd> + <kbd>z</kbd>.
- This will lock your screen after 300 seconds of inactivity, then turn off
your displays after another 300 seconds.

[swayidle]: https://github.com/swaywm/swayidle
[swaylock]: https://github.com/swaywm/swaylock

See [Autostart] and [Idle] for more information.

Note: for now [there is a trick](https://github.com/WayfireWM/wayfire/issues/455#issuecomment-630125354) for binding a key to instantly turn-off the screen (DPMS).

### Screen locker

Lock your screen with [swaylock]:

`~/.config/wayfire.ini`

``` ini
[command]
binding_lock = <super> KEY_ESC
command_lock = swaylock
```

See [Command] for more information.

### Logout

Use [wayland-logout] to kill the current compositor instance.
Launch logout GUI [wlogout]:

`~/.config/wf-shell.ini`

``` ini
[panel]
menu_logout_command = wlogout
```

[wlogout]: https://github.com/ArtsyMacaw/wlogout
[wayland-logout]: https://github.com/soreau/wayland-logout

### Changing the volume

**Example** – Changing the volume with [ALSA]:

`~/.config/wayfire.ini`

``` ini
[command]
repeatable_binding_volume_up = KEY_VOLUMEUP
command_volume_up = amixer set Master 5%+
repeatable_binding_volume_down = KEY_VOLUMEDOWN
command_volume_down = amixer set Master 5%-
binding_mute = KEY_MUTE
command_mute = amixer set Master toggle
```

[ALSA]: https://alsa-project.org

**Example** – Changing the volume with [PulseAudio]:

`~/.config/wayfire.ini`

``` ini
[command]
repeatable_binding_volume_up = KEY_VOLUMEUP
command_volume_up = pactl set-sink-volume 0 +5%
repeatable_binding_volume_down = KEY_VOLUMEDOWN
command_volume_down = pactl set-sink-volume 0 -5%
binding_mute = KEY_MUTE
command_mute = pactl set-sink-mute 0 toggle
```

[PulseAudio]: https://freedesktop.org/wiki/Software/PulseAudio/

**Example** – Changing the volume with [WirePlumber] for [PipeWire]:

`~/.config/wayfire.ini`

``` ini
[command]
repeatable_binding_volume_up = KEY_VOLUMEUP
command_volume_up = wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%+
repeatable_binding_volume_down = KEY_VOLUMEDOWN
command_volume_down = wpctl set-volume @DEFAULT_AUDIO_SINK@ 5%-
binding_mute = KEY_MUTE
command_mute = wpctl set-mute @DEFAULT_AUDIO_SINK@ toggle
```

[WirePlumber]: https://pipewire.pages.freedesktop.org/wireplumber/
[PipeWire]: https://pipewire.org/

See [Command] for more information.

### Changing the screen brightness

**Example** – Changing the screen brightness with [Light]:

`~/.config/wayfire.ini`

``` ini
[command]
repeatable_binding_light_up = KEY_BRIGHTNESSUP
command_light_up = light -A 5
repeatable_binding_light_down = KEY_BRIGHTNESSDOWN
command_light_down = light -U 5
```

**Note**: Make sure that the user is part of the `video` group, otherwise you will not get access to the devices.

[Light]: https://haikarainen.github.io/light/

See [Command] for more information.

### Screen color temperature

Adjust the color temperature of your screen with [Redshift]:

`~/.config/wayfire.ini`

``` ini
[autostart]
redshift = redshift -m wayland
```

**Note**: Requires [Redshift with Wayland support][Redshift – #663].

[Redshift]: http://jonls.dk/redshift/
[Redshift – #663]: https://github.com/jonls/redshift/pull/663

See [Autostart] for more information.

### Taking screenshots

Taking screenshots with [grim] and [slurp]:

`~/.config/wayfire.ini`

``` ini
[command]
binding_screenshot = KEY_PRINT
command_screenshot = grim $(date '+%F_%T').webp
binding_screenshot_interactive = <shift> KEY_PRINT
command_screenshot_interactive = slurp | grim -g - $(date '+%F_%T').webp
```
You can also use `wl-clipboard` in combination with `grim` and `slurp` capture a screenshot to clipboard. Put this entry in `[command]` section.
``` ini
binding_screenshot_clip = <super> <ctrl> KEY_S
command_screenshot_clip = grim -g \"$(slurp)" - | wl-copy
```

[grim]: https://wayland.emersion.fr/grim/
[slurp]: https://wayland.emersion.fr/slurp/

See [Command] for more information.

### Creating screencasts

See [wf-recorder] and [wlrobs].

[wf-recorder]: https://github.com/ammen99/wf-recorder
[wlrobs]: https://hg.sr.ht/~scoopta/wlrobs

### Automatically maximizing windows

You can maximize specific windows by default with the following command:

`~/.config/wayfire.ini`

``` ini
[window-rules]
alacritty = on created if app_id is "Alacritty" then maximize
```

You can get the properties of your applications with the following command:

``` sh
WAYLAND_DEBUG=1 alacritty 2>&1 | kak
```
See [Window rules] for more information.

### Remote desktop

With virtual pointer support, programs like [wayvnc] are possible. No special wayfire configuration is required.

[wayvnc]: https://github.com/any1/wayvnc