# Step 3 : Setup OpenVPN connections

We'll now prepare a complete set of security and parameter files per OpenVPN client \(GM in our context\) so that they can be easily distributed to the relevant people/machines.

{% hint style="info" %}
**At the end of the process, you will only need one such set, for yourself : my-fg-ovpn-dm01**
{% endhint %}

But we'll **also do it for \(revoked\) "my-fg-ovpn-dm02"** so you can test both, and see the connection **working for dm01** and **failing for dm02** \(we _**revoked**_ this guy earlier\), which will verify that the [CRL](step-2-create-your-pki.md#your-initial-certification-revocation-list) is taken into account.



In the first part we are looking at things _**as the PKI owner.**_  Then we'll switch to the _**VPN user point of view**_ when we being playing with _**OpenVPN GUI.**_



## OpenVPN client parameters

### my-fg-ovpn-dm01

So, let's head over to **`T:\fg-ovpn\MY-FG-OVPN\my-fg-ovpn-clients\my-fg-ovpn-dm01`**, one of the folders we prepared long ago.  It should currently be empty :

![](../.gitbook/assets/image%20%2816%29.png)

We'll begin by creating the **OpenVPN connection parameter file** : **`my-fg-ovpn-dm01.ovpn`**

{% code-tabs %}
{% code-tabs-item title="my-fg-ovpn-dm01.ovpn" %}
```text
client
float
dev tun
;dev-node <name of TAP virtual network card in Windows, if you have several of them>
proto udp
remote <OpenVPN Server Public IP or FQDN> 1194
resolv-retry infinite
nobind
persist-key
persist-tun
;http-proxy-retry # retry on connection failures
;http-proxy [proxy server] [proxy port #]
;mute-replay-warnings
ca my-fg-ovpn-ca.crt
cert my-fg-ovpn-dm01.crt
key my-fg-ovpn-dm01.key
ns-cert-type server
tls-auth my-fg-ovpn-ta.key 1
comp-lzo
verb 4
```
{% endcode-tabs-item %}
{% endcode-tabs %}

_**This is a text file that you'll have to save in the above folder.**_

Let's have a look at this config file :

* Lines beginning with **`#`** or **`;`** are commented and have no impact on the connection configuration.
* **lines 14 to 18** show that our OpenVPN client will **need several additional files** to work; they have all been created during the previous step
* **line 6** is where we'll need to put the **Public IP of the OpenVPN server** \(or its DNS name, if it has one\) _**Since we don't yet know this Public IP, we'll leave the &lt;placeholder&gt; there for now**_

So we'll save the .ovpn file above to **`T:\fg-ovpn\MY-FG-OVPN\my-fg-ovpn-clients\my-fg-ovpn-dm01`**, and also copy/paste there from **`T:\fg-ovpn\MY-FG_OVPN\MY_FG_OVPN_PKI\MY-FG-OVPN-PKI-KEYS`** :

* **my-fg-ovpn-ca.crt** : the public certificate for our Certification Authority _&lt;&lt;== NOT client-specific_
* **my-fg-ovpn-ta.key** : the shared TLS Authentication security key    _&lt;&lt;== NOT client-specific_
* **my-fg-ovpn-dm01.crt** : the client public certificate    _**&lt;&lt;==client-specific**_
* **my-fg-ovpn-dm01.key** : the client private key    _**&lt;&lt;==client-specific**_

Which gives :

![OpenVPN Client parameters for my-fg-ovpn-dm01](../.gitbook/assets/image%20%2856%29.png)

{% hint style="success" %}
**Your "distribution folder" for my-fg-ovpn-dm01 is complete...**

**...except for the placeholder at line 6** of**`my-fg-ovpn-dm01.ovpn`**, which will have to be replaced with a real value : **`<OpenVPN Server Public IP or FQDN>`**.
{% endhint %}

{% hint style="info" %}
There is _another way to create OpenVPN configs_ : mono-file connection profiles where _all the certificates and keys are all embedded as text blocks into the .ovpn file_.

I prefer to stay with the "old way" and keep files separate as it maintains compatibility with older versions of OpenVPN \(my GM machine still uses an old 32bit OpenVPN v2.2.2 😬 \) - _but feel free to explore_ [_OpenVPN docs_](https://openvpn.net/community-resources/#articles) _for the other format..._
{% endhint %}



### Rinse and repeat for my-fg-ovpn-dm02

You can :

* copy/paste + **rename and edit** \("dm01"-&gt;"dm02"\) the **`.ovpn`** text file from dm01's to dm02's sub-folder
* then copy/paste the 2 "client-invariant" files : CA certificate and TLS-Auth key
* and copy/paste the 2 "client-specific" files : dm02's certificate+key **from the PKI keys sub-folder**.

_**DM02's setup :**_

{% code-tabs %}
{% code-tabs-item title="my-fg-ovpn-dm02.ovpn" %}
```text
client
float
dev tun
;dev-node <name of TAP virtual network card in Windows, if you have several of them>
proto udp
remote <OpenVPN Server Public IP or FQDN> 1194
resolv-retry infinite
nobind
persist-key
persist-tun
;http-proxy-retry # retry on connection failures
;http-proxy [proxy server] [proxy port #]
;mute-replay-warnings
ca my-fg-ovpn-ca.crt
cert my-fg-ovpn-dm02.crt
key my-fg-ovpn-dm02.key
ns-cert-type server
tls-auth my-fg-ovpn-ta.key 1
comp-lzo
verb 4
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![](../.gitbook/assets/image%20%2813%29.png)



## OpenVPN GUI

Let's now switch to the _**user view of OpenVPN**_, which is what you'll use in the future : the **OpenVPN GUI**, that you can find in the OpenVPN group, "pin to Start", etc...

![](../.gitbook/assets/image%20%2839%29.png)

![](../.gitbook/assets/image%20%2832%29.png)

When we use it for **the first time**, the result is "a bit" **anticlimatic**... 😬 

![](../.gitbook/assets/image%20%2849%29.png)

After you close this dialog, you don't see anything new about OpenVPN on your screen. 😡 

**Actually this is an excellent thing** : OpenVPN will stay as inconspicuous as possible, whether connected to 0, 1, or more servers.

Ok, let's move on to the **notification icons** on the bottom right of the desktop, where you'll finally discover something new :

![](../.gitbook/assets/image%20%2879%29.png)

I'll choose to have this notification icon _always visible_, so it will now always be on my taskbar.

Let's **Right-click** it :

![](../.gitbook/assets/image%20%2847%29.png)

A quick look at the default settings :

![](../.gitbook/assets/image%20%2828%29.png)

![](../.gitbook/assets/image%20%2826%29.png)

![Note : the personal folders above have just been created](../.gitbook/assets/image%20%2827%29.png)

I'll just change 2 options according to **my own preference** : to have it _always running_ \(discreetly, in the taskbar\), and have a _notification balloon_ also when a "reconnect" happens :

![Updating settings / personal preference](../.gitbook/assets/image%20%2842%29.png)

Now let's have a look at the folders in my Windows personal storage \(that we see on the "Advanced" tab of the Settings above\) :

![](../.gitbook/assets/image%20%2812%29.png)

**Let's bring some configs there ! We'll copy/paste our 2 shiny "distribution folders"** for dm01 and dm02, from our "PKI owner" storage : we are "distributing" to ourselves...

![Let&apos;s copy from the client distributions...](../.gitbook/assets/image%20%2831%29.png)

![...and paste to our own &quot;live&quot; client setup.](../.gitbook/assets/image%20%2880%29.png)

{% hint style="success" %}
**There we are ! All set and ready to connect. Just right-click the OpenVPN GUI again :**
{% endhint %}

![](../.gitbook/assets/image%20%2821%29.png)

When your OpenVPN server is on-line and you know its **Public IP** :

* you'll use the "**Edit config**" option to write the **public IP at line 6** of the config, save and close the file,
* then right-click OpenVPN GUI again, this time choosing "**Connect**"

🎆 **Done !** 🎆 ****

{% hint style="info" %}
The **`.ovpn`** **config files** that you edit when using the "**Edit Config**" menu item of **OpenVPN GUI** are the ones stored **in your OpenVPN personal folder.**
{% endhint %}



