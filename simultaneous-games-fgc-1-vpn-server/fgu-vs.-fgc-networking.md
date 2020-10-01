# FGU vs. FGC networking

## MAJOR Difference \#1: Cloud Mode

* **FGC**: GM FGC= server, players FGC = clients
* **FGU in "LAN mode"**: \(like FGC\) GM FGU = server, players FGU = clients
* **FGU in "Cloud Mode"**: GM+Players FGU = clients, server = cloud infrastructure

With **FGU in Cloud mode** :

Everyone \(GM+players\) is a client, using outgoing connections to the cloud server infrastructure and there is no requirement to accept incoming connections on the GM's machine at the network transport layer \(with the delicate/dreaded NAT boundary crossing\).

{% hint style="success" %}
**There is no need for static port forwarding or VPN**  for **FGU in Cloud mode** 🙌 
{% endhint %}

However it does add a dependency on the **availability and reachability of the Unity cloud platform** from the GM and the players...**which is generally ok !** ..._except for the rare case when it isn't..._

_Also, Unity networking is **IPv6-aware**, but we won't_ ~~_open that can of worms_~~ _explore this aspect...it is sufficient to tell that it should work with "all" Internet Access Providers around Ze world..._

{% hint style="success" %}
With FGU networking, SmiteWorks has reached **a great milestone for increased ease of use of Fantasy Grounds,** for an overwhelming majority of users ! 👏 

...and of course let's not forget the **native support of Mac/Linux platforms**...and **LOS**...etc...
{% endhint %}

_Let's hope that within a few years, when FGU remains the only version of FG in use, this blurb will have reached a great milestone towards being totally useless !_ 😂 _Unfortunately for now, experience shows there are still a number of situations where it can help._

## Difference \#2: UDP vs TCP protocol

* **FGC** uses the **TCP** protocol
* **FGU** uses the **UDP** protocol

{% hint style="warning" %}
This is also true for **FGU in LAN mode** =&gt; we'll have to **adapt firewall & port forwarding rules** to take **UDP** into account for that use case
{% endhint %}

**NB**: This is for the **game session** itself;  FGU still uses TCP to reach the Unity Lobby, and to manage the update process from SmiteWorks servers.

## **Difference \#3 : Port number selection**

* **FGU Cloud mode** : not relevant
* **FGU LAN mode** : entry box **available on screen** in launcher, **persisted** to connections history
* **FGC**: **command-line flag** required for non-default port, **when starting FGC "launcher"**

###  For FGU - Cloud mode

**You don't need to select a specific port**, since the connection to Unity platform is dynamically auto-negociating this for you \(**both for the GM and for the players**\).

In **Cloud mode**, all the network plumbing between the GM and the players is handled within the Unity cloud server infrastructure \(of which the GM FGU and the players FGU are clients\), and **the \(UDP\) port entry box on GM FGU launcher screen is NOT taken into account** \(in the following example, the Cloud-mode game will **not** use UDP 1805, even though I typed this port number\) :

![](../.gitbook/assets/image%20%28187%29.png)

_**NB:** The difference between "public" and "private" games in Cloud mode is that players will see all public games listed in the lobby, while they need to "Join By GM Name" to connect to private games.  Both public and private games can be protected by a password._

After a player has successfully connected, the **players' connection history persists relevant information** to easily reconnect for next session \(the password stays hidden\) :

![](../.gitbook/assets/image%20%28196%29.png)



### For FGU - LAN mode

The **GM FGU** is a server, listening to the **\(UDP\) port number** you enter on the **lauch screen port entry box** \(default = UDP 1802\).  The following example shows an FGU game run in LAN mode on non-default port UDP 1805 :

![](../.gitbook/assets/image%20%28195%29.png)

_**NB:** LAN-mode games are not listed in the lobby._

The **players FGU** are clients, connecting directly to the GM FGU using its **public IP address \(or DNS name\)** and **UDP port** \(default = UDP 1802\), entered in the relevant **entry boxes** :

![](../.gitbook/assets/image%20%28186%29.png)

After a player has successfully connected to a specific LAN-mode game, the **connection history will also persist all relevant information** to reconnect for next session, including the specific UDP port :

![](../.gitbook/assets/image%20%28191%29.png)



### For FG "Classic"

FG "Classic" networking works roughly in the same way \(on TCP\) as FGU-LAN mode \(on UDP\) :

* the **GM FGC** is a server =&gt; has to **listen to incoming connections on a specific \(TCP\) port**
* the players FGC are clients, using direct outgoing connections to the GM FGC public IP/Port

_FG\(C\) features the "Server Alias" system, which works reasonably well to both hide the public IP address of the GM, and stay persistent when the GM public IP changes - but it does not mandatorily work for edge-case network situations \(especially VPNs\)._

{% hint style="danger" %}
**FGC uses TCP 1802 by default, and has NO entry box to select a TCP port on the launcher screen, either for the GM or players**.

It also cannot persist a specific TCP port in the player's connection history.
{% endhint %}

{% hint style="success" %}
But **FGC supports a "command-line flag" to specify a TCP port number when running the FG launcher** : **-p**_**NNNN**_

This will enable us to **run multiple concurrent FG Classic games through a single IP address** \(read: a single VPN server\)
{% endhint %}

{% hint style="warning" %}
The main issue here is that this is **NOT 100% transparent**, since the **players have to use the same**  `-pNNNN` **port parameter as the GM**.
{% endhint %}



The easiest way to mitigate that extra complication is to [create an additional Windows Shortcut](https://www.digitalcitizen.life/how-create-shortcuts) to Fantasy Grounds "Classic" \(which should be located in C:\Program Files \(x86\)\Fantasy Grounds\) with the relevant  `-pNNNN`parameter for the port you choose, added at the end of the "Target" field.

For example, to run FGC on TCP port 1809, one would :

* either start it from a command line prompt with : `"C:\Program Files (x86)\Fantasy Grounds\FantasyGrounds.exe" -p1809`
* or create a Windows shortcut to FG, and edit it's properties to add  `-p1809` at the end of the "Target" field

On Windows, that gives \(French version here, "Target" = "Cible"\) :

![A shortcut to FG &quot;Classic&quot; with port TCP1809](../.gitbook/assets/image%20%28188%29.png)

{% hint style="warning" %}
_The port parameter needs **a space before -p**, and **NO space between -p and the port number**_
{% endhint %}

