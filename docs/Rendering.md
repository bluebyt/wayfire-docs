# Backends

In the Wayland ecosystem, compositors typically use either OpenGL ES or Vulkan for rendering, and the pixman library in rarer cases for software rendering. Each has different strengths and weaknesses:

- OpenGL ES is GPU-accelerated, widely supported and well-tested. Wayfire used to support only this backend and as a result many special effects plugins are geared towards it - for example Cube, Wobbly, Fire animation to name a few. These plugins will not work with the other renderers, as porting them to Vulkan is still not done.

- Vulkan is a newer GPU-accelerated renderer. It is still considered experimental in wlroots, however it has the potential to lower the CPU overhead. Moreover, newer work on color management in wlroots is focused on it, which makes color management currently work primarily with the vulkan renderer only.

- Pixman is a software (CPU-based) renderer. It is useful mostly for testing purposes and on some systems where the GPU is unavailable or does not support the necessary functionality for rendering, as its performance is far from optimal.

Wayfire utilizes the `wlr_renderer` abstraction which supports all of these backends. It is sufficient for simple compositing operations, as well as scaling and translation.

# Special effects

Some plugins mentioned in the previous section like Cube, Wobbly and Blur need custom rendering operations not available in `wlr_renderer` like 3D transformations and custom shaders. They are currently implemented only via OpenGL ES commands inserted in the rendering pipeline and therefore will need updates to work with Vulkan.

# Auxilliary buffers

Plugins like Expo need additional buffers to render to. Wayfire's rendering API provides three classes to deal with buffers:

- `wf::render_buffer_t` is a very simple wrapper holding a `wlr_buffer` together with its size.
- `wf::auxilliary_buffer_t` is a helper class to manage temporary buffers used by various plugins. Its main function is to manage the backing `wlr_buffer` and `wlr_texture` (used when sampling from the buffer) and to automatically free them when it goes out of scope.
- `wf::render_target_t` is a subclass of `wf::render_buffer_t`. It can be constructed either from a given `wf::render_buffer_t` or `wf::auxilliary_buffer_t` and contains additional information for the rendering process like buffer transform, scaling options, mapping from logical coordinates to buffer coordinates, etc.