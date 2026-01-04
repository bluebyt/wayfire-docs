# How to create a good bug report

First **identify which component causes the bug**. It can be:
1. A plugin - if the problem happens while using a plugin or it doesn't happen when a plugin is disabled
2. Wayfire core - when the bug is not related to a plugin
2. Wlroots - Doesn't happen very often, but if the bug is connected to some application that is misbehaving in wayfire, you can also try to see if this app works in Sway or Rootston (the example wlroots compositor). If the bug is present there, too, then please tell so in the issue.

Another thing to bear in mind: If you have tried wayfire awhile ago (before August 2018 actually) you are very likely to have an outdated config file. If this is the case, update it using the new example. Otherwise many of the options may not work or some old plugin might crash the compositor.

**Important**: If the bug is actually a crash, you will get a backtrace in the log (stdout). Send it when reporting the bug. If you can reproduce the bug, it would be even better to compile with ASAN, as it will give a much better stacktrace:

```
meson debugbuild --prefix=/usr --buildtype=debug -Db_sanitize=address,undefined
ninja -C debugbuild && sudo ninja -C debugbuild install

wayfire &> log.txt
```

The last command will create a file `log.txt` in the current directory, containing the debug logs and the backtrace.

# Debugging tricks for developers

If you are working on Wayfire's core or plugins (or are an advanced user trying to figure out the root cause of a problem), Wayfire provides a few utilities for debugging:

1. Extended debugging flags for different categories of events. The full list of categories can be found in [`main.cpp`](https://github.com/WayfireWM/wayfire/blob/master/src/main.cpp#L178), and each of them can be enabled by using `-d <category>` on the command line when starting Wayfire. Keep in mind that enabling some of these categories will result in lots of output in the log, which is why they are disabled by default. Some categories are:
    - `txn`, `txni`, `views`: related to view state and transactions
    - `xwayland`, `wlroots`, `layer-shell`
    - `pointer`, `kbd`
    - `wset`

2. When debugging visual problems, sometimes it is useful to try damaging the full screen on every frame (use `-R`). Also, you can try using `-D` which paints all non-damaged regions in yellow, thus allowing the user to figure out which parts of the screen are being repainted each frame.

3. Wayfire provides an API for printing out the current stacktrace (`wf::print_trace(bool)`). The boolean argument indicates whether the fast (true) or the slow (false) mode should be used. The slow mode uses `addr2line` and is generally a really slow method, but will result in better output which contains file names and precise line numbers.

4. If you are debugging anything scenegraph related, you can use `wf::dump_scene()` to print Wayfire's current scenegraph to the log. Also, you can try using some of [my personal plugins](https://github.com/ammen99/wayfire-plugins), especially `ammen99-debugging` and the script `wdbg.py` - the `ipc` plugin is required for these. It can be used from a terminal with `wdbg.py dump-scenegraph`, which will print a colorful description of the scenegraph in the terminal.