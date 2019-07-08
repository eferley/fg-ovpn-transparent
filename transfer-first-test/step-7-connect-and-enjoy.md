# Step 7 : Connect, test, fix glitches -WIP

## Connecting your OpenVPN client

You now know the Public IP for your server : you will use it to connect your OpenVPN client.

Right-click your OpenVPN GUI, and "Edit Config" on your connection menu :

![Edit Config to write the public IP](../.gitbook/assets/image%20%2898%29.png)

Remember the **`<OpenVPN Server Public IP or FQDN>`** placeholder at line 6 ? Replace it with your server's pubic IP that you copied previously in the EC2 Console :

![](../.gitbook/assets/image%20%2824%29.png)

Now **close+save**, right-click again and "**Connect**" !

![](../.gitbook/assets/image%20%2878%29.png)

You'll see a log flowing in a window....🥁🥁 _...aaaannnnndddd..._ 🥁🥁 

![Yeah !](../.gitbook/assets/image%20%2885%29.png)

### ![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) 👏 🎆🧨💃 🕺 **Bravo ! Cheers ! CongratZ !**  🕺 💃 🧨 🎆 👏 ![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) 

_**You are now officially a geek : you're both a VPN client AND a VPN operator, all in 1 person !**_ 

_So bookmark this and come back after you have properly celebrated this major event in your life..._

## Several tests

Ok, now let's note and test a few things...

### Your VPN client Private IP

We'll take a note of our VPN Private IP address, that you see in the OpenVPN balloon tooltip.

**For this demo**, we **specified VPN network in our UserData** as **10.10.10.0/24**, so we are **10.10.10.6**

{% hint style="warning" %}
Being the only client for your VPN server, **you should always get the .6 VPN Private IP address in the network you specify in your UserData**
{% endhint %}

{% hint style="danger" %}
**Important : the server does Fantasy Grounds port forwarding for the .6 IP ONLY**
{% endhint %}

### Some pings

Let's open a new command prompt window and try a few commands :

**`ping 10.10.10.1`** : pings the server's **VPN private IP**

Let's have a quick look at our server various IPs :

![](../.gitbook/assets/image%20%28107%29.png)

**`ping 172.31.10.45`** : pings the server's ethernet LAN adapter **private IP in AWS**

**`ping 13.53.45.99`** : pings the server's **Public IP**

![First ping tests](../.gitbook/assets/image%20%2821%29.png)

Hummm...The server does not reply to our ping on its Public IP. Why ? 🤔 

Because we [don't have a rule in our Security Group](step-6-launch-time.md#your-own-firewall-rules-security-groups) for that.

Let's add a rule to our Security Group : you can **click its name in the left column** of the EC2 instance _**Description**_ tab, then go to the **Inbound rules** tab of the **Security Group** and click **Edit**, then **Add Rule** :

![](../.gitbook/assets/image%20%2817%29.png)

We want to authorized inbound ping requests, which are in reality ICMP echo requests, so our new rule will be typed "_**Custom ICMP rule - IPv4**_", with protocol "_**Echo Request**_", from source "_**0.0.0.0/0**_" :

![](../.gitbook/assets/image%20%2855%29.png)

Let's **Save**, then go back to our EC2 instance details to check it applies :

![Yep ! Now pingable from anywhere](../.gitbook/assets/image%20%2877%29.png)

Let's retry our ping on the public IP to check we have the replies...ok ✅ 

![ping to public IP is now ok](../.gitbook/assets/image%20%2847%29.png)

### Fantasy Grounds

Now let's start Fantasy Grounds and **create a test campaign** with any ruleset and **Start** it.

{% hint style="warning" %}
Let's **not** play with the _**server alias**_ : the _**Connection test**_ will keep failing, **that's expected !**
{% endhint %}

![](../.gitbook/assets/image%20%2852%29.png)

{% hint style="info" %}
I had to switch to **my own FG machine** for that, so i :

* disconnected the test machine \(W2012\) i have used to show the latest OpenVPN \(2.4.7, 64-bit\) until now
* "distributed" the _**my-fg-ovpn-dm01**_ set of VPN client files to my own machine
* connected my own machine to the server, **once again getting VPN IP = 10.10.10.6**

This is a **Windows 7 \(FR\)** with an **older OpenVPN \(2.2.2, 32-bit\)**, but I'm connecting fine :
{% endhint %}

![An older OpenVPN \(2.2.2\)](../.gitbook/assets/image%20%2829%29.png)

![ping results still ok on my Windows 7](../.gitbook/assets/image%20%28114%29.png)

So here I am, connected to the VPN server and with Fantasy Grounds eagerly waiting for players to connect :

![A &quot;blank&quot; D&amp;D5e virtual table](../.gitbook/assets/image%20%2841%29.png)

Let's try this command in our command prompt :

```text
netstat -ano | find "1802"
```

![FG process 16100 listening on ALL my connections](../.gitbook/assets/image%20%28158%29.png)

We can see above that our "**GM FG**" has the Windows Process ID 16100, and is **listening to TCP 1802 on ALL IPs from ALL connected network adapters \(the 0.0.0.0 on the left\),** _**including our VPN Private IP**_

### Anatomy of a local connection

Now let's open a _**second Fantasy Grounds as a player on the same machine**_, and _**Join Game**_ on _**localhost**_ as Host address \(**localhost means "myself" and is the special private IPv4 address 127.0.0.1**\)

![Player connection to localhost](../.gitbook/assets/image%20%2890%29.png)

![](../.gitbook/assets/image%20%28134%29.png)

That works as expected : nothing fancy here, but if we retry our last command, we see how FG networking works :

![FG : GM + 1 player](../.gitbook/assets/image%20%2863%29.png)

* the GM FG is still LISTENING on 0.0.0.0:1802 \(P.ID.16100, **line1**\)
* the player FG \(**line 3**\) :
  * has Windows Process ID 15428
  * It connected from 127.0.0.1 \(using its port 55536\) to localhost=127.0.0.1 on port 1802, and the connection is still active \(ESTABLISHED\)
* the GM FG has accepted the connection \(**line 2**\) and "maintains the dialog" \(ESTABLISHED\) from his own 127.0.0.1:1802 to the "remote player" at 127.0.0.1:55536

The **Player and GM FGs** see each other **both on the localhost 127.0.0.1** IP address, so they accept to work that way \(otherwise we would have a _**license key conflict**_\).

Now let's **disconnect our player FG** and re-check :

![After player leaves the virtual table](../.gitbook/assets/image%20%281%29.png)

We still see our GM FG listening, and the port that was used by our player FG is now in TIME\_WAIT state, with no process ID, and will be completely free to re-use by Windows within 4 minutes.

### Connecting a player FG to the server public IP

This is all well and good, but what you want is players connecting to your server \(its public IP, currently 13.53.45.99 in this demo\) so that they auto-magically appear in your GM FG virtual table...

Let's try !

![Connecting to the server as a player from the Internet](../.gitbook/assets/image%20%2825%29.png)

_**Aarrgghh !!!...well, in fact Yeah ! It works ! \(though it looks disappointing...\)**_

![It looks like a Fail but it is a Pass !](../.gitbook/assets/image%20%2850%29.png)

{% hint style="success" %}
Our **GM FG and Player FG were actually able to talk to each other...**

But they have **different IP addresses** and the **same license key =&gt;** **license key conflict.**
{% endhint %}

### What if it fails !

![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) _**What ? You have a real failure ? Like this one ?**_

![](../.gitbook/assets/image%20%2876%29.png)

![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) _**Ha Ha ! You forgot**_ [_**what you should always remember**_](../general/vpn-solutions.md#always-remember)_**...Ok,10 seconds to**_ 😭 _**and facepalm...**_

{% hint style="info" %}
...Now recheck everything for your **GM machine's Barriers 1-3**, remembering that **FG inbound connections will come** to your GM FG **through the VPN**, which Windows \(rightfully\) considers to be a **DIFFERENT network from your usual home network.**

=&gt; for Windows 10, you have to mark the **VPN connection as PRIVATE !**
{% endhint %}

_...and if you can't make this work, ask for help with the links at the beginning of this document._

 _****_

_\*\*\*\*_

T2/T3 Unlimited mode

_\*\*\*\*_

![](../.gitbook/assets/image%20%282%29.png)









