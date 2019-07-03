# Step 3 : Setup your GM OpenVPN connection -WIP

We'll now prepare sets of security and parameter files per OpenVPN client \(GM in our context\) so that they can be distributed to the relevant people/machines.

{% hint style="info" %}
**You will likely only need one such set, for yourself : my-fg-ovpn-dm01**
{% endhint %}

But we'll also do it for "my-fg-ovpn-dm02" so you can test both, and see the connection **working for dm01** and **failing for dm02** \(we _**revoked**_ this guy earlier\), which will prove that the [CRL](step-2-create-your-pki.md#your-initial-crl-the-certification-revocation-list) is taken into account.



So, let's head over to **`T:\fg-ovpn\MY-FG-OVPN\my-fg-ovpn-clients\my-fg-ovpn-dm01`**, one of the folders we prepared long ago.  It should currently be empty :

![](../.gitbook/assets/image%20%287%29.png)

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

**This is a text file that you'll have to save in the above folder.**

Let's have a look at this config file :

* Lines beginning with **`#`** or **`;`** are comments and have no impact on the connection configuration.
* **lines 14 to 18** show that our OpenVPN client will **need several files** to work
* **line 6** is where we'll need to put the **Public IP of the OpenVPN server** \(or its DNS name, if it has one\)

So we'll save the .ovpn file above to **`T:\fg-ovpn\MY-FG-OVPN\my-fg-ovpn-clients\my-fg-ovpn-dm01`**, and also copy/paste there from **`T:\fg-ovpn\MY-FG_OVPN\MY_FG_OVPN_PKI\MY-FG-OVPN-PKI-KEYS`** :

* **my-fg-ovpn-ca.crt** : the public certificate for our Certification Authority _&lt;&lt;== NOT client-specific_
* **my-fg-ovpn-ta.key** : the shared TLS Authentication security key    _&lt;&lt;== NOT client-specific_
* **my-fg-ovpn-dm01.crt** : the client public certificate    _**&lt;&lt;==client-specific**_
* **my-fg-ovpn-dm01.key** : the client private key    _**&lt;&lt;==client-specific**_

Which gives :



















