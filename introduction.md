---
description: >-
  Overall description of the "transparent" VPN solution and why it may be needed
  depending on GM situation.
---

# Introduction

## So, what is the problem ?

### Fantasy Grounds "Classic" communications

_This document focuses on FG "Classic"_ \(as opposed to the future release of FG "Unity", currently planned for the end of 2019\).

The current release of FG \("Classic"\) as of this writing is v3.3.7 \(for the "Live" branch, v3.3.8 for the "Test" branch\), but **all versions of FG "Classic" should work in the same way**.

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

## What can go wrong ?

There are multiple points where security features or technical niceties may prevent incoming connections from players to your GM FG program.

So let's have a quick look at all the barriers between your players and your shiny new virtual table, starting from your own GM Fantasy Grounds outwards, and with "Barrier 4" we'll go into the "**Port Forwarding**" subject many GMs are afraid of.

{% hint style="info" %}
This can also be used as a reminder / check-list for every GM out there...
{% endhint %}

{% hint style="success" %}
You can **test the first 3 "barriers" locally with 2** _**different**_ **machines** : one being the GM sharing the virtual table, and the other acting as a "local" player : a player on a _different machine than the GM's, but connected to the same \(W\)LAN._

A demo version of FG on the second machine is ok for this kind of test.
{% endhint %}

Then we'll talk about some no-go situations that relate to the last 2 barriers and are "out of reach" for the GM.

And finally we'll introduce VPN solutions and more specifically the non-intrusive "transparent" setup we present in the rest of this document, and how it enables a GM to overcome these barriers.

## GM's machine barriers

These first 3 points relate to the configuration of the GM's machine/OS :

### Barrier 1 😡 : Windows 10 "public/private" network setting

Windows 10 has a setting to classify each network it is connected to as either "pubic" \(read : "not safe"\) or "private" \(read: "safe"\) and W10 will prevent incoming connections for "public" networks, including Fantasy Grounds, so be sure to 👍 **tag your current W10 network connection as "public"**, or else... 👎 

_**TODO : W10 screenshots needed here**_

### Barrier 2 👿 : OS-managed firewall

Be sure to 👍 **allow incoming communications to your TCP 1802, or better yet to your Fantasy Grounds** software in your operating system-integrated firewall or else... 👎 

_Here is an example of Windows 7 firewall rules authorizing **all** types of incoming connections to the Fantasy Grounds program, with TCP+UDP protocols and the "ALL" value for networks, adresses and ports parameters \(W7 Firewall "advanced" view\) :_

![W7 &quot;All Incoming&quot; rules example for F.G. ](.gitbook/assets/image%20%281%29.png)

_**TODO : Other screenshots needed here**_

### Barrier 3 👹 : Third-party protection software

If you are using a third-party anti-malware, anti-virus or any other kind of anti-something protection software, chances are you also need to 👍 **enable incoming communications for FG in these**, or else... 👎 

## Internet connection barriers

The next barriers are related to the general subject of "IP address sharing", generally implemented at NAT/PAT on Internet end-user routers : multiple devices in your home must have access to the Internet through a single Internet connection device, which only has a single "public IP" address.

### Barrier 4 👺 : Crossing the NAT/PAT frontier on the Internet router



### Barrier 5 💩 : Cascading routers



## Some NO-GO Situations



## VPN solutions



## Non-intrusive VPN-based Port Forwarding





