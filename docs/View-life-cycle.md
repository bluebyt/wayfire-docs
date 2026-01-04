This page discusses various aspects of a view's lifetime.

## View creation

There are multiple steps which need to happen when a new view is created (either a client opens a new application window, or a plugin creates a custom view):

1. The view is allocated first. At this step, constructors may set member variables, connect to signals, etc. but usually have a restricted set of available operations, because the view's associated nodes (view root node, transformed node, surface root node) are not initialized yet. Views use the `tracking_allocator` interface to allocate memory and to keep track of all 'alive' views at a given time.

2. The base view is initialized. This means that the aforementioned associated nodes are allocated. This and the previous steps are done together by `view_interface_t::create<ConcreteViewType>()`, which is what all view implementations should use.

3. The concrete view implementation is initialized. This typically happens in a static factory method associated with the concrete view implementation class. At this step, the surface root node should be set. View implementations also typically set the view's output, set fullscreen or other state if necessary.

4. The view is mapped. For plain views, this is as simple as emitting the view-mapped signal, attaching the view to the scenegraph and changing the internal mapped state (maintained in each view implementation) so that `view->is_mapped()` reports true.
   For toplevels, this process is a bit more complicated:
     - First, a transaction changing the toplevel's mapped state is started.
     - When the transaction is being applied (or immediately after):
       - Add the view to the scenegraph
       - Add the view to the workspace set of its output
       - Set the internal mapped state to true and emit view-mapped

# View unmapping

Unmapping views means making them 'inactive'. A view may be unmapped and mapped again multiple times.
Unmapping views is relatively simple (for toplevels, there is one indirection step when a transaction is first started, and then when applying the unmapped state the following is done):

- Emit the view-pre-unmapped signal
- Set the internal state to unmapped
- Disable the view root node
- Emit view-unmapped

# View destruction

Views are managed as `shared_ptr`s, so views are really 'gone' when there are no more references to it. A view must be unmapped before being destroyed! In addition, the `destruction_signal<view_interface_t>` is emitted on a view right before freeing its memory. All custom data is also destroyed at this point.
