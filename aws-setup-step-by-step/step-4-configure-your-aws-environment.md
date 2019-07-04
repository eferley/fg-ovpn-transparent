# Step 4 : Configure your AWS environment -WIP

### ![](../.gitbook/assets/zeferby_dino_64.png) Welcome to Ze cloud 🌥 ⛈ 🌤 

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

There are _**hundreds of videos and articles all over the Internet**_ about that "_create aws account_" topic, so feel free to watch and compare...just beware to select recent enough ones \( &lt; 1 year old is usually ok\).



_I'll now wait for you to come back with your shiny **admin login** to this brand new **AWS account**...._

...

👀 

...

👀 

...

😁 **Ah ! There you are ! And you're an Admin ! And you're at the AWS Management Console !  
Great ! \(also, but**©**Zacchaeus : Jolly Good !\)**

![The AWS Management Console](../.gitbook/assets/image%20%2848%29.png)

This is the "home" of your AWS management console : you can **manage your account** with the menu titled with your "IAM username" @ "account name", **choose an "AWS Region"** with the menu to the right of it, and access the management consoles for all AWS services.

_Each AWS service has its own "console" and you can have **multiple "service consoles" in different tabs** of your web browser, **which is convenient**, especially if you have to copy/paste or cross-check infos from one to the other._

Many AWS services operate at the "AWS Region" level and you can choose very different settings from region to region; some services like IAM or Billing are "global" and have no region selector.



**We're going to prepare once and for all your "personal virtual infrastructure"** inside the AWS cloud, using several AWS services, mainly :

* **IAM = Identity & Access Management** : this service is used by all others to manage permissions
* **EC2 = Elastic Cloud Computing** : your virtual servers
* **S3 = Simple Storage Service** : your virtual file storage

We'll also indirectly use this service, which you won't need to check because the defaults are ok for us.

* **VPC = Virtual Private Cloud** : your own networks, which together form a "private cloud" in the AWS cloud

_This "personal infrastructure" will be **extremely simple** but **we'll take precautions** to avoid introducing any "security hole"..._



### What we'll do in this section :

* create an AWS "**IAM Role**" for your server to access AWS services during startup
* choose an "**AWS Region**"
* create an AWS "**S3 bucket**" for your server to auto-configure during startup
* have a quick look at your AWS "**Default VPC**"
* create an AWS "**Key pair**" \(similar to our \[certificate+key\] pairs in our own PKI\), for you to access your server interactively if you need
* create an AWS "**Security Group**" for your server \(a kind of firewall rule\)
* "**fill the bucket**" : organize and transfer our security and configuration files into the S3 bucket
* create an AWS EC2 "**Launch Template**" : a set of option choices defining a kind of "server model", that you'll use later on, to repeatedly launch the server itself in the quickest possible way \(since you'll "terminate" =destroy the server after each session\)

Then we'll be ready to test our setup.



### Creating the IAM Role \(region-agnostic\)

The first "service console" we'll open is the **IAM console** where you'll find the "**Role**" option in the menu to the left :

![](../.gitbook/assets/image%20%2816%29.png)

Choose this link, then use the big blue "**Create Role**" button at the top...

![](../.gitbook/assets/image%20%2871%29.png)

Click :

* the "**AWS service**" block under _**Select Type of trusted entity**_
* the "**EC2**" block under _**Choose the service that will use this role**_

Then click the big blue "**Next: Permissions**" button at the bottom...

![](../.gitbook/assets/image%20%2841%29.png)

_I already have some policies and roles defined in this account, which is why I had to blur specifics_

Use the search filter box above the list of **permission policies** to locate and **put a** ✅ **check mark** on these 2 policies provided by AWS :

* **AmazonEC2FullAccess** \(search for "ec2fu"\)

![](../.gitbook/assets/image%20%2866%29.png)

* **AmazonS3FullAccess** \(search for "s3fu"\)

![](../.gitbook/assets/image%20%284%29.png)

Then click the blue "**Next: Tags**" button at the bottom...

![](../.gitbook/assets/image%20%2852%29.png)

We don't need tagging, so just click the blue "**Next: Review**" button at the bottom...

![](../.gitbook/assets/image%20%2874%29.png)

**Give a name** to your new role \(i suggest **`"configuration name"-ec2role`** as shown above\) and verify you have selected the right policies.

{% hint style="success" %}
Then hit the blue "**Creale Role**" button and you're done.
{% endhint %}

![Your shiny new IAM Role](../.gitbook/assets/image%20%286%29.png)

{% hint style="info" %}
You have just created an **IAM Role**, that you'll **assign to your OpenVPN server** later on, so that it can **access the EC2 and S3 services on your behalf at startup, without restriction**.
{% endhint %}

Your server will need that to :

* auto-configure itself during startup \(setting network options in the EC2 service\)
* download configuration parameters and scripts from S3
* without the need of passwords \(=&gt; 👍 **safe parameter files and scripts** 👍 \)

_**Note:** This IAM Role could have been defined with restricted custom-specified permissions, both in EC2 and S3, rather than "Full Access", but that would be too complex to explain here.  Also if you connect to your server in an interactive terminal, you'll be happy to access the full EC2 and S3 APIs from there without restriction or needing an access key or password._



### Choosing the AWS Region

Starting from here, let's choose an AWS Region as our playground.  **The geographically closest to you is usually the best because it should give minimal latency** \(=delay\) **in network traffic** between you and your server.

For this demo I'll choose Stockholm \(called eu-north-1\), an AWS region which is "virgin territory" for me \(I have never used it before\) :

![](../.gitbook/assets/image%20%2861%29.png)



### Creating the S3 bucket

Let's create our storage space for parameters and scripts : an S3 bucket.

{% hint style="danger" %}
**KEEP IT PRIVATE !!   KEEP IT PRIVATE !!   KEEP IT PRIVATE !!   KEEP IT PRIVATE !!**   
{% endhint %}

👆 _Do you need me to repeat, or is that clear enough ?_👆 

So let's go to the **S3 console** \(S3 is in the _**Storage**_ section on the main console listing all services\) :

![](../.gitbook/assets/image%20%2857%29.png)

You can guess 🙄 we'll click the blue "**Create bucket**" button...

![](../.gitbook/assets/image%20%2875%29.png)

**Step 1 : Give it a name,** which must conform to some name restrictions, and be **unique** \(so you may need to change it if your preferred name is already used by someone else\), check the **region**, then hit "**Next**"...

![](../.gitbook/assets/image%20%2838%29.png)

**Step 2 :** let's **NOT** use any option and simply click "**Next**"...

![](../.gitbook/assets/image%20%2843%29.png)

👇 _If you already forgot..._ 👇 

{% hint style="danger" %}
**KEEP IT PRIVATE !!   KEEP IT PRIVATE !!   KEEP IT PRIVATE !!   KEEP IT PRIVATE !!**   
{% endhint %}

**Step 3 :** Let's **keep the security tight** and "**Block** _**all**_ **public access**" as is the default, and hit "**Next**"...

![](../.gitbook/assets/image%20%2853%29.png)

A quick review of our choices, then click "**Create bucket**"...

![](../.gitbook/assets/image%20%2851%29.png)

{% hint style="success" %}
**...and here is your own** _**private**_ **bucket, reporting for duty !**
{% endhint %}



### Virtual Private Cloud

#### Quick overview 🚁 

> **This is mostly Tech blurb...**
>
> **If you want to skip, jump to "Default VPC"**

A VPC is a set of private IP networks all part of the same private "address space" that belongs to you :

* in **one AWS Region** \(Stockholm : eu-north1\)
* spread over **several AZs = AWS "Availability Zones"** \(Stockholm : 3 AZs = eu-north-1a/1b/1c\)
* sharing some of their security and network settings
* with or without public IPs attached to the servers in the networks

The "private address space" mentioned above means that all your networks in a VPC will be part of a contiguous large range of private IP addresses belonging to you _\(recently added AWS feature : you can now even add more ranges of private IPs if the first one is not enough\)_

The **VPC private IP range** is a block \(of max. "/16" = 64K = 65536 IPs\) **contained within one of these** :

* **192.168.0.0 to 192.168.255.255**, which is noted **192.168.0.0/16** \(that's called a "CIDR block"\)
* **172.16.0.0 to 172.31.255.255**, which is noted **172.16.0.0/12**
* **10.0.0.0 to 10.255.255.255**, which is noted **10.0.0.0/8**

An **AWS Region** is made of **several AZs = AWS Availability Zones** which you can consider as multiple distinct datacenters, near enough each other that they are all **interconnected on VERY high speed** fiber optics connections.

So a VPC in one region is split in multiple interconnected IP "sub-networks" \(at least 1 for each AZ\). When you create a VPC, you can also choose to split it into more subnets, each with some specific characteristics :

* with/without Internet access
* reachable from the Internet, or not \(servers also get a public IP, or they don't\)
* etc...

If you want to know more about AWS VPCs, please [browse the docs](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html).

#### Default VPC

When you created your AWS account, your were automatically provided a Default VPC in each region, and you can see how it is defined in the "**VPC Console**" \(in the _**Networking & Content Delivery**_ section\).

{% hint style="success" %}
**We'll use the most possible basic setup, relying on the Default VPC that AWS automatically provides for you in any AWS Region.**
{% endhint %}

This default AWS VPC in your region of choice perfectly fits the bill to host our lonely little OpenVPN server, so we'll use it "as is".

For the moment, just use the "VPC Console" to **take a quick look at your default VPC and make a note of its private IPv4 block** :

![Note your Default VPC IPv4 CIDR block](../.gitbook/assets/image%20%282%29.png)



### Creating the AWS Key Pair

An AWS key pair \(public+private key\) is mandatory to access a virtual server and must exist or be created at launch time.

If you don't intend to interactively connect to your server, you will not use it, but it has to be created nevertheless, so let's head over to the **EC2 Console** \(EC2 is in the _**Compute**_ section\); I highlighted the different sections that we'll use here :

![](../.gitbook/assets/image.png)

So let's go to "**Key Pairs**", and click the tempting "**Create Key Pair**" button :

![](../.gitbook/assets/image%20%2888%29.png)

Enter a **Key pair name** of your choice as above and click "**Create**"...

AWS will create a random public+private key pair for you, and your web browser should automatically download the file to your machine :

![](../.gitbook/assets/image%20%283%29.png)

{% hint style="warning" %}
**Lets move it** from our _Downloads_ folder _****_**to somewhere we have a chance to find it in the future !**...
{% endhint %}

I'll store it in my **`T:\fg-ovpn\MY-FG-OVPN`** folder :

![Keeping our AWS key pair safe](../.gitbook/assets/image%20%2815%29.png)

We do not plan to use it, but if needed, it's safe here !



### Creating an AWS Security Group

Back to the **AWS EC2 Console**, let's go to the "**Security Groups**" area...it's the **equivalent of a set of firewall rules** for your VPC. \(_the AWS infrastructure acts like a gigantic NAT/PAT router and firewall in front of your virtual servers in any AWS region/VPC/private network_\)

You will find a "_default VPC security group_" there, which authorizes all internal network traffic within your VPC \("source" = every member of the security group itself\), and outbound traffic to anywhere \("destination" = 0.0.0.0/0\).

We'll create a _**specific**_ **Security Group** for our **OpenVPN + Fantasy Grounds** requirements \(and SSH access if you whish\), using the big blue "**Create Security Group**" button :

![](../.gitbook/assets/image%20%2823%29.png)

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

![](../.gitbook/assets/image%20%2840%29.png)

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

![](../.gitbook/assets/image%20%2842%29.png)

As soon as you select "**My IP**" as a source or destination, it is replaced with your **current Public IPv4 as seen from the Internet**, which is why I blurred mine, _even though it's actually not a risk..._

Ok, after playing with rules for a moment, let's just clean up and finish with **our required Security Group Rules** for **FG + OpenVPN access from any IPv4 address**, then finally click the "**Save**" button :

![Our required S.G. rules for FG + OpenVPN from any IPv4](../.gitbook/assets/image%20%2827%29.png)

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





