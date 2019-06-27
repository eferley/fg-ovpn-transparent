# VPN solutions

{% hint style="success" %}
Thankfully, if you find yourself in a NO-GO situation, and **as long as your can establish outgoing connections** to the Internet, **you should be able to use a VPN-based solution**.
{% endhint %}

There are **multiple commercial offers for VPN \(=Virtual Private Network\) services** available nowadays.

Many of them are targetted at : either hiding your own public IP address, or gaining full access to the Internet in countries where the government imposes harsh restrictions.

Others provide one or multiple kinds of "real" VPNs functionnality like "closed VPNs".

**Some of them** \(like [Hamachi](https://www.vpn.net/), which is well known in the gaming sphere\) **provide a free tier**.

{% hint style="info" %}
I have not actively tested commercial VPN offers other than Hamachi.

Hamachi is a very neat solution for "closed" VPNs with multiple options for network setup and it supports Windows, Mac, Linux and mobile platforms.

Their free tier is very good and quite sufficient for FG-like requirements \(you can host 4 players in a free VPN and create multiple VPNs simultaneously for more players if needed\).

If you want to try out Hamachi, you'll find them at : [https://www.vpn.net/](https://www.vpn.net/)

**Be sure to read the Getting Started document** available at : [https://secure.logmein.com/welcome/documentation/EN/pdf/Hamachi/LogMeIn\_Hamachi\_GettingStarted.pdf](https://secure.logmein.com/welcome/documentation/EN/pdf/Hamachi/LogMeIn_Hamachi_GettingStarted.pdf)
{% endhint %}

## "Closed" VPNs

Some offers \(like the free Hamachi offer\) enable setting up a "closed" VPN where **only members of the VPN created by the GM are able to connect** to the GM's machine \(or to each other, depending on the networking options you choose\).

{% hint style="info" %}
"Closed" VPNs are actually what VPN technology was initially designed for.
{% endhint %}

My personnal preference, both as a GM and as a player, is to avoid using that kind of offer, because :

* **it implies that all players install the same VPN software and setup their access to the specific game or GM VPN**
* _for Hamachi specifically_ : it maintains a permanent set of networking parameters in my machine, even when not in active use, that sometimes interfere with my own \(rather complex\) network settings: i am often connected to 2 or 3 other VPNs at a time for work, and my laptop also supports local networks for virtual machines all the while...
* I have my own VPN servers 🤣 

But "**closed" VPNs are a very valid option for a GM** :

* you can select the players who will be able to access your game by including / excluding people from your own VPN\(s\), or close/create new VPNs at will 
* with some "network topologies" \(like the Hamachi "hub-and-spoke"\) you can maintain network isolation between players \("spokes"\), enabling them to reach only the "hub" \(your GM FG machine\)

## Other VPN offers

Given the new popularity of VPN services, **this is a battlefield** ! Just google "VPN Services" to find out...

{% hint style="warning" %}
Be cautious about several points :
{% endhint %}

* subscribing to any VPN service offer implies _**some trust**_ from you in the service company...
* VPN service offers **do not mandatorily have an available/reliable free tier**
* VPN service offers **do not mandatorily support forwarding TCP 1802**

## Your own 1 GM-only VPN server based on industry standards

What we propose in the following sections is :

{% hint style="success" %}
**Creating your own small VPN server to "break barriers 4-6"**
{% endhint %}

* based on **SSL/TLS with** [**OpenVPN Community**](https://openvpn.net/community/) edition
* hosted on a **minimalist Linux virtual server at** [**AWS**](https://aws.amazon.com/) \(Amazon Web Services\)
* **mono-client** : 1 GM \(you\) only
* with pre-installed **FG-only port forwarding** \(this is actually independant of the VPN itself\)
* for a **cost of $0 \(or a neglectable cost** if you are not elligible for the free tier or already exceed its bounds due to other usage of AWS\)
* **"transparent" for the players**
* **non-intrusive for the GM's machine**
* **100% under your own control** : **you** are the VPN service provider, and you are your **only** client 😁 

It does require some initial setup effort, which then makes it a no-brainer to start/stop/destroy/recreate **your very own Amazing Port-Forwarding OpenVPN-Powered Fantasy Grounds Hosting solution** 🙃 

{% hint style="info" %}
If you are tech-savvy and want to discover more/advanced options, the **OpenVPN + Linux iptables combination has nearly endless possibilities**.

I have used **OpenVPN** since 2004 both for professional and personal usage in various server and/or client configurations \(with some machines being both multi-servers and multi-clients\), on Linux/Windows/Android, and it is **to this day by far my preferred VPN foundation**, including for mobile platforms.

Quite a number of commercial VPN offers either rely on, or support OpenVPN.

If you want to know more about OpenVPN, read the Howto at [https://openvpn.net/community-resources/how-to/](https://openvpn.net/community-resources/how-to/) and more generally browse the useful docs and articles available at [https://openvpn.net/community-resources/](https://openvpn.net/community-resources/)
{% endhint %}

{% hint style="danger" %}
Remember that _whatever your network setup is_, **you still have to check your GM FG machine for barriers 1-3 !**
{% endhint %}

