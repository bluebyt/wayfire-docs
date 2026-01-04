# General information

## Name

`wayfire` – A 3D [Wayland] compositor, inspired by [Compiz] and based on [wlroots].

[Wayland]: https://wayland.freedesktop.org
[wlroots]: https://github.com/swaywm/wlroots
[Compiz]: https://launchpad.net/compiz

## Synopsis

```
wayfire [options…]
```

## Options

```
-c <config>
  Specifies a config file.

```

## Description

Wayfire is a 3D [Wayland] compositor, inspired by [Compiz] and based on [wlroots].

It aims to create a customizable, extendable and lightweight environment without sacrificing its appearance.

You can run `wayfire` directly from a TTY, or via a Wayland-compatible login manager.

## Configuration

`wayfire` searches for a config file in the following locations, in this order:

1. `$XDG_CONFIG_HOME/wayfire.ini`
2. `~/.config/wayfire.ini`


An example configuration can be found [here][`wayfire.ini`].
You are encouraged to copy this to `~/.config/wayfire.ini` and edit it from there.

For information on the config file format, see [Configuration](Configuration.md).

[`wayfire.ini`]: https://github.com/WayfireWM/wayfire/blob/master/wayfire.ini