# Step 4 : Configure your AWS environment -WIP

{% hint style="warning" %}
If you have not already done so, here comes the time for you to follow that [account creation process info](../proposed-solution/amazon-web-services.md#how-to-create-an-aws-account), and especially take a few minutes to **watch the small video linked there, including the part for creating your first IAM user** \(**IAM** = Identity & Access Management\).
{% endhint %}

{% hint style="success" %}
**Create your first IAM user as an administrator** ASAP as it will enable you to avoid using your "root account".
{% endhint %}

{% hint style="info" %}
**PLEASE ! Create an administrative IAM user for yourself** in your new AWS account and **after that avoid using your "root account"** to manage your AWS environment.

That's an "AWS best practice" for a reason...
{% endhint %}

There are _**hundreds of videos and articles all over the Internet**_ about that "_create aws account_" topic, so feel free to watch and compare...just beware to select recent enough ones \( &lt; 1 year old is usually ok\).



_**I'll now wait for you to come back with your shiny admin login to this brand new AWS account....**_

...

👀 

...

👀 

...

😁 **Ah ! There you are ! And you're an Admin ! And you're at the AWS Management Console ! Great !**

![The AWS Management Console](../.gitbook/assets/image%20%2840%29.png)

This is the "home" of your management console : you can **manage your account** with the menu titled with your "IAM username" @ "account name", **choose an "AWS Region"** with the menu to the right of it, access to the management of all sorts of AWS services.

_Each AWS service has its own "console" and you can have **multiple "service consoles" in different tabs** of your web browser, which is convenient when you have to copy/paste from one to the other._

{% hint style="info" %}
If possible choose **an AWS Region near you**, to benefit from the lowest possible latency \(=network delay\) between your home and your OpenVPN server.
{% endhint %}

Many AWS services operate at the AWS Region level and you can have very different settings from region to region; some services like IAM or Billing are "global" and have no region selection.



**We're going to prepare once and for all your \(extremely simple\) "personal virtual infrastructure"** inside the AWS cloud, using several AWS services, mainly :

* **IAM = Identity & Access Management** : this service is used by all others to manage security
* **VPC = Virtual Private Cloud** : your own networks which together form a "private cloud" in the AWS cloud
* **EC2 = Elastic Cloud Computing** : your virtual servers
* **S3 = Simple Storage Service** : your virtual file storage



### IAM Role \(region-agnostic\)

The first "service console" we'll open is the **IAM console** where you'll find the "**Role**" option in the menu to the left :

![](../.gitbook/assets/image%20%2812%29.png)

Choose this link, then use the big blue "**Create Role**" button at the top...

![](../.gitbook/assets/image%20%2861%29.png)

Click :

* the "**AWS service**" block under _**Select Type of trusted entity**_
* the "**EC2**" block under _**Choose the service that will use this role**_

Then click the big blue "**Next: Permissions**" button at the bottom...

![](../.gitbook/assets/image%20%2834%29.png)

_I already have some policies and roles defined in this account, which is why I had to blur specifics_

Use the search filter box above the list of **permission policies** to locate and **put a check mark** on these 2 policies provided by AWS :

* **AmazonEC2FullAccess** \(search for "ec2fu"\)

![](../.gitbook/assets/image%20%2857%29.png)

* **AmazonS3FullAccess** \(search for "s3fu"\)

![](../.gitbook/assets/image%20%281%29.png)

Then click the blue "**Next: Tags**" button at the bottom...

![](../.gitbook/assets/image%20%2844%29.png)

We don't need tagging, so just click the blue "**Next: Review**" button at the bottom...

![](../.gitbook/assets/image%20%2863%29.png)

**Give a name** to your new role : i suggest **`my-fg-ovpn-ec2role`** as shown above.

{% hint style="success" %}
Then hit the blue "**Creale Role**" button and you're done.
{% endhint %}

![Your shiny new IAM Role](../.gitbook/assets/image%20%283%29.png)

{% hint style="info" %}
You have just created an **IAM Role**, that you'll **assign to your OpenVPN server** later on, so that it can **access the EC2 and S3 services on your behalf, without restriction**.
{% endhint %}

Your server will need that to :

* auto-configure itself during startup \(setting network options in the EC2 service\)
* download configuration parameters and scripts from S3
* without the need of passwords \(=&gt; 👍 **safe parameter files and scripts** 👍 \)

_**Note:** This IAM Role could have been defined with restricted custom-specified permissions, both in EC2 and S3, rather than "Full Access", but that would be too complex to explain here.  Also if you connect to your server in an interactive terminal, you'll be happy to access the full EC2 and S3 APIs from there without restriction or needing an access key or password._



### Choosing the AWS Region

Starting from here, let's choose an AWS Region as our playground.  The **geographically closest to you** is the best because that will usually give **minimal latency** \(=delay\) in network traffic between you and your server.

For this demo I'll choose Stockholm, an AWS region which is "virgin territory" for me \(I have never used it before\) :

![](../.gitbook/assets/image%20%2853%29.png)



### S3 bucket

Let's first create our storage space for parameters and scripts : an S3 bucket.

{% hint style="danger" %}
**KEEP IT PRIVATE !!   KEEP IT PRIVATE !!   KEEP IT PRIVATE !!   KEEP IT PRIVATE !!**   
{% endhint %}

👆 _Do you need me to repeat, or is that clear enough ?_👆 

So let's go to the **S3 console** \(S3 is in the _**Storage**_ section on the main console listing all services\) :

![](../.gitbook/assets/image%20%2849%29.png)

You can guess we'll click the blue "Create bucket" button...

![](../.gitbook/assets/image%20%2864%29.png)

**Step 1 : Give it a name,** which must conform to some name restrictions, and be unique \(so you may need to change it if your preferred name is already used by someone else\), check the **region**, then hit "**Next**"...

![](../.gitbook/assets/image%20%2832%29.png)

**Step 2 :** let's **NOT** use any option for now and simply click "**Next**"...

![](../.gitbook/assets/image%20%2835%29.png)

👇 _If you already forgot..._ 👇 

{% hint style="danger" %}
**KEEP IT PRIVATE !!   KEEP IT PRIVATE !!   KEEP IT PRIVATE !!   KEEP IT PRIVATE !!**   
{% endhint %}

**Step 3 :** So let's **keep the security tight** and "**Block** _**all**_ **public access**" as is the default, and hit "**Next**"...

![](../.gitbook/assets/image%20%2845%29.png)

A quick review of our choices, then click "**Create bucket**"...

![](../.gitbook/assets/image%20%2843%29.png)

{% hint style="success" %}
**...and here is your own private bucket, reporting for duty !**
{% endhint %}



### Virtual Private Cloud : a quick overview









