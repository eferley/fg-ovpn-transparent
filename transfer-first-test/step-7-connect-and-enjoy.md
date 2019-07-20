# Step 7 : Connect, test, fix glitches

## Connecting your OpenVPN client

You now know the Public IP for your server : you will use it to connect your OpenVPN client.

Right-click your OpenVPN GUI, and "Edit Config" on your connection menu :

![Edit Config to write the public IP](../.gitbook/assets/image%20%28111%29.png)

Remember the **`<OpenVPN Server Public IP or FQDN>`** placeholder at line 6 ? Replace it with your server's pubic IP that you copied previously in the EC2 Console :

![](../.gitbook/assets/image%20%2827%29.png)

Now **close+save**, right-click again and "**Connect**" !

![](../.gitbook/assets/image%20%2889%29.png)

You'll see a log flowing in a window....🥁🥁 _...aaaannnnndddd..._ 🥁🥁 

![Yeah !](../.gitbook/assets/image%20%2897%29.png)

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

![](../.gitbook/assets/image%20%28122%29.png)

**`ping 172.31.10.45`** : pings the server's ethernet LAN adapter **private IP in AWS**

**`ping 13.53.45.99`** : pings the server's **Public IP**

![First ping tests](../.gitbook/assets/image%20%2824%29.png)

Hummm...The server does not reply to our ping on its Public IP. Why ? 🤔 

Because we [don't have a rule in our Security Group](step-6-launch-time.md#your-own-firewall-rules-security-groups) for that.

Let's add a rule to our Security Group : you can **click its name in the left column** of the EC2 instance _**Description**_ tab, then go to the **Inbound rules** tab of the **Security Group** and click **Edit**, then **Add Rule** :

![](../.gitbook/assets/image%20%2818%29.png)

We want to authorized inbound ping requests, which are in reality ICMP echo requests, so our new rule will be typed "_**Custom ICMP rule - IPv4**_", with protocol "_**Echo Request**_", from source "_**0.0.0.0/0**_" :

![](../.gitbook/assets/image%20%2862%29.png)

Let's **Save**, then go back to our EC2 instance details to check it applies :

![Yep ! Now pingable from anywhere](../.gitbook/assets/image%20%2888%29.png)

Let's retry our ping on the public IP to check we have the replies...ok ✅ 

![ping to public IP is now ok](../.gitbook/assets/image%20%2854%29.png)

### Fantasy Grounds

Now let's start Fantasy Grounds and **create a test campaign** with any ruleset and **Start** it.

{% hint style="warning" %}
Let's **not** play with the _**server alias**_ : the _**Connection test**_ will keep failing, **that's expected !**
{% endhint %}

![](../.gitbook/assets/image%20%2859%29.png)

{% hint style="info" %}
I had to switch to **my own FG machine** for that, so i :

* disconnected the test machine \(W2012\) i have used to show the latest OpenVPN \(2.4.7, 64-bit\) until now
* "distributed" the _**my-fg-ovpn-dm01**_ set of VPN client files to my own machine
* connected my own machine to the server, **once again getting VPN IP = 10.10.10.6**

This is a **Windows 7 \(FR\)** with an **older OpenVPN \(2.2.2, 32-bit\)**, but I'm connecting fine :
{% endhint %}

![An older OpenVPN \(2.2.2\)](../.gitbook/assets/image%20%2832%29.png)

![ping results still ok on my Windows 7](../.gitbook/assets/image%20%28130%29.png)

So here I am, connected to the VPN server and with Fantasy Grounds eagerly waiting for players to connect :

![A &quot;blank&quot; D&amp;D5e virtual table](../.gitbook/assets/image%20%2847%29.png)

Let's try this command in our command prompt :

```text
netstat -ano | find "1802"
```

![FG process 16100 listening on ALL my connections](../.gitbook/assets/image%20%28178%29.png)

We can see above that our "**GM FG**" has the Windows Process ID 16100, and is **listening to TCP 1802 on ALL IPs from ALL connected network adapters \(the 0.0.0.0 on the left\),** _**including our VPN Private IP**_

### Anatomy of a local connection

Now let's open a _**second Fantasy Grounds as a player on the same machine**_, and _**Join Game**_ on _**localhost**_ as Host address \(**localhost means "myself" and is the special private IPv4 address 127.0.0.1**\)

![Player connection to localhost](../.gitbook/assets/image%20%28103%29.png)

![](../.gitbook/assets/image%20%28153%29.png)

That works as expected : nothing fancy here, but if we retry our last command, we see how FG networking works :

![FG : GM + 1 player](../.gitbook/assets/image%20%2872%29.png)

* the GM FG is still LISTENING on 0.0.0.0:1802 \(Process ID16100, **line1**\)
* the player FG \(**line 3**\) :
  * has Windows Process ID 15428
  * It connected from 127.0.0.1 \(using dynamic port 55536\) to localhost=127.0.0.1 on port 1802, and the connection is still active \(ESTABLISHED\)
* the GM FG has accepted the inbound connection \(**line 2**\) and "maintains the dialog" \(ESTABLISHED\) between his own 127.0.0.1:1802 and the "not-so-remote" player at 127.0.0.1:55536

The **Player and GM FGs** see each other **both on the localhost 127.0.0.1** IP address, so they accept to work that way \(otherwise we would have a _**license key conflict**_, as we'll soon witness\).

Now let's **disconnect our player FG** and re-check :

![After player leaves the virtual table](../.gitbook/assets/image%20%282%29.png)

We still see our GM FG listening, and the dynamic port that was used by our player FG \(TCP 55536\) is now in TIME\_WAIT state, belongs to no process ID, and will be completely free to re-use by Windows within 4 minutes.

### Connecting a player FG to the server public IP

This is all well and good, but what you want is **players connecting to your server** \(its public IP, currently 13.53.45.99 in this demo\) so that they **auto-magically appear in your GM FG virtual table**...

**Let's try !**

![Connecting to the server as a player from the Internet](../.gitbook/assets/image%20%2828%29.png)

_\*\*\*\*_![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) _**Aarrgghh !!!...Well, actually : Yeah !!!**_ 🎆👯💃🧨 _**It works !!!** \(though it looks disappointing...\)_

![It looks like a Fail but it is a Pass !](../.gitbook/assets/image%20%2857%29.png)

## Houston, we have liftoff !

{% hint style="success" %}
**YES !** Our **GM FG and Player FG were actually able to talk to each other...**

_...but they have **DIFFERENT IPs** and the **SAME license key =&gt;** **LICENSE KEY CONLICT.**_

**Your "REAL" players CAN now connect to your GM FG, using the server Public IP**
{% endhint %}

And if you then retry our netstat command, you'll see all these connections arriving on your VN Private IP on port 1802, all coming in from the server Private IP / various ports.



**EDIT :** After some trial and errors, I was finally able to install the _Demo version of Fantasy Grounds on my temporary Windows 2012R2 machine_, even though I had some errors because it has no support for DirectX.

So here is an example of 2 connections, **from 2 players with a Demo version of FG** to my GM FG virtual table **through the OpenVPN port-forwarding server** :

![](../.gitbook/assets/image%20%28165%29.png)

The player connections seen on my GM FG machine :

![](../.gitbook/assets/image%20%2892%29.png)

{% hint style="info" %}
Our GM FG sees **ALL the "port-forwarded" player connections** coming in :

* **from the server Private IPv4 address** \(172.31.10.45 in this demo\) from various TCP ports
* **to our own VPN Private IP** \(10.10.10.6 in this demo\) always on our TCP 1802 port
{% endhint %}

_Note : I have FG Ultimate license so I can host any combination of licensed/demo players; if you have FG Standard then you can host 1 demo **or** N licensed players.  This is the FG licensing scheme._



## What if it fails ?

![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) _**What ? How ? When ? Where ? You have a real failure ? Like this one with a "player" FG ? Or worse, you can't even connect with your OpenVPN client to your shiny new server ?**_

![](../.gitbook/assets/image%20%2887%29.png)



### ![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) FG : _**Maybe you forgot**_ [_**what you should always remember**_](../general/vpn-solutions.md#always-remember) _**?**_

_**Ok,10 seconds to**_ 😭_**...**_

{% hint style="info" %}
...Now **recheck everything for your GM machine's Barriers 1-3**, remembering that FG inbound connections will come to your GM FG through **the VPN connection**, which Windows \(rightfully\) considers to be a **DIFFERENT network than your usual home network.**

=&gt; for Windows 10, you have to mark the **VPN connection as PRIVATE !**

=&gt; don't forget to open some doors in all your **firewall-like thingies**...you'll probably need to do so for **both :**

* your **VPN private IP network** \(10.10.10.0/24 in this demo\)
* **AND** the server **Private IPv4** in your AWS VPC \(172.31.10.45 in this demo\)
{% endhint %}



### **OpenVPN :** CRLF ? Check the startup log of your OpenVPN server instance

In the **Instances** section of the **EC2 Console**, use this right-click contextual menu :

![Getting the system log, including the startup log](../.gitbook/assets/image%20%28118%29.png)

That should give you \(**several minutes after** the instance is launched\) something like this...

![Beginning of the system log](../.gitbook/assets/image%20%2899%29.png)

After 450 to 500 lines, if you encounter the following error message, it means your _****_**`fgovpn001-init.sh`** _**was saved with the CRLF mode**_ for End-Of-Line control characters _**instead of LF**_ :

![Error example : init script saved with CRLF mode](../.gitbook/assets/image%20%28140%29.png)

In this example, because of a rogue **CR** character \(^M\) at the end of the very first line, **the init script did NOT execute AT ALL**, so OpenVPN is not even installed on your \(useless\) virtual server !

> In the above example, don't be confused by the following line 502 \("_UserData script finished_"\).
>
> The very last thing that the UserData script tried to do was running fgovpn001-init.sh \(line 499\), so the error actually comes from that file, not from the UserData script \(it has been cleansed of any CRs by importing it in the _web editor_ in the AWS Console\).

=&gt; [**Correct the error**](../aws-setup-step-by-step/4.7-filling-the-bucket.md#important-save-files-in-lf-unix-format) by saving **fgovpn001-init.sh in LF mode** in your **`my-fg-ovpn-s3`** local sub-folder, re-transfer the file to S3 to overwrite the one you previously stored, **Terminate** \(=destroy\) your current instance \(right-click / _**Instance State**_ / _**Terminate**_\), and then **launch** a brand new one !



_...finally, if you still can't make this work, then **ask for help** with the links at the beginning of this document..._

 _****_

## A little fix to our AWS Launch Template

A little glitch caught my 👀 when we launched our server, that you'll find just below the pink highlight for Source/dest. check on the screenshots of the EC2 console :

T2/T3 Unlimited mode : _**Enabled**_

**This is not a setting we want for our use case**, as it may introduce costs \(though that is very unlikely, OpenVPN using a very low percentage of CPU\).

Anyway, this came out because in the AWS region we used, we did not have the usual "free tier elligible" t2.micro instance type but the t3.micro instead, and i found this note in AWS EC2 documentation which explains this unexpected \(at least for me\) setting :

![](../.gitbook/assets/image%20%283%29.png)

Just to be sure, we'll disable this setting and that will give us an opportunity to experiment with creating a new version of our Launch Template.

### The Launch Template v2

We go to the Launch Templates section of the EC2 Console and click "**Create launch template**" again, but this time we :

* select _**Create a new template version**_
* select our existing template
* enter a version description
* select the existing version as source

![New template version, select our template...](../.gitbook/assets/image%20%2842%29.png)

![...enter a version description and select original version as source](../.gitbook/assets/image%20%2878%29.png)

Then we go to the bottom, expand **Advanced details** and put the **T2/T3 Unlimited** setting to _**Disable**_ :

![Disable T2/T3 Unlimited](../.gitbook/assets/image%20%28120%29.png)

And now we can click the final "_**Create launch template**_" button :

![](../.gitbook/assets/image%20%281%29.png)

Here we are :

![](../.gitbook/assets/image%20%28127%29.png)

Now let's **make this new version the default** version of our Launch Template, because _**the original one is currently still the default one**_ :

![](../.gitbook/assets/image%20%2849%29.png)

So we check the line is selected \(blue square on the left\) and use the **Actions** button :

![](../.gitbook/assets/image%20%2833%29.png)

Nothing mysterious there...

![](../.gitbook/assets/image%20%2868%29.png)

Finally we're done :

![](../.gitbook/assets/image%20%2835%29.png)



