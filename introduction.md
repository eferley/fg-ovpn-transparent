---
description: >-
  Overall description of the "transparent" VPN solution and why it may be needed
  depending on GM situation.
---

# Introduction

## Fantasy Grounds \("Classic"\) communications

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

Network setup combinations are infinite; in this docment we focus on the usual "home" network with a single "Internet router" \(or a cascade of 2 devices\) connecting all your personnal devices to the Internet \(PC/Mac, Phone, Tablet, SmartTV,...\).

So let's have a quick look at the barriers between your players and your shiny new virtual table, starting from your own "GM Fantasy Grounds" outwards; with "Barrier 4" we'll go into the "**Port Forwarding**" subject many GMs are afraid of.

Then we'll talk about some no-go situations that relate to the last 2 barriers and are "out of reach" for the GM.

And finally we'll introduce VPN solutions and more specifically the non-intrusive "transparent" setup we present in the rest of this document, and how it enables a GM to overcome these barriers.

{% hint style="info" %}
This can also be used as a reminder / check-list for every GM out there...
{% endhint %}

{% hint style="success" %}
You can **test the first 3 "barriers" locally with 2** _**different**_ **machines** : one being the GM sharing the virtual table, and the other acting as a "local" player : a player on a _different machine than the GM's, but connected to the same \(W\)LAN._

A demo version of FG on the second machine is ok for this kind of test.
{% endhint %}

{% hint style="warning" %}
If a test "Player" FG on a second machine in your local network is unable to connect to your "GM" FG, then first clear the first 3 barriers, because each of these is also a show stopper for remote player connections.
{% endhint %}

## GM's machine barriers

These first 3 points relate to the configuration of the GM's machine/OS.

_**Breaking these barriers** should be enough to **successfully host a game for players** located **in your own home network**, even if you are in a no-go situation for remote player access._

### Barrier 1 😡 : Windows 10 "public/private" network setting

Windows 10 has a setting to classify each network it is connected to as :

* either "pubic" \(read : "not safe"\)
* or "private" \(read: "safe"\)

Windows 10 will prevent incoming connections from "public" networks, including Fantasy Grounds, so be sure to 👍 **tag your current W10 network connection as "private"**, **or else...** 👎 

_**TODO : W10 screenshots needed here**_

### Barrier 2 👿 : OS-managed firewall

Be sure to 👍 **allow incoming communications to your TCP 1802, or better yet** 👍👍 **to your Fantasy Grounds executable program in the operating system firewall**, **or else...** 👎 

_Here is an example of Windows 7 firewall rules authorizing **any kind of incoming connection** to the Fantasy Grounds program, with TCP+UDP protocols and the "ALL" value for networks, adresses and ports parameters \(W7 Firewall "advanced" view\) :_

![W7 &quot;All Incoming&quot; rules example for F.G. ](.gitbook/assets/image%20%281%29.png)

_**TODO : Other screenshots needed here**_

### Barrier 3 👹 : Third-party protection software

If you are using any kind of  third-party anti- malware/virus/threat protection software, chances are that you also need to 👍 **enable incoming communications for FG in these**, **or else...** 👎 

_**TODO : Other screenshots needed here**_

## Internet connection barriers

The last 2 points relate to your Internet connection itself.

{% hint style="info" %}
**Most GMs** have a single "Internet router" and **do not have to bother about Barrier 5**
{% endhint %}

### Barrier 4 👺 : Crossing the NAT/PAT frontier on the Internet router

Most home Internet connections are delivered through a single all-in-one device we'll call an "Internet router", which actually does several jobs at once :

* **Modem** : it adapts an electrical or optical signal on your ISP line to and from network traffic on your side \(_depending on installations, this may actually be done by a secondary "adapter box" located just between your Internet router and the ISP line itself_\)
* **WIFI access point** : it enables wireless connection for your devices \(PC/Mac, Tablet, Phone, SmartTV, etc\)
* **Router** : it routes network traffic between your own private "in-home" network space to and from the Internet
* Other roles :
  * **DHCP server** : it assigns private IP addresses to connected devices on your internal network \(plus a minimal set of networking parameters\)
  * **DNS forwarder** : it helps your connected devices getting DNS names \(like [fantasygroundscollege.net](https://fantasygroundscollege.net) or [www.fantasygrounds.com](https://www.fantasygrounds.com)\) resolved into public IP addresses
  * depending on the model, it may also provide **other services** like file or printer sharing, etc...

The critical element here are the **Router** and **DHCP Server** features of your "Internet router" : 

* when your devices are connected to it, it's **DHCP Server** feature delivers **one PRIVATE IP address to each device** on your home network; _your GM FG machine is one of those_.
* when the "Internet router" is connected to your ISP / the Internet, the **Router** has a **single PUBLIC IP address**
* the **Router** then uses something called NAT \(Network Address Translation\) or more specifically PAT \(Port-Address Translation\) to **"share" it's single public IP** for all your devices communicating through it with the Internet

So **ALL your devices**, when communicating with the Internet via the Router, are seen as the same **single PUBLIC IP address** as the Router itself, and **your FG machine** \(the GM's one\) **is only one of many** such devices "using" the same public IP.

When an incoming communication request comes in from a remote FG player, the Router needs to know specifically where to route that request \(i.e. to your GM FG machine, not to your SmartTV ! 😛 \); otherwise the Router will just ignore the communication request.

#### This is where **Port Forwarding** comes into play !

**You have to** **give the Router a rule to forward TCP 1802 packets coming in on the single public IP from the Internet to redirect them to your GM FG machine, on its private IP, still on port TCP 1802**.

It means you usually have to :

* enter the administration/setup facility of your "Internet router" \(and you probably need the proper administrative username + password for that\)
* locate the Port Forwarding features, which may be called by many different names depending on the device brand;  you will generally find them under a "NAT" or "PAT" or "Advanced" section...
* enter a rule for Fantasy Grounds with basically :
  * \(potentially\) a name
  * the public / external port : **TCP 1802** _This is the port on which the Router will "listen" for connection requests incoming from remote FG players_
  * a destination : either choose from a list of devices, or enter an IP address _**This is your GM FG machine \(private IP\) !**_
  * a destination / private / internal port : **TCP 1802** _This is the port of your GM FG machine on which FG listens to incoming player connections_
* for some models you also will have to "Save the configuration"

A large info repository on many "Internet router" brands/models is available at [https://portforward.com/router.htm](https://portforward.com/router.htm) where you may find more step-by-step instructions for your own specific model. _**Just be wary of all the links inciting you to purchase tools and/or subscriptions there !**_



{% hint style="warning" %}
**If your home network has Internet access via 2 cascaded routers, also check "Barrier 5".**
{% endhint %}



### Barrier 5 💩 : Cascaded devices



## Some NO-GO Situations



## VPN solutions



## Non-intrusive VPN-based Port Forwarding





