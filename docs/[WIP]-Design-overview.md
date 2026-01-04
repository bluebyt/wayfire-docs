The goal of this document is to describe the various components in Wayfire's core and elaborate a bit on how they interact with each other.

**Note 1**: If you are not sure what a Wayland compositor is, make sure to first read [some of the tasks of a wayland compositor](https://github.com/WayfireWM/wayfire/wiki/%5BWIP%5D-What-does-a-compositor-do%3F) to better understand what all of the classes below are needed for.

**Note 2**: this isn't a full description of Wayfire's API. For details, refer to the corresponding header files and the doc strings for each method/field. Sometimes the doc strings can provide more context or explain why a certain feature is useful.

## High-level overview

Wayfire follows a mostly object-oriented design (but individual functions are also present where that makes sense). As a result, most of the logic is structured around the following main classes:

- The singleton class `wf::core_interface_t`. It is a composition of various other classes, which have responsibility for output management, input handling and other smaller functionalities.
- `wf::output_t` is a per-monitor object. It is again a composition of several inter-connected subobjects:
  - `render_manager_t` is the subobject responsible for all drawing operations and damage tracking.
  - `workspace_manager_t` is the subobject which keeps track of view stacking order and workspaces.
  - `plugin_loader` is the subobject which dynamically loads and unloads the plugins on each output.
- `wf::view_interface_t` is an abstraction for any window-like entity. Typically used to represent client windows together with their position, transforms, child views (dialogs), decorations and more.
- `wf::plugin_interface_t` is the base class for all plugins.

These objects form a hierarchy: Plugins and Views are part of an Output, Outputs are part of Core.

Communication between these objects is done in two main ways: for calls up in the hierarchy (e.g. views -> output), a direct method call is used. For downward communication to outputs/views, methods are also often used, but communication with plugins is done only via signals and a few specialized callbacks (render hooks and input grabs). Both realize the Observer design pattern.

The following sections should provide a little more detail on each of these parts.

## Core

This is the first object that gets created in Wayfire. It is usually accessed via `wf::get_core()` which returns the single instance of this class.

#### Public API
- `output_layout` is the pointer to the output layout manager. This object is responsible for managing the current configuration of outputs (their mode, position, scale, mirroring, DPMS, etc.), as well as for creating and destroying outputs when they are plugged and unplugged by the wlroots backend. It also is responsible for creating and destroying virtual outputs in case that no physical outputs are present.

- `config` is the object where all of Wayfire's configuration is stored. This is the glue code between plugins (which directly consume options via `wf::option_wrapper`) and the configuration backend, which is responsible for populating the config manager with options and for updating it at runtime.

- `protocols` contains objects to all wlroots-enabled protocols that core creates.

- Methods for controlling the focused layer (used by lockscreens & similar)

- Methods for focusing a particular output

- Registering a view via `add_view`

- Methods which are wrappers around seat and input related functionality - touch gestures, manipulating the cursor and keyboard focus.

#### Private members

The core implementation (not public API, accessible by `wf::get_core_impl()`) has a few further important members:

- `input` is the object which contains input state like: currently active input grab, exclusive clients like lockscreens which disable input to other clients, active input devices.

- `seat` represents a group of input devices which logically belong together. Each seat has a cursor and keyboard focus (which get the corresponding input events), has its own cursor image and position, and also supports primary selection, clipboard and drag-and-drop.

Note: While some wlroots compositors support multiple seats, Wayfire's design currently does not allow more than one.

## Output

Outputs are important objects in Wayfire. In contrast to some compositors like Mutter(GNOME), every output is almost completely independent from the others. This means: every output has its own set of workspaces. Each output has its own set of plugin instances. Different plugins (actually plugin instances) can be active on different outputs at the same time.

### Render Manager

Render manager is, yet again, a composition of smaller modules with various rendering-related functionalities. Together, they have the following tasks:

- Damage tracking. This is a technique employed by many compositors, which in nutshell means: track which regions of the output have changed (for example, a fullscreen client updates its left half, so the left half of the output needs to be updated).

- Frame scheduling and repaint delay: Whenever something on the output changes, schedule and wait for the next page flip (on DRM backend), then after a particular delay, repaint the output and swap the buffers (everything is double-buffered).

- Update workspace streams. Workspace streams are a mechanism used by Wayfire to update buffers on each frame with the contents of a particular workspace. For example, the Expo plugins has a workspace stream per workspace, and the corresponding buffers are then arranged next to each other on the output space.

- Keep track of various hooks and custom renderers. The default Wayfire renderer simply has a workspace stream to the output's buffer. However, plugins can override the default renderer to achieve different perspectives of the workspaces and windows (cube, expo), and can also add various overlays and postprocessing effects.

### Workspace Manager

Workspace manager is also a composition of multiple subobjects. Its main tasks are:

- Keep track of the stacking order of views. All views on an output are part of a tree-like structure:
  - The workspace manager has a list of layers. These are used for rough ordering of the views, and differentiating them by their role (i.e panels, backgrounds, regular views, menus). The available layers and their Z-ordering are hardcoded.

  - Each layer consists of several sublayers. A sublayer is a container of several views, which are kept together in their stacking order relative to other views, but views inside a sublayer can be reordered relative to each other. A layer has three types of sublayers:
    - sublayers docked below: Fixed-position sublayers, always below other views in the layer.
    - sublayers docked above: Fixed-position sublayers, always above other views in the layer.
    - floating sublayers: sublayers below the docked sublayers, they can be reordered wrt. each other.

  - An exception to this Z-ordering are fullscreen views. If and only if a fullscreen view is the topmost view in the workspace layer, this view is temporarily pushed on top of panels (it is promoted). However, as soon as another view is restacked on top of the fullscreen view, it loses its promoted status.

- Keep track of the current workspace, and view <-> workspace mappings. In Wayfire, workspaces are arranged in a grid, and each workspace has different coordinates (the current workspace starts at `(0, 0)`, the one on the left at `(1920, 0)` if `1920` is the output's logical size). Since workspace's coordinates are relative to the current workspace, each workspace's coordinates actually change whenever the current workspace changes.

- Views are not constrained to a single workspace. They can be visible on two or more workspaces, depending on their geometry.

### Plugin Loader

This is perhaps the simplest part of an output. The plugin loader is created after an output has been initialized, and it creates a plugin instance for each enabled plugin for this output. This plugin is then destroyed if it is disabled in the config file or the output is destroyed.

## Views

The `wf::view_interface_t` class is the base class for all windows visible on the screen. In the most basic sense, they are axis-aligned 2-D rectangles with some content, which can be moved around and managed by plugins. Each view has a position, size, bounding box (including subsurfaces, see the points below), all of which are located in the output-workspace-local coordinate system. In other words, their coordinates are relative to the current workspace.

#### View types

There are 4 main types of views:

- Views which represent client windows. These views are all subclasses of `wf::wlr_view_t` (not part of the API). 
  - `xdg-shell` views, they are used for Wayland-native clients.
  - `xwayland` views. There are several subtypes of views here, depending on what the Xwayland view is.
  - `layer-shell` views. These are created for special clients like panels, backgrounds, etc., which utilize the layer-shell protocol.
- Views which have compositor-generated content. Examples of such views include colored rectangles used for previewing view geometry (for example Move's snap previews), or they can mirror the contents of another view. The latter case can be useful in case the view needs to be visible on multiple locations (or even multiple outputs) at the same time.

#### Subsurfaces and dialogs

Views can have two types of children: dialogs (part of the view tree) and subsurfaces (part of their surface tree). There are several notable differences:

- Dialogs (child views) are views on their own. They are basically glued to the main view and are not kept separately in the stacking order of workspace-manager. However, because they are views, they can be moved around, they can have their own transforms, etc. Dialogs can be modal (they are always focused instead of the main views) or non-modal, in which case both the view and the dialogs can be focused. Note that in any case, dialogs remain on top of their parent.

- Subsurfaces are not views, they are inseparable parts of the view. They are included in the view's bounding box, move together with the view and have the same transforms applied to them (rotation, scaling, etc.). Subsurface lifetime is tied to that of their main view, and are unmapped/destroyed together with it.

#### Transformers

A good number of plugins need to present views in a different way than they typically appear. For example, the Scale plugin needs to scale down and  translate views while it is active. Sometimes, the background views have to be dimmed a bit, or a view's background needs to be blurred. All of this is implemented via a mechanism called view transformers. Each view can have multiple transformers attached to it. In each transformation step (each transformer), the view is rendered to an auxiliary buffer, and its coordinates are changed according to the transformer. In this way, we can achieve arbitrary combination of transformers, each of which can use a different OpenGL shader and settings.

#### View state

Each view can be in a number of states. It can be fullscreen, maximized (in the code tiled), minimized or sticky. Note that changing the view's state should almost never be done by setting `view->fullscreen` & co directly. Instead, you should at least use the wrapper functions `set_fullscreen()/...`, or even better, `request_fullscreen()/...`. The former does basic sanity checking for the new state of the view, and might perform a few necessary actions, and will notify plugins of the change. The latter will allow core and/or other plugins to actively react to the change in the state - for example, `request_fullscreen()` will actually change the view size to match the full output size, and will also trigger an animation with the Grid plugin, if it is enabled.

## Plugins

Plugins are the most important feature in Wayfire. Its philosophy is: almost everything is a plugin! This allows the user to overwrite most of the compositor's behavior and/or add new powerful features. Most plugins are meant to be self-sufficient and to not interact with other plugins directly (with a few notable exceptions).

#### Life-time

The life-time of a plugin is like this:

- For each new output, an output-local instance is created for each plugin. There are also mechanisms for having shared data between instances, or having a single instance.
- For each removed output, the output-local instance of each plugin is destroyed.
- If a plugin is disabled in the config, then all instances on all outputs are destroyed.

#### Internal plugin architecture

A typical plugin will register callbacks for various signals, effect hooks and bindings in its `init()` function.
Then, the plugin reacts to these events, keeps internal state, and calls core or other plugins to achieve its purpose.

#### Plugin interaction

Plugins interact with each other in one of the following ways:

- Synchronizing/communicating via core interfaces. For example, a plugin calls `view->request_maximize()`, core emits a signal and another plugin carries out this action.
- Creating common interfaces, which are like extensions of core's own interfaces. As an example, take the wobbly plugin: it provides an extension to the view interface, where you can trigger a set of predefined actions (anchoring, translating, resizing, etc.).
- Some plugins export some of their functionality in their headers (e.g. workspace-wall). These plugins are then not manually enabled in the config file by the user, they are directly included in each plugin which needs them.
- Using capabilities. A plugin can request that it is activated with certain capabilities. This means, it wants exclusive access to certain parts of Core. For example, a plugin which wishes to grab input via input grabs (there can only be one grab at a time!) need to activate with the input grabbing capability. If other plugins attempt to activate this way, they will fail in the activation stage and will thus know that they cannot proceed.
