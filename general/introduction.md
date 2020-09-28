# Introduction - F.G.Comms

## Fantasy Grounds \("Classic"\) communications

**This chapter -and most of this document- focuses on FG "Classic"** \(as opposed to ~~the future release of~~ FG "Unity", currently ~~planned for the end of 2019~~ in Early Access as of september 2020\) on a Microsoft **Windows machine**.

{% hint style="info" %}
FG **Unity / Classic** have **different** communication methods.   - see [FGU vs. FGC networking](../fgu-fantasy-grounds-unity/fgu-vs.-fgc-networking.md)
{% endhint %}

The current release of FG \("Classic"\) as of this writing is ~~v3.3.7~~  v3.3.8, but **all versions of FG "Classic" should work in the same way**.



As a Game Master, **hosting a virtual table** implies that the table \(= actually the GM's FG program\) has to be **reachable from the the players**' FG programs running on distant computers.

Fantasy Grounds \("Classic"\) uses **by default TCP port 1802 on IPv4** for communications between the GM and player Fantasy Grounds instances.

So, to join a GM's virtual table, **a player's FG only has to know/find the GM's IP address** where it will be able to connect to \(on the TCP 1802 port\).



The target IP\(v4\) address used will be one of :

1. the **GM's "public IP"** address for players connecting **through the Internet**
2.  the **GM's "private IP"** address for players located **in the same local network**

{% hint style="warning" %}
Be aware of the above **distinct requirements for players connecting from the Internet vs. from the in-house network** \(same ethernet LAN or wireless access point\).
{% endhint %}

## What can go wrong ?

There are multiple points where security features or technical niceties may prevent incoming connections from players to your GM FG program.

Network setup combinations are infinite; in this document we focus on the usual "home" network with a single "Internet router" \(or a cascade of 2 devices\) connecting all your personal devices to the Internet \(PC/Mac, Phone, Tablet, SmartTV,...\).

So let's have a quick look at the barriers between your players and your shiny new virtual table, starting from your own "GM Fantasy Grounds" outwards; with "Barrier 4" we'll go into the "**Port Forwarding**" subject many GMs are afraid of.

Then we'll talk about some no-go situations that relate to the last 3 barriers and are "out of reach" for the GM.

And ~~finally~~ we'll introduce **VPN solutions** and more specifically the non-intrusive "transparent" setup we present in the rest of this document, and how it enables a GM to overcome these barriers **when port forwarding is not an option**.

{% hint style="info" %}
**Added sections will then go into more advanced setups for FGC and/or FGU :**

* [Lifting the "1 GM at-a-time" restriction](../simultaneous-games-fgc-1-vpn-server/untitled.md)
* [FGU vs. FGC networking](../fgu-fantasy-grounds-unity/fgu-vs.-fgc-networking.md)
* [Why not support both ?](../simultaneous-fgc-fgu-games-1-vpn-server/untitled.md)
{% endhint %}

{% hint style="success" %}
You can **test the first 3 "barriers" locally with 2** _**different**_ **machines** : one being the GM sharing the virtual table, and the other acting as a "local" player : a player on a _different machine than the GM's, but connected to the same LAN or WLAN._

A demo version of FG on the second machine is ok for this kind of test.
{% endhint %}

{% hint style="warning" %}
If a test "Player" FG on a second machine in your local network is unable to connect to your "GM" FG, then **begin with clearing the first 3 barriers**, because each of these is also a show stopper for remote player connections.
{% endhint %}



