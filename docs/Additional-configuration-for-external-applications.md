An WIP guide for configuring the necessary daemons so that all applications work in Wayfire. If know anything missing here, please add it.


## Networking
* VPN activation fails via NetworkManager 

```
(Error: "Failed to request VPN secrets #3: No agents were available for this request").
```

gnome-shell provides some agent for NetworkManager that is not running with Wayfire. In GNOME the agent may be provided via `network-manager-openvpn-gnome` / `network-manager-vpnc-gnome` / `network-manager-openconnect-gnome` depending on the selected VPN solution. 

It seems like a backend service for `libsecret` is missing. It's either `gnome-keyring-daemon` or `kwallet`.

## Input

* Selected keyboard layout not applied (U.S. keyboard layout is set)



