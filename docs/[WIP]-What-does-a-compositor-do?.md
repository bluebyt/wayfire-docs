The goal of this page is to briefly explain what a Wayland compositor is responsible for in a typical setup. See also the [wayland docs](https://wayland.freedesktop.org/docs/html/ch03.html#id-1.6.2.8)

## General

The compositor is (very roughly) the GUI part of an operating system. It provides a graphical environment for applications, manages input and output devices, and more.

## Rendering

In Wayland, regular applications (clients) draw to an off-screen buffer. Then they present this buffer (commit it) to the compositor. In Wayfire and many other compositors, these buffers are uploaded or converted to OpenGL ES textures.

Finally, whenever a new frame can be drawn (depending on hardware refresh rate), the compositor is responsible for updating the portion of the output which actually changed.

## Window management

In contrast to X11, in Wayland every application works in almost complete isolation from the other clients. Clients do not even know their position on the screen. Thus, a Wayland compositor needs to keep track of the position of each window, which outputs it is visible on, whether it has a decoration or not, what transforms are applied (for ex. in various effects), etc.

A wayland compositor will also act as a X11 window manager for Wayland windows: it is responsible for interactive move/resize operations, it provides workspaces, it provides effects like alt-tab window switching, maximizing, fullscreening, minimizing windows, etc.

## Input event handling and forwarding

Since clients have no idea about their position, stacking order, etc., the compositor is also responsible for receiving input events from the drivers (typically via libinput), and then processing them. A compositor would typically have a list of key and button bindings (`Alt-Tab`, `<super> + KEY_LEFT` to start interactively moving a window). When an input event arrives, first the bindings are checked, if none match the input event, then the appropriate client is selected and the event is forwarded to it.

This allows the compositor to support many different modes of operation, and also to pass the correct position to a transformed window.

## Various other functionalities

The compositor also has a lot of other functionalities it can (and should) offer. Here is a non-exhaustive list:

- Managing multiple outputs, their logical positions, modes, output mirroring, etc.
- Tracking user idleness, e.g. turning off monitors after X seconds of inactivity to conserve power.
- Provide support for virtual input devices
- Facilitate copy-paste and drag-and-drop operations (remember, clients don't know much about each other!)
- Implement screensharing/recording protocols (wlr-screencopy-v1, or PipeWire/xdg-desktop-portal)
- Support legacy Xwayland clients


