# Our own "transparent" VPN-based Port Forwarding

We already gave the salient points about the kind of setup we propose in the [previous paragraph](https://zeferby.gitbook.io/transparent-openvpn-for-fantasy-grounds/general/vpn-solutions#your-own-1-gm-only-vpn-server-based-on-industry-standards), so let's have a closer look at the Why's and How's...

## Objectives

### Primary objective

{% hint style="success" %}
#### For 1 GM, ability to host FG games even in a "NO-GO situation"
{% endhint %}

Relying on **a hosted virtual machine which can accept TCP 1802** incoming connections from the Internet, **forward "transparently" all TCP 1802 traffic to and from the GM's machine** on a VPN-secured private connection.

### Secondary objectives

* **do NOT require a VPN connection on the players side** : by handling all port forwarding aspects on a single "point of contact" \(the VPN server for the GM is also the GM's FG address as seen from the players\)
* provide the **least possible "intrusiveness" to the GM's machine configuration**
* **minimize costs for the GM** to $0 or at most &lt; $10/year
* introduce **NO dependancy for the GM on any external company/organization** \(except of course the need of virtual server hosting\)

### Other benefits

* **most possible versatile solution** : OpenVPN, when you have administrative of both the VPN server and client, enables you to taylor the solution according to you exact wishes
* "owned" security : **You have absolute authority** on your whole setup and **you don't need to "share" any security info with anyone**
* fully "destroyable" : **you can easily get rid** of your whole VPN infrastructure at any time
* **no "permanent route" added** : can be used on machines with complex network settings without impacting the other connections

### Drawbacks

* **Initial setup effort** : we'll mitigate that with step-by-step instructions on the whole process
* **FG Alias and "Connection test" will not work** \(like most other VPN-based solutions\) : you'll have to either rely on DNS or give the public IP of your VPN server to your players instead of an alias



## Implementation

In the following steps, we'll use a Windows 7 machine \(I managed to avoid W10 until now...\) to setup our 1-GM VPN.

We'll setup :

* **a Certificate Authority** on your own machine \(to keep the core security off-line from the VPN server itself\) with which you'll manage the certificates identifying the VPN server and the VPN clients. _Actually you'll probably need only one client certificate : your own GM FG machine_
* **a private AWS storage space \(called an "AWS S3 bucket"\)**, to store some security parameters and startup scripts for the VPN server
* a small **AWS virtual server to run OpenVPN \(server mode\) and TCP 1802 port forwarding**, **auto-configured at startup \(**using the security parameters and startup script files stored in the S3 bucket\)

{% hint style="success" %}
Keeping parameters and scripts in S3 **enables you to destroy \(AWS calls that "terminate"\) and re-create your virtual server at will.**
{% endhint %}

{% hint style="info" %}
That helps **bringing the running costs down to nearly $0 even if you are outside of the AWS free tier bounds**, and the server startup delay \(create vs. start\) is very small.

If you prefer, you can also **start/stop the virtual server**, keeping its "hard disk" storage between multiple uses instead of having it destroyed, but if you are outside of the AWS free tier you may incurr a small monthly cost, and this server should not store any data anyway, so...
{% endhint %}



## Tools

We'll use several tools during the setup process; OpenVPN + EasyRSA are mandatory and the rest is very helpful.

### Open VPN Community \(free\)

[OpenVPN](https://openvpn.net/) now has commercial offers but we'll use the **free** [**Community**](https://openvpn.net/community/) **edition** for our "do it yourself" installation.

You can select and download the latest version \(v2.4.7 as of this writing\), corresponding to your own OS, here : [https://openvpn.net/community-downloads/](https://openvpn.net/community-downloads/)

![](../.gitbook/assets/image%20%288%29.png)

### Easy-RSA \(free\)

Easy-RSA is also managed by the OpenVPN team; it is the tool we'll use to create our **PKI** \(**Public Key Infrastructure**\) with its **CA** \(**Certificate Autority**\), to create and manage our **Digital Certificates.**

**Easy-RSA 2 should already be included in OpenVPN Windows installers**, but here is it's GitHub download location : [https://github.com/OpenVPN/easy-rsa-old/releases](https://github.com/OpenVPN/easy-rsa-old/releases)

_For Linux-like environments, Easy-RSA 3 can be downloaded from :_ [_https://github.com/OpenVPN/easy-rsa/releases_](https://github.com/OpenVPN/easy-rsa/releases)\_\_

### Optional tools

#### CloudBerry Explorer for Amazon S3 \(free version\)

CBE for S3 is a very useful tool to **transfer files between your own machine and your Amazon S3 buckets**, with a familiar Windows Explorer -like GUI : 

![CloudBerry Explorer for Amazon S3 : GUI](https://www.cloudberrylab.com/img/modules/product-tour/explorer/amazon-s3/2.png)

{% hint style="info" %}
This tool has a free version and a commercial version; however, **only the commercial version supports encrypted S3 buckets**, so **we'll do without encrypted buckets** \(at least at first\).
{% endhint %}

You can grab the software here : [https://www.cloudberrylab.com/explorer/amazon-s3.aspx](https://www.cloudberrylab.com/explorer/amazon-s3.aspx)

#### PuTTY \(free\)

This tool should not be required, but **if you want to open a command line terminal to your Linux VPN server**, then PuTTY is the way to go for the Windows platform.

PuTTY is available from : [https://www.putty.org/](https://www.putty.org/)

![](../.gitbook/assets/image%20%285%29.png)



## AWS : Amazon Web Services





