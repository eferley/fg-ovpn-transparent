# "Transparent" OpenVPN-based Port Forwarding

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
* **1 GM-at-a-time** : with this setup, having **only 1 public IP address**, only 1 GM at a time can be the target of the port forwarding rule \(using such a server for multiple GMs can be done with some kind of bookings/schedule\)



## Implementation

In the following steps, we'll use a Windows 7 machine \(I managed to avoid W10 until now...\) to setup our port forwarding VPN.

We'll setup :

* **a Certificate Authority** on your own machine to have full control of security, with which you'll manage the certificates identifying the VPN server and the VPN clients. _Actually you'll probably need only one client certificate for your own GM FG machine_
* **a private AWS storage space \(called an "AWS S3 bucket"\)**, to store some security + booting parameters and startup scripts for the VPN server
* a small **AWS virtual server to run OpenVPN \(server mode\) and TCP 1802 port forwarding**, **auto-configured at startup \(**using the parameters and startup script files stored in the S3 bucket\)

**Then you'll be happily GMing games with Fantasy Grounds !** 😁 

{% hint style="success" %}
Keeping parameters and scripts in S3 **enables you to destroy \(AWS calls that "terminate"\) and re-create your virtual server at will.**
{% endhint %}

{% hint style="info" %}
That helps **bringing the running costs down to nearly $0 if you are outside of the AWS free tier bounds**, and the server startup delay \(create vs. start\) is very small.

If you prefer, you can also **start/stop the virtual server**, keeping its "hard disk" storage between multiple uses instead of having it destroyed, but if you are outside of the AWS free tier you may incurr a small monthly cost, and this server should not store any data anyway, so...
{% endhint %}



### The CA / PKI

Using a **Public Key Infrastructure is the best option for security**.  It adds a bit of overhead to our setup process, but it brings so many advantages...

To quote from the OpenVPN web site "How-To" at :   
[https://openvpn.net/community-resources/how-to/\#setting-up-your-own-certificate-authority-ca-and-generating-certificates-and-keys-for-an-openvpn-server-and-multiple-clients](https://openvpn.net/community-resources/how-to/#setting-up-your-own-certificate-authority-ca-and-generating-certificates-and-keys-for-an-openvpn-server-and-multiple-clients)

> ### SETTING UP YOUR OWN CERTIFICATE AUTHORITY \(CA\) AND GENERATING CERTIFICATES AND KEYS FOR AN OPENVPN SERVER AND MULTIPLE CLIENTS <a id="setting-up-your-own-certificate-authority-ca-and-generating-certificates-and-keys-for-an-openvpn-server-and-multiple-clients"></a>
>
> #### Overview <a id="overview"></a>
>
> The first step in building an OpenVPN 2.x configuration is to establish a PKI \(public key infrastructure\). The PKI consists of:
>
> * a separate certificate \(also known as a public key\) and private key for the server and each client, and
> * a master Certificate Authority \(CA\) certificate and key which is used to sign each of the server and client certificates.
>
> OpenVPN supports **bidirectional authentication based on certificates**, meaning that the client must authenticate the server certificate and the server must authenticate the client certificate before mutual trust is established.
>
> Both server and client will authenticate the other by first verifying that the presented certificate was **signed by the master certificate authority \(CA\)**, and then by testing information in the now-authenticated certificate header, such as the certificate common name or certificate type \(client or server\).
>
> This security model has a number of desirable features from the VPN perspective:
>
> * **The server only needs its own certificate/key** — it doesn’t need to know the individual certificates of every client which might possibly connect to it.
> * **The server will only accept** clients whose **certificates were signed by the master CA certificate** \(which we will generate below\). And because the server can perform this signature verification without needing access to the CA private key itself, it is **possible for the CA key \(the most sensitive key in the entire PKI\) to reside on a completely different machine**, even one without a network connection.
> * **If a private key is compromised, it can be disabled** by adding its certificate to a CRL \(certificate revocation list\). The CRL allows compromised certificates to be selectively rejected **without requiring that the entire PKI be rebuilt**.
> * The server can enforce client-specific access rights based on embedded certificate fields, such as the Common Name.
>
> _Note that the server and client clocks need to be roughly in sync or certificates might not work properly._

We'll take advantage of these benefits to keep the core security off-line from the VPN server itself : we'll **create the PKI entirely on your own machine**, in a "safe place" of your choice, and **only put the minimum required certificates on the VPN server and client\(s\)**.

All of that will be done with Easy-RSA 2 which should come bundled with OpenVPN \(see Tools\)

### The AWS S3 Bucket



### OpenVPN on an AWS Linux virtual server





