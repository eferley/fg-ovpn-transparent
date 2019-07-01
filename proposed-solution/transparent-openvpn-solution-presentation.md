# "Transparent" OpenVPN-based Port Forwarding

We already gave the salient points about the kind of setup we propose in the [previous paragraph](https://zeferby.gitbook.io/transparent-openvpn-for-fantasy-grounds/general/vpn-solutions#your-own-1-gm-only-vpn-server-based-on-industry-standards), so let's have a closer look at the Why's and How's...

_**We'll make our way SLOWLY, and I'll annoyingly insist on the security aspects, so that you don't inadvertently create security "holes" in your own setup or risk to break it down.**_

## Objectives

### Primary objective

{% hint style="success" %}
#### For 1 GM, ability to host FG games even in a "NO-GO situation"
{% endhint %}

Relying on **a hosted virtual machine which can accept TCP 1802** incoming connections from the Internet, **forward "transparently" all TCP 1802 traffic to and from the GM's machine** on a VPN-secured private connection **across barriers 4-6.**

### Secondary objectives

1. **do NOT require a VPN connection on the players side** : by handling all port forwarding aspects on a single "point of contact" \(the VPN server for the GM is also the GM's FG address as seen from the players\)
2. be the **most "non-intrusive" possible vs. the GM's machine configuration**
3. **minimize costs for the GM** to $0 \(or a neglectable amount, if you are out of the AWS free tier bounds\)
4. introduce **NO dependancy for the GM on any external company/organization** \(except of course the need of virtual server hosting\)
5. provide a **mostly automated startup process**

### Other benefits

* **most possible versatile solution** : OpenVPN, since you have administrative access on the VPN server, enables you to taylor the solution according to you exact wishes
* "owned" security : **You have absolute authority** on your whole setup and **you don't need to "share" any security info with anyone**
* fully "destroyable" : **you can easily get rid** of your whole VPN infrastructure at any time
* **no "permanent route" added on the GM's machine** : can be used on machines with complex network settings without impacting the other connections

### Drawbacks

* **Initial setup effort** : we'll try to mitigate that with step-by-step instructions on the whole process
* **FG Alias and "Connection test" will not work** \(like most other VPN-based solutions\) : you'll have to either rely on DNS, or give the public IP of your VPN server to your players, instead of an FG alias
* **1 GM-at-a-time** : with this setup having **only 1 public IP address**, only **1 GM at a time can be the target of the port forwarding rule** \(using such a server for multiple GMs, still 1 at a time, can be done with "time sharing", using any kind of booking/scheduling, but it still remains "1-at-a-time"\)



## Implementation

### What we'll setup

* **a Certificate Authority \(CA\) on your own machine** to have **full control of security**, with which you'll manage the certificates+keys identifying the VPN server and VPN clients. _Actually you'll probably need only one active client certificate, for your own GM FG machine_
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

Using a **Public Key Infrastructure \(PKI\) with its own Certificate Authority \(CA\) is the best option for security**.  It adds some overhead to our setup process, but it brings many advantages...

If you are wary about security issues, read this excerpt from the OpenVPN web site "How-To" at :   
[https://openvpn.net/community-resources/how-to/\#setting-up-your-own-certificate-authority-ca-and-generating-certificates-and-keys-for-an-openvpn-server-and-multiple-clients](https://openvpn.net/community-resources/how-to/#setting-up-your-own-certificate-authority-ca-and-generating-certificates-and-keys-for-an-openvpn-server-and-multiple-clients)

> ### SETTING UP YOUR OWN CERTIFICATE AUTHORITY \(CA\) AND GENERATING CERTIFICATES AND KEYS FOR AN OPENVPN SERVER AND MULTIPLE CLIENTS <a id="setting-up-your-own-certificate-authority-ca-and-generating-certificates-and-keys-for-an-openvpn-server-and-multiple-clients"></a>
>
> #### Overview <a id="overview"></a>
>
> **The first step** in building an OpenVPN 2.x configuration is to **establish a PKI** \(public key infrastructure\). The PKI consists of:
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

We'll take advantage of these benefits to keep the core security off-line from the VPN server itself : we'll **create the PKI entirely on your own machine**, in a **"safe place" of your choice**, and **only put the minimum required certificates on the VPN server and client\(s\)**.

Building the PKI will be done with **Easy-RSA 2** which should come bundled with OpenVPN \(see [Tools](https://zeferby.gitbook.io/transparent-openvpn-for-fantasy-grounds/our-openvpn-based-solution/tools)\).

{% hint style="info" %}
You can backup or transfer the contents of your PKI folder to another storage after it is complete.  I find it convenient to keep it in my machine in case i need to quickly disable some certificates to deny acces to an existing certificate
{% endhint %}



### The AWS S3 Bucket

We'll create this "bucket", which is **simply a storage space**, on the AWS S3 service.  It can done either with the AWS management console, or with CloudBerry Explorer for S3 \(see [Tools](tools.md)\).

Some **important security parameters will be stored there**, so make sure **NOT** to check any option to make it publicly accessible !  _The AWS management console shows some clear warnings when a bucket content is "at risk"_.

{% hint style="danger" %}
**Keep this S3 storage "bucket" PRIVATE !**
{% endhint %}

{% hint style="warning" %}
If you want to manage the contents of your S3 bucket with the **free version of CloudBerry Explorer for S3** \(see [Tools](tools.md)\), you must **not enable encryption** on your bucket.
{% endhint %}

{% hint style="info" %}
If you manage your S3 bucket contents with a **licenced version of CloudBerry Explorer for S3, and/or** with the **AWS management console, you can use encryption**.
{% endhint %}

#### **What will the S3 bucket contain ?**

* startup scripts for the AWS Linux/OpenVPN server
* your own OpenVPN server configuration options
* the minimal amount of security parameters, ⚠ _**including the OpenVPN server "public certificate" and "private key"**_ ⚠ so, once again, in case you didn't hear me \(ICYMI\), make sure that you...

{% hint style="danger" %}
**&lt;shout mode on&gt; KEEP THIS S3 STORAGE "BUCKET" PRIVATE !!! &lt;shout mode off&gt;**
{% endhint %}

🧐 **You've been warned... Don't come back crying...** 😭 

{% hint style="warning" %}
If you want to store **other data in S3**, please **create a second private S3 bucket** for that !
{% endhint %}



### OpenVPN on an AWS Linux virtual server

This virtual server will be hosted at Amazon Web Services, a.k.a. AWS.

**Check the** [**Amazon Web Services**](amazon-web-services.md) **section for an overview of what that means.** Some quick facts :

{% hint style="success" %}
If you have a new AWS account you'll be able to **run a small server for free 24x7 for one year** from account creation thanks to the **AWS Free Tier**.
{% endhint %}

#### **If you venture out of the AWS Free Tier bounds** :

If your AWS account is older than 1 year or you have multiple AWS servers running, whatever...

**Then you will incurr small running costs for your server and storage, but this solution is designed so that these costs are minimal, and probably neglectable for most.**

Choosing the **cheapest AWS server type**, which **is more than adequate** for that task :

* **if you need to run it 24x7, which is VERY unlikely**, it can cost you in the **range of $5 to $10 /month, depending on the AWS region** of the world where you run it.
* if you want to host games for **3x 4H a week**, which gives around 50 H/month, the cost should be in the **range of $0.3-$0.6 /month, depending on the region**  _\(I don't know if AWS bothers to produce invoices for such low amounts...\)_



#### We'll create an AWS virtual server :

* using a "free tier elligible" type of server
* to run the basic "Amazon Linux AMI 2018.xx.y" distribution
* giving it permission to access the S3 bucket for the parameters and scripts it needs
* giving it network access rules to accept incoming connections both for OpenVPN \(for you, the GM\) and for FG traffic \(for the players\)
* giving it a small bootstrap script that will do all the OpenVPN \(+some tools +port forwarding\) installation and configuration work for you

{% hint style="success" %}
After 2 or 3 minutes, when the server "**Instance state**" is "**running**" and "**Status Checks**" shows the green "**passed**" tick in the AWS management console, you can enjoy !
{% endhint %}



