# Step 1 : Install OpenVPN + Easy-RSA

As exposed in the [Tools](../proposed-solution/tools.md) section, we absolutely need 2 pieces of software :

## OpenVPN \(+ Easy-RSA 2\)

**OpenVPN is an "SSL/TLS-based VPN" that is very mature and very widely used** _**all over the multiverse**_**.**

> **Tech Blurb**
>
> **OpenVPN is NOT one of the old "SSL VPNs" running in a web browser, but uses SSL/TLS cryptography natively.**  It uses the **transport layer on any unused TCP or UDP port**, is **"NAT-friendly"**, ****and uses a **single protocol+port** \(contrarily to protocols like the old PPTP with TCP 1723 + GRE, or IPSEC with ESP + AH + IKE\)
>
> For VPNs and network tunnels in general, **it is always best to choose the UDP transport** layer protocol, **rather than TCP.**  _See this excellent, old but still valid, article :_ [_Why TCP Over TCP Is A Bad Idea_](http://sites.inka.de/bigred/devel/tcp-tcp.html) _by the creator of_ [_CIPE_](http://sites.inka.de/~bigred/devel/cipe.html)_, who moved later on to OpenVPN_
>
> But **in case you absolutely need it**, **you can run OpenVPN even on TCP 80 or 443** \(web server ports\), if unused on your server machine : if you happen to be _on a network with very strict Internet access rules, you may have to resort to that_.
>
> However in this setup we'll use the [**"official" OpenVPN UDP port**](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml?search=1194) : **UDP 1194**.

**Easy-RSA 2 is a set of command-line utilities to create/manage Public Key Infrastructures \(PKIs\)**

_Easy-RSA 2 is developed by the same team as OpenVPN._

**Windows installation packages for OpenVPN include Easy-RSA 2**.



## =============== CONTINUE WORK HERE ================



I usually try to keep a complete copy of Easy-RSA 2 directly in the "safe" folders where I create PKIs because :

* all my PKIs are "off-line" PKIs for remote OpenVPN servers and i prefer a safer place to keep them than the Windows system drive...
* i can edit the default settings specifically for each PKI and keep these customized files

I encourage you to do the same and have a "blank PKI" folder ready to be duplicated to easily initialize new PKIs :

* create/locate a **`new-PKI`** subdirectory somewhere in your safe place
* copy the all files from the **`<OpenVPN Installation>\easy-rsa`** subdirectory to your target **`new-pki`** folder
