# Lifting the "1 GM at-a-time" restriction

What if you have **multiple GMs that need the VPN to host games** ? For example, you may belong to a community of Fantasy Grounds players and GMs mostly based in "pure IPv6" countries.

There are several ways to manage such situations, that we'll explore below :

* **1-at-a-time /N :** _**Time-sharing**_ a single VPN Server + GM connection/certificate
* **Nx 1-at-a-time :** Each GM creates his _**own VPN server**_ following this guide
* **1x N-at-a-time :** Single VPN Server + Nx _**GM-dedicated connections/certificates**_



{% hint style="warning" %}
Remember that [**each GM has to check his/her own machine**](../general/gms-machine-barriers.md) for "Barriers 1 to 3"
{% endhint %}



## **1-at-a-time /N :** _**Time-sharing**_ a single VPN Server + GM connection/certificate

You share with other GMs the hard work you have already done following this guide :

* you distribute [the same set of OpenVPN parameters and credentials](../local-setup/step-3-prepare-your-openvpn-connection.md#my-fg-ovpn-dm01) to all you GM friends
* each of them [installs OpenVPN client](../local-setup/step-1-install-openvpn-+-easy-rsa-2.md) _**NB: They do NOT need to install EasyRSA2 or setup a PKI, only OpenVPN itself is required**_
* each of them [installs the set of parameters](../local-setup/step-3-prepare-your-openvpn-connection.md#openvpn-gui) that you supplied

👍 **Pros :**

* lightweight solution
* no technical skills required for other GMs

👎 **Cons :**

* **STILL 1-AT-A-TIME !**
* replacing the certificate requires installing the new set of parameters for all GMs
* **your own availability**, required to start/launch the server and communicate the new Public IP if it only runs on-demand, as opposed to 24x7
* **time-sharing organization**, usage planning and timezone errors, colliding time slots - **2 GMs connected at the same time =&gt; broken FG sessions** - one day or another you'll encounter issues like those :

![Overlapping sessions](../.gitbook/assets/gms-collision01.png)

![VPN not disconnected after a game session =&amp;gt; next game delayed](../.gitbook/assets/gms-collision02.png)



## **Nx 1-at-a-time :**  Each GM creates his own VPN server

This option is self-explanatory...

The main issue here is that some \(most ?\) of the **other GMs may not be as geeky as you are** \(since you made it this far...😉\), and **may be daunted by this solution**.

👍 **Pros :**

* maximum flexibility

👎 **Cons :**

* maximum setup work
* technical skills required for GMs



## **1x N-at-a-time :** Single VPN Server + Nx _**GM-dedicated connections/certificates**_

_\*\*\*\*_

