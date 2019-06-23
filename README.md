# A "transparent" OpenVPN setup for Fantasy Grounds

This document will describe a solution for GMs \(Game Masters\) to publish their FG virtual table to remote players through an ad-hoc "GM-only" VPN hosted on a small AWS \(Amazon Web Services\) Linux server.

## Do you _really_ need a VPN for ![](.gitbook/assets/fg35.png) Fantasy Grounds ![](.gitbook/assets/fg35.png) ?

{% hint style="success" %}
**Most GMs do not need to use a VPN** to successfully share their virtual table for players
{% endhint %}

{% hint style="success" %}
**Players generally do not need to use a VPN**
{% endhint %}

But there are situations where **a VPN may be needed \(from the GM's point of view\)** :

1. **temporary connection from a "public" internet facility** \(hotel room, etc\)
2. country/ISP setup makes a **unique public IPv4 address unavailable**
3. **proper port forwarding not possible** for one \(or more\) router\(s\)
4. **upstream "closed" firewall**

{% hint style="warning" %}
Also remember : **if you are a player and your GM is using a "closed VPN" offer like Hamachi, you will have to be a member of the same VPN as the GM.**
{% endhint %}

{% hint style="success" %}
This document presents a **"transparent" VPN solution**, where :

* **only the GM** uses a VPN
* the VPN is used to **forward FG traffic only** \(TCP 1802\)
* the **players do not have to install/use any kind of VPN** or custom IP routing software
{% endhint %}

