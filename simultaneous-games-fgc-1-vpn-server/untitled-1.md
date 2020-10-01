# VPNs for both FGC + FGU \(LAN mode\)

Since **FGU-LAN mode** networking is mostly the **same as FGC** networking with the **UDP/TCP** transport as **only difference**, we are able to support either or both in our extended "N-at-a-time" setup.

With our current OpenVPN setup \(with a "/24" subnet\), **we can manage up to approximately 60 certificates/connections**.

### **For each GM certificate/connection**

_**including the initially created single/time-shared one \(my-fg-ovpn-dm01\)**_

We'll :

* **reserve 1 VPN private IP address** _\(our initial "my-fg-ovpn-dm01" connection has the ".6" private IP in the VPN subnet, so : 10.10.10.6 in our example\)_
* **choose to support** \(= implement transparent port forwarding for\) :
  * _only_ FGC \(on a TCP port\) : _like we we did previously for my-fg-ovpn-dm01_
  * _only_ FGU-LAN mode \(on a UDP port\)
  * _both_ FGC \(on a TCP port\) _and_ FGU-LAN mode \(on a UDP port\)

{% hint style="danger" %}
...to be continued...
{% endhint %}

