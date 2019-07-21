# Tools

We'll use several free tools during the setup process; OpenVPN + Easy-RSA are mandatory and the rest is very helpful.

## Open VPN Community \(free\)

**OpenVPN is an "SSL/TLS VPN" that is very mature and very widely used.**

It can operate on any unused TCP or UDP port and can **easily cross all kinds of firewalls and NAT-PAT routers**, even in generally adverse situations like mobile devices.

[OpenVPN](https://openvpn.net/) now has commercial offers but we'll use the **free** [**Community**](https://openvpn.net/community/) **edition** for our "do it yourself" installation.

You can select and download the latest version \(v2.4.7 as of this writing\), corresponding to your own OS, here : [https://openvpn.net/community-downloads/](https://openvpn.net/community-downloads/)

![](../.gitbook/assets/image%20%28141%29.png)



## Easy-RSA \(free\)

Easy-RSA is also managed by the OpenVPN team; it is the tool we'll use to create our **PKI** \(**Public Key Infrastructure**\) with its **CA** \(**Certificate Autority**\), to create and manage our **Digital Certificates.**

{% hint style="info" %}
**Easy-RSA 2 should already be included in OpenVPN Windows installers**
{% endhint %}

If needed, here is the GitHub download location : [https://github.com/OpenVPN/easy-rsa-old/releases](https://github.com/OpenVPN/easy-rsa-old/releases)

_For Linux-like environments, you can also choose Easy-RSA 3; download from :_ [_https://github.com/OpenVPN/easy-rsa/releases_](https://github.com/OpenVPN/easy-rsa/releases)\_\_

{% hint style="danger" %}
If you use Easy-RSA 3, check the docs as the commands have changed from Easy-RSA 2
{% endhint %}



## Optional tools

### CloudBerry Explorer for Amazon S3 \(free version\)

CBE for S3 is a very useful tool to **transfer files between your own machine and your Amazon S3 buckets**, with a familiar Windows Explorer -like GUI : 

![CloudBerry Explorer for Amazon S3 : GUI](https://www.cloudberrylab.com/img/modules/product-tour/explorer/amazon-s3/2.png)

{% hint style="info" %}
This tool has a free version and a commercial version; however, **only the commercial version supports encrypted S3 buckets**, so **we'll do without encrypted buckets** \(at least at first\).
{% endhint %}

You can grab the software here : [https://www.cloudberrylab.com/explorer/amazon-s3.aspx](https://www.cloudberrylab.com/explorer/amazon-s3.aspx)



### PuTTY \(free\)

This tool should not be generally required, but **if you want to open a command line terminal to your Linux VPN server**, then PuTTY is the way to go for the Windows platform.

PuTTY is available from : [https://www.putty.org/](https://www.putty.org/)

![](../.gitbook/assets/image%20%28100%29.png)

### Your text editor of choice

_**Avoid the very basic Windows Notepad**_ for its lack of customization options, **no support for Unix-style "End-Of-Lines"**, etc.  If you paste in Windows Notepad some file contents from these web pages, they will look like garbage...

**ABSOLUTELY DON'T USE ANY MICROSOFT-WORD-LIKE "WORD PROCESSING" SOFTWARE !!!**

Here are 3 very common good quality free text editors \(initially created for developers, they support much more than simple text editing, but this is out of our scope\) :

* A good **lightweight but feature-rich free editor** is **Notepad++** that you can find at : [https://notepad-plus-plus.org](https://notepad-plus-plus.org/)

![](../.gitbook/assets/image%20%28182%29.png)

**More powerful** \(but also more complex\) environments are **available for free** like :

* [Microsoft Visual Studio Code](https://code.visualstudio.com/)

![](../.gitbook/assets/image%20%28168%29.png)

* [GitHub Atom](https://atom.io/)

![](../.gitbook/assets/image%20%28181%29.png)





