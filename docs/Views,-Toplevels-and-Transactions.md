The aim of this page is to explain the concepts of views, toplevels and transactions and their interactions.

# Views

Views are an abstraction in Wayfire which represents any desktop surface without a parent. Typical examples include toplevel (application) windows, but also panels, backgrounds, etc.

Views support adding transformers to them (used to implement various effects), and can report their title, app-id, be pinged or closed. These are the operations common for all types of views. Note that these views do not have any desktop-related state (fullscreen, maximized, etc.) and neither do they have geometry.

Views are represented with multiple nodes in the scenegraph. First is the view root node, which, as the name indicates, contains the view and all of its child views (only toplevels have child views by default, but of course plugins can add arbitrary nodes here). The main purpose of this node is allowing plugins and core to quickly enable or disable the whole view tree, for example when the view is mapped/unmapped or minimized.

After the view root node comes the transformed node. It is the node containg the main desktop surface only (and potentially any subsurfaces). The purpose of the transformed node is to indicate the chain of transformers, which are nodes between the transformed node down to the surface root node.

The surface root node of a view is the node which represents the desktop surface + subsurfaces only. It is also the node which receives keyboard focus and defines the view position in the scenegraph (i.e. nodes above it work in output-local coordinates, but children of the surface root node have coordinates in view-local coordinates, that is, relative to the top-left corner of the view's main surface).

It is noteworthy that view nodes typically clear all children of the surface root node before mapping and after unmapping.

## Toplevel views

Toplevel views are a subclass of views which represent application windows. They have properties like geometry, tiled edges, fullscreen state, etc. managed by their toplevel subobject described below.

# Transactions

Transactions in Wayfire and many other compositors are a mechanism for applying multiple state changes to one or more objects atomically. For example, a view might be fullscreened and have its geometry updated at the same time (it would not make sense to say that the view is really fullscreen until the client submits a buffer with the full output resolution).

Transactions are composed of transaction objects, which have very simple requirements. Namely, they have to support the commit and apply operations, and also emit the ready signal after committing, so that the transaction knows when all of its objects have become ready to be applied atomically.

Because multiple transactions for the same object in parallel usually do not make sense, transactions are managed by the transaction manager. It sometimes delays transactions (and merges them with other transactions if needed) in order to avoid parallel transactions for the same object.

# Toplevels

Toplevel objects are the bridge between toplevel views and transactions. Toplevels are transaction objects with state which can be committed and applied: geometry, decoration margins, fullscreen state, tiled edges.

The commit operation requests the necessary state changes from the clients (for example, it requests that the client submits a new buffer with a particular size), and emits the ready signal when the client actually commits the new buffer. When the new state is applied, the toplevel updates the state of the view and the corresponding scenegraph nodes.

## Current and pending state

To achieve their purpose, toplevels expose two states: current and pending (internally, a third - committed - is maintained as well). The pending state generally describes the desired next state of the toplevel. Typically, plugins will update one or more pending members and then commit the toplevel. Until the client reconfigures and the transaction is applied, plugins may request further state changes. These changes are further accumulated in the pending state.

Once a state change is applied, the current state is updated to reflect the currently displayed buffer of the application.
