---
description: >-
  Overall description of the "transparent" VPN solution and why it may be needed
  depending on GM situation.
---

# Introduction

## So, what is the problem ?

### Fantasy Grounds "Classic" communications

_This document focuses on FG "Classic"_ \(as opposed to the future release of FG "Unity", currently planned for the end of 2019\).

The current release of FG \("Classic"\) as of this writing is v3.3.7 \(for the "Live" branch, v3.3.8 for the "Test" branch\), but **all versions of FG "Classic"** should work in the same way.

{% hint style="info" %}
As a Game Master, **hosting a virtual table** implies that the table \(= actually the FG program\) has to be **reachable from the the players**' FG programs running on distant computers.

Fantasy Grounds \("Classic"\) uses **TCP port 1802 on IPv4** for communications between the GM and player Fantasy Grounds instances.
{% endhint %}

So, to join a GM's virtual table, **a player's FG only has to know/find the GM's IP address** where it will be able to connect to \(on the TCP 1802 port\).

The IP\(v4\) address used will be one of :

1. the **GM's "public IP"** address for players connecting **through the Internet**
2.  the **GM's "private IP"** address for players located **in the same local network**

{% hint style="warning" %}
Be aware of the above **distinct requirements for player connection from the Internet / from the in-house network** \(same ethernet LAN or wireless access point\).
{% endhint %}

Let's have a look at all the barriers between your players and your shiny new virtual table, starting from your own GM Fantasy Grounds outwards...





