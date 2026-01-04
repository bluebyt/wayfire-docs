Broadly speaking, the projects in [WayfireWM](https://github.com/WayfireWM) can be divided in two groups:

1. Wayfire as a compositor. Here, the main repositories are [wayfire](https://github.com/WayfireWM/wayfire) and the supporting libraries [wf-config](https://github.com/WayfireWM/wf-config), [wf-utils](https://github.com/WayfireWM/wf-utils) and [wf-touch](https://github.com/WayfireWM/wf-touch).

2. Wayfire as a small desktop environment. The most important components are [wf-shell](https://github.com/WayfireWM/wf-shell) and [wcm](https://github.com/WayfireWM/wcm), also [wf-install](https://github.com/WayfireWM/wf-install) and [wf-osk](https://github.com/WayfireWM/wf-osk). [wayfire-plugins-extra](https://github.com/WayfireWM/wayfire-plugins-extra) is a little weird, because it is much more closely tied to the compositor, but is still optional.

# Compositor

Currently, the compositor (and so everything in group 1) is where the most development effort goes. There are multiple reasons for that:

- The compositor has the most features and responsibilities.
- The compositor defines the API for plugins, one of the main selling points of Wayfire. A stable API is a prerequisite for expanding Wayfire's external plugins without requiring their constant maintenance.
- The compositor can be shared with other desktop projects like carbonOS and DesQ.

### Goals

1. Be as flexible as possible, while providing an easy-to-use framework. Since these goals often conflict, a compromise needs to be made.
2. Do not include unnecessary features in core or in the core plugins. They only weigh the project down. Core plugins are supposed to fit at least one of the following criteria:
    - Show how to implement common DE/window manager functionality in Wayfire. vswitch, switcher, expo, scale, simple-tile all fit here. Feature-completeness is however not a goal in itself.
    - Demonstrate various features of the Wayfire API, for example extra-gestures, cube and decoration.
    - Plugins which try to push the boundaries of what is possible are sometimes also included. Blur, Wobbly, simple-tile.
3. Plugins should be as modular and as self-contained as possible, even if this means missing on features which require integration between multiple plugins. Rationale: core plugins should be easy to replace, and to integrate in other projects/DEs based on Wayfire.

### Is there a roadmap somewhere?

The closest thing to a roadmap are the [milestones on Github](https://github.com/WayfireWM/wayfire/milestones). They represent a minimal set of bugs/features which need to be implemented for a given release. They are the 'priority' issues for a release. But of course, other contributions are welcome, we do not follow a strict plan (we can always implement more, not less).

In general, current development is focused on stabilizing core and the core plugins and polishing the API. Few new plugins and features are accepted, except if they don't fit the described goal. If you think that something important is missing though, feel free to reach out.

# DE Components

A secondary goal for WayfireWM is the creation of a small desktop environment. This is especially useful as users can see a graphical interface to the compositor, with a background, panel, etc., because neither of these things are provided by the compositor itself.

Because of limited resources, these parts of the project have a lot of rough edges and missing features. In addition, there is no concrete plan for the future development of wf-panel, wf-osk and others. Thus, not many new features and bug fixes are to be expected in the foreseeable future (at least until Wayfire 1.0). This doesn't mean that PRs are not desired! External contributions are welcome, be they small or big. Even large refactors/changes are acceptable, if you have an idea how we can make these projects better.