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

![The AWS Management Console](../.gitbook/assets/image%20%2858%29.png)

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



