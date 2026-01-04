This document outlines how the IPC is implemented in Wayfire and aims to help people who want to either develop custom IPC methods in plugins or want to create client IPC bindings.

# Wire format

Wayfire uses (by default) a unix socket for communication with clients. The location of the socket is exported in the `WAYFIRE_SOCKET` environment variable. Messages in both direction follow a very simple wire format: 4 bytes encoding the length (little-endian) of the actual message (excluding the 4-bytes header), followed by the actual message.

Messages are always serialized json objects.

## Message structure


### Client -> Compositor
All messages going from clients to Wayfire are method calls. As such, they must have the following structure:

```
{
"method": "<method name>",
"data": {
 <whatever data is needed for the particular method>
}
}
```

A particular example with the `wm-actions/set-always-on-top` method:

```
{
"method": "wm-actions/set-always-on-top",
"data": {
 "view-id": 15,
 "state": true,
}
}
```

### Compositor -> Client

Messages send from the compositor to the client are less structured:

- When the compositor sends a message as a response to client's last method call (it always sends a response for each method call), then the json object has no constraints on its structure. However, very often Wayfire will return something containing either `{"result": "ok"}` or `{"error": "<description of the error>"}`.
- When the compositor sends a notification to clients which have subscribed for events, then they will get a message containing `{"event": "<event type>"}`.

For a complete view of what methods do and what they return, always take a look at the source code :)

# IPC in the compositor

Wayfire's IPC mechanism is made so that it is nearly independent from the actual socket implementation. There are three main objects:

- The IPC socket itself, provided by the `ipc` plugin. This plugin realizes the aforementioned wire format and dispatches calls to the method repository.
- The IPC method repository. Every plugin that wishes to provide IPC methods registers its methods in this method repository. A method in this context is seen as a function which takes the json data (the `"data": {}` subobject from the wire format section) and generates a response for the client.
- IPC clients (objects which implement this interface): they represent a connected client. Used so that the IPC methods can identify clients and send them notifications.

With this division, it is possible to provide these IPC methods also over DBus, or any other IPC variant, as long as the json data can be transported to the client. All plugins providing actual functionalities work only with the method repository, which is independent of the actual IPC realization.

