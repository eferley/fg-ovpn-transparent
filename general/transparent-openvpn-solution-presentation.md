# Non-intrusive, "transparent" VPN-based Port Forwarding

We already gave the salient points about the kind of setup we propose in the previous paragraph, so let's have a closer look at the Why's and How's...

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

### Drawbacks

* **Initial setup effort** : we'll mitigate that with step-by-step instructions on the whole process
* **FG Alias and "Connection test" will not work** \(like most other VPN-based solutions\) : you'll have to either rely on DNS or give the public IP of your VPN server to your players instead of an alias



## Implementation

You will build :

* **a Certificate Authority** on your own machine \(to keep the core security off-line from the VPN server itself\) with which you'll manage the certificates identifying the VPN server and the VPN clients. _Actually you'll need only one client certificate : your own GM FG machine_
* **a private AWS storage space \(called an "AWS S3 bucket"\)**, to store some security parameters and startup scripts for the VPN server
* a small **AWS virtual server to run the VPN and TCP 1802 port forwarding**, **auto-configured at startup \(**using the security parameters and startup script files stored in the S3 bucket\)

{% hint style="success" %}
This technique \(keeping parameters and scripts in S3\) **enables you to destroy \(AWS calls that "terminate"\) and re-create your virtual server at will.**
{% endhint %}

{% hint style="info" %}
That helps **bringing the running costs down to nearly $0 even if you are outside of the AWS free tier bounds**, and the server startup delay is neglectable.

If you prefer, you can also **start/stop the virtual server**, keeping its "hard disk" storage between multiple uses instead of having it destroyed, but if you are outside of the AWS free tier you may incurr a small monthly cost, and this server should not store any data anyway, so...
{% endhint %}









