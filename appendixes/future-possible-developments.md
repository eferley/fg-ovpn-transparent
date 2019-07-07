# Possible \(?\) developments

![](../.gitbook/assets/teameutest.png)

## FG "Unity"

This is _**unknown territory for me**_ and will stay so until at least I can _**get the beta**_ "in my hands", and more probably until the _**official launch**_ of the product.

There _**should remain a virtual table access/communication mode of the same kind as FG "Classic"**_,  but _**also some much awaited**_ "MMO-like" _**features**_ like a lobby, etc.., _**supposedly freeing the GMs from the port forwarding issue.**_

That would probably mean that SmiteWorks maintain _**some kind of central routing platform \(?\)**_ handling **inbound** communications for the live virtual tables from GMs+Players ???

That would also probably need to be _globally deployed_, to be _viable on a worldwide scale_, and that makes me _**wonder about running costs ???**_

Enough what-ifs !

_**I'm ignorant on what can actually be done with Unity network features**_, so I'll just have to look at the new FGU networking when I can test the beast...

## Multiple concurrent GMs

One drawback of this setup is **under-utilization** : it uses a **complete server** virtual machine **for only 1 GM** at a time, because the server only has 1 public IPv4 address \(to avoid costs\).  With the smallest possible virtual server at AWS \(currently 1 virtual processor + 512MB RAM\), the **CPU is used less than 0.5%** ! What a waste !

However we have one issue for FG "Classic" : **FG "Join Game" connects exclusively to TCP 1802** on the target server, and I didn't find anything where you can override this behaviour, and since **with only 1 public IP, there can be only 1 "listener" on TCP 1802 for inbound connections...** 💩 

### **Multiple Public IPs**

{% hint style="info" %}
**A \[fixed\] public IPv4 address is now like a magical item giving \[+2\]+1 to all abilities...**
{% endhint %}

**While using multiple public IPs is an option, it requires using "Elastic IP Addresses" \(EIP\) at AWS, which are fixed public IP addresses that are reserved for your account when allocated, but they are free ONLY when attached to a RUNNING server as the ONLY public IP for that server :**

* the first EIP attached to a running virtuall server is free 
* an additional EIP attached to the same running server will cost $0.005 /Hour +VAT
* an unattached EIP, or an EIP attached to a stopped server will also cost $0.005 /Hour +VAT

That means \(as of june 2019\) for **an unused/second EIP : around $4.4 /month**, VAT included.

Also, an EIP is attached to a network interface, of which there can be only a **limited number per server** depending on its type, and of course the smallest/cheapest servers can only have 2 interfaces =&gt; a maximum of 2 concurrent GMs that could be reached from players on EIP1/1802 + EIP2/1802.

And finally, the "_create on use, destroy when finished_" strategy we use for the server would leave the EIP\(s\) unattached =&gt; billable...

### Redirecting on the player side

So, the only way i see to "mutliplex" is on the client side \(i.e. the player machine\), to introduce proxying and redirect an outbound connection to TCP 1802 towards another \(IP+\)port.  The best way to do that for a player is to use the localhost \(127.0.0.1\) address as the connection target, and intercept 127.0.0.1/TCP 1802 inbound connections to redirect them.

{% hint style="info" %}
I have **successfully tested** setups with the old [**rinetd**](https://boutell.com/rinetd/) utility with a very simple 1-line config file, and that is the "**best**" **\(least bad\) solution i found for Windows** : lightweight, simple, no permanent setting left behind.
{% endhint %}

It is also achievable with `netsh interface proxyport` on modern versions of Windows, but it is very awkward, needing Windows Admin access and creating a permanent redirect 👿 that you have to delete afterwards...

This is what I tested :

We can have n concurent GMs connected to the VPN server, each with his own **fixed VPN private IP** \(using the GMs' certificate identity and the **ccd**="client-config directory" ****feature of OpenVPN\) and with corresponding IPn-specific port forwarding rules installed in the VPN server :

* **GM1** always gets private IP **IP1** in the VPN when connecting to OpenVPN \(configured in ccd\)
* **GM2** always gets private IP **IP2** in the VPN when connecting to OpenVPN \(configured in ccd\)
* the server listens to **ports P1 and P2** \(ex: 18021 and 18022\) on its unique public ****IP=**IPP**
* 1 forwarding for **IPP/P1 &lt;=&gt; IP1/1802 : GM1**
* 1 forwarding for **IPP/P2 &lt;=&gt; IP2/1802 : GM2**

So the problem is to find a way to have, **for a session \(NOT permanently\)** :

* **Players A, B and C** to have their FG connect to **IPP/P1** to get into **GM1**'s  virtual table
* **Players X, Y and Z** to have their FG connect to **IPP/P2** to get into **GM2**'s  virtual table

**Same unique public IP on the server but 2 different TCP ports =&gt; 2 FG virtual tables at a time, and that can be generalized to a higher number than 2 of course.**

{% hint style="warning" %}
Unfortunately that **defeats our secondary objective N°1 : "transparency"**

Even if it's not a VPN client install, it's still a specific config to manage for the players \(1 small exe + a 1-line config file to keep somewhere and run when needed\), so the solution becomes **"NOT transparent" for the players**. 👿 
{% endhint %}

{% hint style="info" %}
That _**could be**_ **acceptable for a regular group** \(GM + Players\) **for a whole campaign**
{% endhint %}

{% hint style="danger" %}
But it **would not be viable for temporary things like 1shot games, FGC classes, etc**...
{% endhint %}

So : no _**satisfying**_ free workaround on my side for the moment...



## Other ideas...

### **...are welcome !** 😁 



### Discord helper bot

I plan to build a small _**basic operations admin bot**_ for my own OpenVPN + Discord : launch, terminate, start, stop, reboot + getting the public IP \(+updating DNS in Route53\)

This will be based on ideas I found while [prowling GitHub](https://github.com/search?p=3&q=discord+bot+aws&type=Repositories)...[Discord.js](https://discord.js.org)...[Discord Dev portal](https://discordapp.com/developers)...

### _**useful tools ???**_ 

Diag.tools, common commands etc...

Not sure it would be useful : there are already sooo many examples everywhere.....

### Other ways of using OpenVPN

If interest for own "_closed VPN"_ for example...





