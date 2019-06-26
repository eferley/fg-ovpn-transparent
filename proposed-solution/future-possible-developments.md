# Future possible developments

## FG "Unity"

This is still **unknown territory** for me until I can get the beta "in my hands".

There should remain a virtual table access/communication mode of the same kind as FG "Classic",  but also some "MMO-like" features like a lobby, etc. supposedly freeing the GMs from the port forwarding issue.

That would probably mean that SmiteWorks maintain some kind of "central routing platform" handling communications for the live virtual tables, and that makes me wonder about running costs ?

I'll see when I can test the beast...

## Multiple concurrent GMs \(open discussion\)

One drawback of this setup is **under-utilization** : it uses a **complete server** machine **for only 1 GM** at a time, because the server only has 1 public IPv4 address \(to avoid costs\).  With the smallest possible virtual server at AWS \(currently 1 virtual processor + 512MB RAM\), the **CPU is used less than 0.5%** ! What a waste !

However we have one issue for FG "Classic" : **FG "Join Game" connects exclusively to TCP 1802** on the target server, and I didn't find anything where you can override this behaviour, and since **with only 1 pubic IP, there can be only 1 "listener" on TCP 1802 for inbound connections...** 💩 

### **Multiple Public IPs**

{% hint style="warning" %}
**A \(fixed\) public IPv4 address is now like a magical item giving \(+2\)+1 to all abilities...**
{% endhint %}

**While using multiple public IPs is an option, it requires using "Elastic IP Addresses" \(EIP\) at AWS, which are fixed public IP addresses that are reserved for your account when allocated, but they are free ONLY when attached to a RUNNING server as the only ONLY public IP for that server :**

* the first EIP attached to a running virtuall server is free 
* an additional EIP attached to the same running server will cost $0.005 /Hour +VAT
* an unattached EIP, or an EIP attached to a stopped server will also cost $0.005 /Hour +VAT

That means \(as of june 2019\) for **an unused/second EIP : around $4.5 /month**, VAT included.

Also, an EIP is attached to a network interface, of which there can be only a **limited number per server** depending on its type, and of course the smallest/cheapest servers can only have 2 interfaces =&gt; a maximum of 2 concurrent GMs that could be reached from players on EIP1/\(TCP\)1802 and EIP2/\(TCP\)1802.

And finally, the "create on use, destroy when finished" strategy we use for the server would leave the EIP\(s\) unattached =&gt; billable...

### Redirecting on the player side

So, the only way i see to "mutliplex" is on the client side \(i.e. the player machine\), to introduce proxying and redirect an outbound connection to TCP 1802 towards another \(IP+\)port.  The best way to do that for a player is to use the localhost \(127.0.0.1\) address as the connection target, and intercept 127.0.0.1/TCP 1802 inbound connections to redirect them.

I have successfully tested setups with the old [rinetd](https://boutell.com/rinetd/) utility with a very simple 1-line config file, and that is the best solution i found for Windows : lightweight, simple, no permanent setting left behind.

It is also doable with `netsh interface proxyport` on modern versions of Windows, but it is very awkward, needing Windows Admin access and craeting a permanent redirect that you have to delete afterwards...

Then we can have multiple concurent GMs connected to the VPN server, each with his own fixed VPN private IP \(using the GMs' certificate identity and the **ccd**="client-config directory" ****feature of OpenVPN\) and with corresponding IP1/IP2-specific port forwarding rules installed in the VPN server :

* **GM1** always gets private IP **IP1** in the VPN when connecting to OpenVPN \(configured in ccd\)
* **GM2** always gets private IP **IP2** in the VPN when connecting to OpenVPN \(configured in ccd\)
* the server listens to ports P1 and P2 \(ex: 18021 and 18022\) on its unique public IP=**IPP**
* 1 forwarding rule for **GM1** : **IPP/P1 =&gt; IP1/1802**
* 1 forwarding rule for **GM2** : **IPP/P2 =&gt; IP2/1802**

So the problem is to find a way to have, **for a session \(NOT permanently\)** :

* **Players A, B and C** to have their FG connect to **IPP/P1** to get into **GM1**'s  virtual table
* **Players X, Y and Z** to have their FG connect to **IPP/P2** to get into **GM2**'s  virtual table

**Same unique public IP on the server but 2 different TCP ports =&gt; 2 FG virtual tables at a time.**

{% hint style="warning" %}
Unfortunately **that more or less defeats our secondary objective N°1** :

Even if it's not a VPN client install, it's still a specific config to manage for the players, so the solution becomes **"NOT transparent" for the players**.
{% endhint %}

{% hint style="info" %}
That **could be acceptable for a regular group** of GM + Players **for a whole campaign**
{% endhint %}

{% hint style="danger" %}
But it **would not be viable for temporary things like 1shot games, FGC classes, etc**...
{% endhint %}

So : no satisfying \(free\) workaround for the moment...



