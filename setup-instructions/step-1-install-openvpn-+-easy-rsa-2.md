# Step 1 : Install OpenVPN + Easy-RSA

As exposed in the [Tools](../proposed-solution/tools.md) section, we absolutely need 2 pieces of software :

## OpenVPN \(+ Easy-RSA 2\)

**OpenVPN is an "SSL/TLS-based VPN" that is very mature and very widely used all over the multiverse.**

> **Tech Blurb**
>
> **OpenVPN is NOT one of those old "SSL VPNs" running in a web browser, but uses SSL/TLS cryptography natively.**  It operates at the **transport layer** on any unused **TCP or UDP** port, is **"NAT-friendly"**, ****and uses a **single port** \(contrarily to protocols like the old PPTP with TCP 1723 + GRE, or IPSEC with ESP + AH + IKE\)
>
> For VPNs and network tunnels in general, **it is always best to choose the UDP transport** layer protocol, **rather than TCP.**  _See this excellent, old but still valid, article :_ [_Why TCP Over TCP Is A Bad Idea_](http://sites.inka.de/bigred/devel/tcp-tcp.html) _by the creator of_ [_CIPE_](http://sites.inka.de/~bigred/devel/cipe.html)_, who moved later on to OpenVPN_
>
> But **in case you absolutely need it**, you can run OpenVPN even on TCP 80 or 443 \(web server ports\), if unused on your server machine : if you happen to be _on a network with very strict Internet access rules, you may have to resort to that_.
>
> However in this setup we'll use the **official OpenVPN port** : **UDP 1194**.

**Easy-RSA 2 is a set of command-line utilities to create/manage Public Key Infrastructures \(PKIs\)**

_Easy-RSA 2 is developed by the same team as OpenVPN._

Windows installation packages for OpenVPN include Easy-RSA 2.

I usually try to keep a complete copy of Easy-RSA 2 directly in the "safe" folders where I create PKIs because :

* my laptop is not supposed to host a VPN server
* all my PKIs are "off-line" PKIs for remote OpenVPN servers and i prefer a safer place than the Windows system drive...
* i can edit the default settings specifically for each PKI and keep these customized files

I encourage you to do the same :

create a "new-CA" subdirectory in your safe place

copy the easy-rsa subdirectory from

