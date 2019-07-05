# Step 4 : Your AWS environment

## ![](../.gitbook/assets/zeferby_dino_64.png) Welcome to Ze cloud 🌥 ⛈ 🌤 

{% hint style="warning" %}
If you have not already done so, here comes the time for you to follow that [account creation process info](../proposed-solution/amazon-web-services.md#how-to-create-an-aws-account), and especially take a few minutes to watch the small video linked there, **including the part for creating your first IAM user** \(**IAM** = Identity & Access Management\).
{% endhint %}

{% hint style="success" %}
**Create your first IAM user as an administrator** ASAP as it will enable you to avoid using your "root account".
{% endhint %}

{% hint style="info" %}
**PLEASE ! Create an administrative IAM user for yourself** in your new AWS account and **after that, avoid using your "AWS root account"** to manage your environment.

_That's an "AWS best practice" for a reason..._
{% endhint %}

There are [_**hundreds of videos**_](https://www.youtube.com/results?search_query=create+aws+account) _**and articles all over the Internet**_ about that "_create aws account_" topic, so feel free to watch and compare...just beware to select recent enough ones \( &lt; 1 year old is usually ok\).



\_\_![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) _I'll now wait for you to come back with your shiny **admin login** to this brand new **AWS account**...._

...

👀 

...

👀 

...

😁 **Ah ! There you are ! And you're an Admin ! And you're at the AWS Management Console !  
Great ! \(**_**also, but**_©_**Zacchaeus : Jolly Good !**_**\)**

![The AWS Management Console](../.gitbook/assets/image%20%2850%29.png)

This is the "home" of your AWS management console : you can **manage your account** with the menu titled with your "IAM username" @ "account name", **choose an "AWS Region"** with the menu to the right of it, and **access the service management consoles** for all AWS services.

Each AWS service has its own "console" and you can have **multiple "service consoles" in different tabs** of your web browser, **which is convenient**, especially if you have to copy/paste or cross-check infos from one to the other.

Many AWS services operate at the "AWS Region" level and you can choose very different settings from region to region; some **services like IAM or Billing are "global"** and have no region selector.



**We're going to prepare once and for all your "personal virtual infrastructure"** inside the AWS cloud, using several AWS services, mainly :

* **IAM = Identity & Access Management** : this service is used by all others to manage permissions
* **EC2 = Elastic Cloud Computing** : your virtual servers
* **S3 = Simple Storage Service** : your virtual file storage

We'll also indirectly use this service, which you won't need to check because the defaults are ok for us.

* **VPC = Virtual Private Cloud** : your own networks, which together form a "private cloud" in the AWS cloud

_This "personal infrastructure" will be **extremely simple** but **we'll take precautions** to avoid introducing any "security hole"..._



## What we'll do in this section :

1. create an AWS "**IAM Role**" for your server to access AWS services during startup
2. choose an "**AWS Region**" to build our mini-infrastructure
3. create an AWS "**S3 bucket**" for your server to auto-configure during startup
4. have a quick look at your AWS "**Default VPC**"
5. create an AWS "**Key pair**" \(similar to our \[certificate+key\] pairs in our own PKI\), for you to access your server interactively if you need
6. create an AWS "**Security Group**" for your server \(a kind of firewall rule\)
7. "**fill the bucket**" : organize and transfer our security and configuration files into the S3 bucket
8. create an AWS EC2 "**Launch Template**" : a set of option choices defining a kind of "server model", that you'll use later on, to repeatedly launch the server itself in the quickest possible way \(since you'll "terminate" =destroy the server after each session\)

**Then we'll be ready to test our setup.**









### Creating an AWS Security Group

Back to the **AWS EC2 Console**, let's go to the "**Security Groups**" area...it's the **equivalent of a set of firewall rules** for your VPC. \(_the AWS infrastructure acts like a gigantic NAT/PAT router and firewall in front of your virtual servers in any AWS region/VPC/private network_\)

You will find a "_default VPC security group_" there, which authorizes all internal network traffic within your VPC \("source" = every member of the security group itself\), and outbound traffic to anywhere \("destination" = 0.0.0.0/0\).

We'll create a _**specific**_ **Security Group** for our **OpenVPN + Fantasy Grounds** requirements \(and SSH access if you whish\), using the big blue "**Create Security Group**" button :

![](../.gitbook/assets/image%20%2824%29.png)

Give it a **Security group name** and a **Description**, your \(only\) default VPC should already be selected, and then let's have a look at the **Security group rules** panel.



#### **Outbound rules** tab :

![Authorized Outbound traffic : All / Anywhere](../.gitbook/assets/image%20%2868%29.png)

This shows that the members \(virtual servers\) of that Security Group will be authorized to **send network packets of any kind, to anywhere** : no restriction, freedom, Yeah ! 🆓 

> **Tech blurb:**
>
> This is required, because your server will need to send packets :
>
> * from its own TCP 1802 port to your players on unknown/volatile TCP ports at unknown/volatile IPs
> * from its own UDP 1194 port to you : unknown/volatile UDP port, probably unknown/volatile IP
>
> **The only "known" thingies here are on this server's side**



#### Inbound rules tab :

Using the "**Add Rule**" button, let's add 2 inbound rules, to authorize incoming network traffic, one using the "**Custom TCP rule**" type for FG, and the other the "**Custom UDP rule**" type for OpenVPN, like this :

![](../.gitbook/assets/image%20%2842%29.png)

...and let's click the blue "**Create**" button.

{% hint style="success" %}
You now have a "working" dedicated **Security Group** for your FG-OpenVPN setup - _which also works if you are in a hotel room, etc..._
{% endhint %}

...but it's not perfect, so **let's modify it.**



#### You can always change the rules...

._**..even when they are currently in use by running servers**_ \(which is cool for tests, by the way\).

If we look at the Inbound rules for our newly created Security Group :

![](../.gitbook/assets/image%20%2862%29.png)

We find some IPv6 rules there \(Source = ::/0\) that we don't want \(that's because we used the "Anywhere" destination or source, which is both IPv4 + IPv6\). So we'd like to **delete** these 2 lines.

Also maybe we would like to **add** an authorization rule for ourselves \(the current Public IPv4 of our home Internet access\) to access our server for interactive terminal connection through SSH \(=TCP 22\).

So let's click the "**Edit**" button on the **Inbound** tab...We can do all of that here :

![Red: delete rule, Orange : add rule, Blue : change some details](../.gitbook/assets/image%20%2873%29.png)

Let's delete the 2 useless IPv6 rules, and add an SSH rule \(Type=SSH =&gt; TCP 22\) for "My IP"...

![](../.gitbook/assets/image%20%2844%29.png)

As soon as you select "**My IP**" as a source or destination, it is replaced with your **current Public IPv4 as seen from the Internet**, which is why I blurred mine, _even though it's actually not a risk..._

Ok, after playing with rules for a moment, let's just clean up and finish with **our required Security Group Rules** for **FG + OpenVPN access from any IPv4 address**, then finally click the "**Save**" button :

![Our required S.G. rules for FG + OpenVPN from any IPv4](../.gitbook/assets/image%20%2828%29.png)

{% hint style="info" %}
**You can always come back to change Security Group Inbound and Outbound rules**.
{% endhint %}

{% hint style="warning" %}
**Just don't delete the Security Group itself**, so that references to it **continue to be valid** \(see the _**Launch Template**_ further on\).
{% endhint %}

{% hint style="success" %}
_\*\*\*\*_![](../.gitbook/assets/zeferby_dino_64%20%282%29.png) _**Ze Summary for our Security Group Rulez :**_

* **Outbound** : **ALL** Traffic **to ALL** IPv4
* **Inbound** : **TCP 1802 from ALL** IPv4 \(for FG\)
* **Inbound** : **UDP 1194 from ALL** IPv4 \(for OpenVPN\)
{% endhint %}

{% hint style="warning" %}
IF you absolutely want to, you **can** restrict OpenVPN inbound rule to your own Public IPv4, **BUT then** you must **be prepared to update your OpenVPN inbound rule** :

* every time your **home public IPv4 address changes**
* every time you want to **host a game from any other place** \(hotel, etc...\)
{% endhint %}



### Filling the bucket





### Building the Launch Template





