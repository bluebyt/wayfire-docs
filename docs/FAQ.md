Here is a non-extensive list of some common issues which arise when using Wayfire.

**Table of contents**
* [1. Wayfire shows just a black screen with a cursor visible](#1-wayfire-shows-just-a-black-screen-with-a-cursor-visible)
* [2. The mouse cursor is invisible](#2-the-mouse-cursor-is-invisible)
* [3. Compilation error X](#3-compilation-error-x)
* [4. Monitor displays garbage/ is flickering/ is black](#4-monitor-displays-garbage-is-flickering-is-black)
* [5. A plugin is enabled but does nothing](#5-a-plugin-is-enabled-but-does-nothing)

#### 1. Wayfire shows just a black screen with a cursor visible

Wayfire itself is just a "bare-bones" compositor, similar to Mutter. If you want a background and a panel, you should also install or update [wf-shell](https://github.com/WayfireWM/wf-shell). In case you install to non-standard prefix and you do not use `wf-install`, don't forget to set the appropriate environment variables(for example, `LD_LIBRARY_PATH` so that the panel/background can find `wf-config`, and `PATH`).


#### 2. The mouse cursor is invisible

This is a known problem for ex. with the nouveau driver. As a workaround, set the `WLR_NO_HARDWARE_CURSORS=1` environmental variable before starting Wayfire.

#### 3. Compilation error X

Wayfire depends on `wlroots`, which sometimes has breaking changes. To find a compatible version of wlroots:

1. Wayfire releases usually mention which wlroots release(!) they are compatible with.
2. Wayfire master branch usually compiles with wlroots master branch.
3. When in doubt, look at the `subprojects/` folder. The wlroots submodule listed there always points towards a wlroots commit which is compatible with the checked out Wayfire version.

#### 4. Monitor displays garbage/ is flickering/ is black
Try setting the evironment variable ```WLR_DRM_NO_MODIFIERS=1``` before launching wayfire. See [#1397](https://github.com/WayfireWM/wayfire/issues/1397).

#### 5. A plugin is enabled but does nothing

Plugins that cause an error (throw an exception) when Wayfire attempts to load them will be discarded. Despite this, they will still show up as "enabled" in WCM (as WCM only looks at the configuration file). You can verify if this happened by looking at Wayfire's logs -- look for any error message that says ``[src/core/plugin-loader.cpp:167] Failed to load plugin`` (logs are usually appended to the system logs when running a graphical session or displayed in the terminal if running Wayfire from the command line). The most likely cause is trying to run a newly installed or updated plugin without restarting Wayfire (in most cases this is required), or an actual bug in the plugin. Simply try restarting Wayfire and try again.
