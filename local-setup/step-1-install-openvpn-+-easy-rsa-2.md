# Step 1 : OpenVPN + Easy-RSA & "new-PKI"

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



So, we download and save the installer from [https://openvpn.net/community-downloads/](https://openvpn.net/community-downloads/) for our version of Windows \(I have built a small temporary Windows 2012R2 to "impersonate" the GM FG machine for this demo\) into the folder we previously created at **`T:\fg-ovpn\source-install`** :

![](../.gitbook/assets/image%20%2861%29.png)

...and we execute, authorizing it to do its job through the **Yes** on the "User Access Control" dialog...  


![](../.gitbook/assets/image%20%2829%29.png)

...making sure to **select "Easy-RSA 2 Certificate Management Scripts"** :

![](../.gitbook/assets/image%20%2882%29.png)

...AND **allowing the creation of the virtual network card** with the "**Install**" button :

![](../.gitbook/assets/image%20%2875%29.png)

{% hint style="success" %}
Finally, we'll have our **complete installation result** looking like this \(depends on Windows version AND the version of OpenVPN\) :
{% endhint %}

![](../.gitbook/assets/image%20%2895%29.png)

> **Tech Blurb :**
>
> **Note the "TAP-Windows" group above :** OpenVPN uses a virtual network card/adapter for each concurrent VPN.  So if you need _multiple VPNs connected at a time_ \(I often have 2 or 3\), you'll need to use the "Add new TAP..." but **this is not required for our demo setup**.

\*\*\*\*

Let's have a look at the installation folders in **`Program Files`** :

![](../.gitbook/assets/image%20%2883%29.png)



We now have the whole required "prime materials" on disk.  Let's immediately create a source to initialize "autonomous" blank Public Key Infrastructures in our **`new-PKI`** folder :

## Prepare the new-PKI folder contents

This folder will be a ready-to-duplicate source folder to create a brand new PKI.  We'll keep there both the current version of the Easy-RSA script files and the associated binary programs.

We'll then use it to initialize our own PKI in its own folder .  This way, even if we break/uninstall/lose our installation directory for whatever reason, we'll still be able to manage our VPN security \(create/revoke certificates\), and we'll have the right version of required programs available for that.

> **Tech Blurb :**
>
> Also by creating all PKIs \(only 1 is needed for our setup\) by duplicating this source folder content later on, we have the ability to tweak script files or even the openssl configuration file **used for that specific PKI** if needed, without impacting others.



1. copy all files from **`<OpenVPN Installation>\easy-rsa`** to your **`new-PKI`** folder
2. then copy these \(or all, if you want\) files from **`<OpenVPN Installation>\bin`** to **`new-PKI`** as well

![Copy from easy-rsa to new-PKI](../.gitbook/assets/image%20%2840%29.png)

![Copy from bin to new-PKI](../.gitbook/assets/image%20%2868%29.png)

{% hint style="success" %}
**Your autonomous "ready-to-duplicate" `new-PKI` folder is now complete :**
{% endhint %}

![](../.gitbook/assets/image%20%2818%29.png)



