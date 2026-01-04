# Tips & Tricks

**Table of Contents**

- [Gtk+3 applications slow startup or .desktop files not opening](#gtk3-applications-slow-startup-or-desktop-files-not-opening)
- [Run xwayland apps as root](#run-xwayland-apps-as-root) 
- [Wine](#wine)
- [Screensavers](#screensavers)
- [Login and gtkgreet with wayfire](#login-and-gtkgreet-with-wayfire)
- [Logout and shutdown](#logout-and-shutdown)
- [Remote desktop](#remote-desktop)
- [Native webcam](#native-webcam)
- [High CPU usage](#high-cpu-usage)
- [Catch crashes with systemd-coredump](#catch-crashes-with-systemd-coredump)
- [Screen sharing](#screen-sharing)
- [Setting up monitor frequency if wayfire.ini does not work](#setting-up-monitor-frequency-if-wayfireini-does-not-work)
- [GTK Theme not working](#gtk-theme-not-working)
- [Getting the title and app-id of windows](#getting-the-title-and-app-id-of-windows)
- [Starting Wayfire as a systemd service](#starting-wayfire-as-a-systemd-service)
- [Apps are not focused when expected](#apps-are-not-focused-when-expected)

## Gtk+3 applications slow startup or .desktop files not opening

This is due to GTK+ waiting for xdg-desktop-portal to start via D-Bus. This times out because the D-Bus activated service doesn't know what `WAYLAND_DISPLAY` to connect to. This can also cause some .desktop files not to open on XDG Desktop Entry spec-compliant menus (like `wf-panel`) if they are `DBusActivatable=true`. Add the following to your configuration file:
```
[autostart]
0_environment = dbus-update-activation-environment --systemd WAYLAND_DISPLAY DISPLAY XAUTHORITY
```
For more details: https://github.com/WayfireWM/wayfire/issues/775

## Run xwayland apps as root

This is a small wrapper script that uses `xhost` and `sudo` to launch x11 clients as root under xwayland. Save to a file, make executable and use in place of sudo to run an x11 client with privileges in wayfire.

```
#!/bin/bash
# small script to enable root access to x-windows system

# enable root access
xhost +SI:localuser:root

sudo -E $@

# disable root access after application terminates
xhost -SI:localuser:root
```

## Wine
If you are having trouble with wine applications, such as input not responding, tearing windows, or buggy mouse behavior in some fullscreen games check the following:
- Make sure xwayland is enabled in wayfire meson configure output and in wayfire.ini [core] xwayland = 1
- In winecfg in the Graphics tab, enable 'Automatically capture mouse in full screen mode', disable 'Allow the window manager to decorate the windows', disable 'Allow the window manager to control the windows' and enable 'Emulate a virtual desktop'

Note for Wine/Proton games launched via Stream: you may need to set `WINEPREFIX` and `WINEARCH` and call the correct `wine` executable for the game you are configuring. For example (adjusting the command for the particular game id and steam installation path):

```
WINEPREFIX=~/.local/share/Steam/steamapps/compatdata/11020/pfx/ WINEARCH=win64 ~/.local/share/Steam/steamapps/common/Proton\ -\ Experimental/files/bin/wine64 winecfg.exe
```

## Screensavers
The Idle plugin offers a built in screensaver for wayfire. It has options to choose either DPMS or screensaver timeout. If the cube plugin is enabled, the Idle screensaver timeout will rotate the cube based on settings found in the Idle plugin. Otherwise, the screen will just become black on timeout. There is also [swayidle](https://github.com/swaywm/swayidle) which can be used in conjunction with a [special version of xscreensaver](https://github.com/soreau/xscreensaver). It is recommended to install this version of xscreensaver to a nonstandard prefix because it will not work correctly with X. To use it, make two entries in the Autostart plugin like so:
```
a2 = /opt/xscreensaver/bin/xscreensaver -nosplash
a3 = swayidle timeout 60 '/opt/xscreensaver/bin/xscreensaver-command -activate' resume '/opt/xscreensaver/bin/xscreensaver-command -deactivate'
```
Change the prefix accordingly. The first will start the daemon and the second will activate the screensaver after 60 seconds of inactivity. You may change this value to your liking.

## Login and gtkgreet with wayfire

This is a sample config that uses [gtkgreet] and [wayland-logout] and can be used with `wayfire -c`
```
[autostart]
autostart_wf_shell = false
dm = gtkgreet -l && wayland-logout

[core]
plugins = autostart
vheight = 1
vwidth = 1
xwayland = false
```

[gtkgreet]: https://git.sr.ht/~kennylevinsen/gtkgreet
[wayland-logout]: https://github.com/soreau/wayland-logout

## Logout and shutdown

Wayfire has a hard coded keybinding Ctrl+Alt+BkSpc to shutdown. It also supports layer shell protocol, which makes it possible to use a program such as [wlogout] to log out and shut down. There is also [wayland-logout] which kills the current wayland compositor instance.

[wlogout]: https://github.com/ArtsyMacaw/wlogout
[wayland-logout]: https://github.com/soreau/wayland-logout

## Remote desktop

With virtual pointer support, programs like [wayvnc] are possible.

If you find that your pointer is not following the cursor on the virtual desktop you might need to map the virtual input to the specific output wayvnc is running on. https://github.com/WayfireWM/wayfire/wiki/Configuration#input-device-specific-options

In your wayfire config file add:
```
[input-device:wlr_virtual_pointer_v1]
output = DP-1

[input-device:wlr_virtual_keyboard_v1]
output = DP-1
```
change device and output names accordingly. 

You can get the name of the input device by reviewing the wayfire log when connecting to a wayvnc session. Wayfire should be run with the debug option wayfire -d


[wayvnc]: https://github.com/any1/wayvnc

## Native webcam

Use the following command to show /dev/video0 in a native window with gst-launch-1.0 ([gstreamer]). Note that glimagesink must be available.
```
gst-launch-1.0 -v v4l2src device=/dev/video0 ! glimagesink
```
[gstreamer]: https://gitlab.freedesktop.org/gstreamer

## High CPU usage

Compile wayfire with the `release` buildtype to enable performance optimizations.
```
meson -Dbuildtype=release build
```

## Catch crashes with systemd-coredump

Build wayfire with the flag `-Dprint_trace=false`

## Screen sharing

To enable screen sharing in chromium

Add the following lines to the configuration file `wayfire.ini`

```
[autostart]

0_0 = systemctl --user import-environment
xdg = sleep 1 && (XDG_SESSION_TYPE=wayland XDG_CURRENT_DESKTOP=sway /usr/lib/xdg-desktop-portal --replace & /usr/lib/xdg-desktop-portal-wlr)
```
(This is for arch linux, for other distributions the path could have /libexec/ instead of /lib/)

Install `xdg-desktop-portal` and `xdg-desktop-portal-wlr`

Edit the configuration file `~/.config/xdg-desktop-portal-wlr/config`

```
[screencast]
output_name=
max_fps=30
chooser_cmd=slurp -f %o -or
chooser_type=simple
```
(you should add your output_name)


test that this script works:
https://gitlab.gnome.org/-/snippets/19


set this flag on chrome

chrome://flags/#enable-webrtc-pipewire-capturer

## Setting up monitor frequency if wayfire.ini does not work 

If your output = kanshi as default, you may need to create a config file in $HOME/.config/kanshi/config with your desired settings
e.g:
```
profile { 
	output DP-1 mode 2560x1080@74.990997 position 0,0
}
```

See kanshi git for more info: https://github.com/emersion/kanshi

## GTK Theme not working

[This script from a sway article](https://github.com/swaywm/sway/wiki/GTK-3-settings-on-Wayland) can set your theme for you, assuming you already had one set with X.

```sh
#!/bin/sh
# .config/setWaylandTheme.sh

# usage: import-gsettings
config="${XDG_CONFIG_HOME:-$HOME/.config}/gtk-3.0/settings.ini"
if [ ! -f "$config" ]; then exit 1; fi

gnome_schema="org.gnome.desktop.interface"
gtk_theme="$(grep 'gtk-theme-name' "$config" | sed 's/.*\s*=\s*//')"
icon_theme="$(grep 'gtk-icon-theme-name' "$config" | sed 's/.*\s*=\s*//')"
cursor_theme="$(grep 'gtk-cursor-theme-name' "$config" | sed 's/.*\s*=\s*//')"
font_name="$(grep 'gtk-font-name' "$config" | sed 's/.*\s*=\s*//')"
gsettings set "$gnome_schema" gtk-theme "$gtk_theme"
gsettings set "$gnome_schema" icon-theme "$icon_theme"
gsettings set "$gnome_schema" cursor-theme "$cursor_theme"
gsettings set "$gnome_schema" font-name "$font_name"
```

add the script to autostart in your wayfire.ini. don't forget to add exec permissons.

```
[autostart]
a0 = ~/.config/setWaylandTheme.sh
```

## Getting the title and app-id of windows

Getting the title and/or app-id of a view/window (e.g. for use with the `window-rules` plugin) requires the use of the [`wlr-foreign-toplevel-management-v1`](https://wayland.app/protocols/wlr-foreign-toplevel-management-unstable-v1) protocol. Prior to Wayfire 0.8.0, this was implemented in core, but it's now implemented by the `foreign-toplevel` plugin.

Once support is enabled, a list of the title and app-id of views/windows can be found via the [`lswt` program](https://sr.ht/~leon_plickat/lswt/). Alternatively, one can compile and run the `foreign-toplevel.c` program; prior to 2023-10-12, this was available in the `examples` directory of the wlroots source, but is now [part of the wlr-clients repository](https://gitlab.freedesktop.org/wlroots/wlr-clients/-/blob/master/foreign-toplevel.c?ref_type=heads).


## Starting Wayfire as a systemd service

### System Unit

You should obviously consider the security implications that starting wayfire automatically could have.

These instructions are inspired from [Weston's](https://lists.freedesktop.org/archives/wayland-devel/2017-November/035973.html).
In order to start wayfire, we need to have a libseat session running (which is why it's not possible eg. to start it directly from an ssh session), and the unit adds boilerplate that enables it.

```ini
[Unit]
Description=Wayland Compositor Service
After=systemd-user-sessions.service
After=graphical.target
After=session-c1.scope

[Service]
Type=simple
ExecStart=/usr/bin/wayfire
Restart=always
RestartSec=10
User=wayfireuser

# Reuse /etc/pam.d/login
PAMName=login

# Set some useful variables
Environment=XDG_RUNTIME_DIR=/run/user/%U
Environment=DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/%U/bus
Environment=LIBSEAT_BACKEND=logind
Environment=XDG_SESSION_CLASS=greeter
Environment=XDG_SESSION_TYPE=wayland

# A virtual terminal is needed, we're using #7
TTYPath=/dev/tty7
TTYReset=yes
TTYVHangup=yes
TTYVTDisallocate=yes

# Fail to start if not controlling the tty.
StandardInput=tty-fail

# To get -d output
StandardOutput=journal
StandardError=journal

# Log this user with utmp, letting it show up with commands 'w' and 'who'.
UtmpIdentifier=tty7
UtmpMode=user

# Activate seat... cf. https://github.com/cage-kiosk/cage/issues/383
ExecStartPre=+chvt 7

[Install]
WantedBy=graphical.target
```

## Apps are not focused when expected

E.g. when opening a file, an already open text editor view should be focused. If this is not working, you need to enable the [XDG Activation protocol](https://github.com/WayfireWM/wayfire/wiki/Configuration#xdg-activation-protocol) plugin

