# Amazon Web Services

[AWS](https://aws.amazon.com/) provides cloud hosting services at the **IaaS** \(**Infrastructure as a Service**\) level. Leading rivals in that area include the [Google Compute Engine](https://cloud.google.com/compute/) and [Microsoft Azure](https://azure.microsoft.com/) offers.

> _I won't go into the war between these competitors \(and others\).  Being an AWS user since 2014, I am proficient enough  with the AWS platform to design a reliable and secure "end-user" setup.  Competing offers can provide the equivalent of what I'll show here but I'm not proficient enough with them..._

**IaaS** means you can create your virtual environment in the cloud, hosted within AWS infrastructure, **"exactly" the way you want** : networks, subnets, private or public, in multipe geographic locations, with servers and databases of all sizes and types, with or without Internet access, network traffic filtering, etc...

"_exactly the way you want_" implies some measure of **"do it yourself"** and entails a **not-so-easy learning curve** to master the ins and outs of the AWS platform \(just like all other IaaS providers\).  So we'll use a **simple but secure setup**.

**AWS uses a "pay as you go" economic model, and provides a nice 1 year Free Tier + some on-going goodies after the first year.**

{% hint style="info" %}
All the details about the **AWS Free Tier and it's limits** can be found at [https://aws.amazon.com/free](https://aws.amazon.com/free)
{% endhint %}

{% hint style="warning" %}
To become an AWS user you have to **create an account** \(even if only to use the Free Tier\).
{% endhint %}



### How to create an AWS account

Go check and follow the process here : [https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)

Or simply click "**Create Free Account**" at the bottom of the main AWS page at : [https://aws.amazon.com](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)

Also here is a link to a[ recent enough video showing both](https://youtu.be/W-udN-8qTHE) :

* new AWS account creation
* first IAM user creation in the new account

AWS Account creation videos : [https://www.youtube.com/results?search\_query=create+aws+account](https://www.youtube.com/results?search_query=create+aws+account)



### AWS Free Tier

{% hint style="success" %}
The AWS Free tiers provides a new subscriber with everything needed to run this solution free of charge for 1 year.
{% endhint %}



### AWS potential costs : how to avoid them ?

It is extremely easy to create I.T. "resources" on AWS with a few clicks...which implies that you may try things out and not check if they remain within the Free Tier bounds.

_On the other hand, associated with the "pay as you go" pricing, it offers the opportunity to test high-end configurations for a short time at a low cost without a large upfront investment - provided you remember to destroy \("terminate"\) these resources after that to avoid ongoing costs._

So keep this in mind :

* the _Free Tier **bounds**_ are specified, detailed by service, at : [https://aws.amazon.com/free](https://aws.amazon.com/free)
* **always check the cost structure for any service you may want to test** \(free part/billable part/invoicing criteria\)
* **ALL pricing info is available** per service, or from the "umbrella" pricing page : [https://aws.amazon.com/pricing/](https://aws.amazon.com/pricing/)
* **Each AWS Region \(geographical area\) has it's own costs.** From what i saw, _South America is currently \(mid 2019\) more expensive than other regions_.  You may take this into account when choosing the place where you'll run your own server
* **Periodically check** your month-to-date actuals and month-total estimated values in your **Billing Dashboard** in the AWS Management Console.  They are updated at intervals of 6 to 8 hours : 

![Billing Dashboard shortcut, near the top-right corner in the AWS console](../.gitbook/assets/image%20%2815%29.png)



### Shrinking AWS costs when you step outside the Free Tier

{% hint style="warning" %}
**1 year after account creation, some bonuses from the AWS Free Tier expire, including those that enabled running our proposed setup for free.  So the cost will mandatorily become &gt;$0 /month.**
{% endhint %}

{% hint style="success" %}
**However this solution is designed to be lightweight, and that reflects in it's potential running cost when NOT operating free of charge :**
{% endhint %}

* the total permanent volume of **S3 sorage** is around 20KB, which **should remain virtually free** _\(maybe $0.01 if you have A LOT of startups/boots in a month ?\)_
* with the **"destroy when not used" strategy,** and a **minimal virtual disk** size \(8GB\), you should **extremely reduce** both **EC2 and EBS costs** \(EC2=Elastic Compute Cloud : your CPU\(s\)+RAM, and EBS=Elastic Block Store : your virtual disk\(s\)\)
* **OpenVPN requirements are extremely low** : you can run it with the **cheapest EC2 instance type**

{% hint style="info" %}
Overall, **WITHOUT the Free Tier goodies**, you should be able to run your VPN server for **50 Hours per month for less than $0.5 /month and a permanently running \(24x7\) server should cost $5 to $10 depending on the world region**
{% endhint %}



### Mobile Management App

This is something I like : when on the move I can use the mobile app to check statues or start/stop AWS servers etc...

It does **not** provide _everything_ you can do with the main AWS Management Console, though.



