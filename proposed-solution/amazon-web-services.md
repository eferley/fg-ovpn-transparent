# Amazon Web Services

[AWS](https://aws.amazon.com/) provides cloud hosting services at the **IaaS** \(**Infrastructure as a Service**\) level. Leading rivals in that area include the [Google Compute Engine](https://cloud.google.com/compute/) and [Microsoft Azure](https://azure.microsoft.com/) offers.

_I won't go into the war between these competitors \(and others\).  Being an AWS user since 2014, I am proficient enough  with that platform to design a reliable "end-user" setup.  Competing offers can provide the equivalent of what I'll show here but I'm not proficient enough with them..._



**IaaS** means you can create your virtual environment in the cloud, hosted within AWS infrastructure, "exactly" the way you want : networks, subnets, private or public, in multipe geographic locations, with servers and databases of all sizes and types, with or without Internet access, network traffic filtering, etc...

"_exactly the way you want_" implies some measure of "do it yourself" and entails a **not-so-easy learning curve** to master the ins and outs of the AWS platform \(just like other IaaS providers\).

**AWS uses a "pay as you go" economic model, and provides a nice 1 year Free Tier + some on-going goodies after the first year.**

All the details about the **AWS Free Tier and it's limits** can be found at [https://aws.amazon.com/free](https://aws.amazon.com/free)

To become an AWS user you have to **create an account** \(even if only to use the Free Tier\).

### How to create an AWS account

Go check and follow the process here : [https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)

Or simply cick "Sign up" on the main AWS page at : [https://aws.amazon.com](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)



### AWS Free Tier

{% hint style="success" %}
The AWS Free tiers provides a new subscriber with everything needed to run this solution free of charge for 1 year.
{% endhint %}



### AWS potential costs : how to avoid them ?

It is extremely easy to create I.T. "resources" on AWS with a few clicks...which implies that you may try things out and not check if they remain within the Free Tier bounds.

_On the other hand it enables people to try out some high-end configurations for several hours at a low one-time cost without a large upfront investment, provided they destroy \("terminate"\) these resources after the test to avoid ongoing costs._

So keep this in mind :

* the _Free Tier **bounds**_ are specified, detailed by service, at : [https://aws.amazon.com/free](https://aws.amazon.com/free)
* **always check the cost structure for any service you may want to test** \(free part/billable part/invoicing criteria\)
* **ALL pricing info is available** per service, or from the "umbrella" pricing page : [https://aws.amazon.com/pricing/](https://aws.amazon.com/pricing/)
* **Each AWS Region \(geographical area\) has it's own costs.** From what i saw, _South America is currently \(mid 2019\) more expensive than other regions_.  You may take this into account when choosing the place where you'll run your own server.



### Shrinking AWS costs when you go outside the Free Tier

{% hint style="warning" %}
**1 year after account creation, some bonuses from the AWS Free Tier expire, including those that enabled running our proposed setup for free.  So the cost will mandatorily become &gt;$0 /month.**
{% endhint %}

{% hint style="success" %}
**However this solution is designed to be lightweight, and that reflects in it's potential cost when NOT operating free of charge :**
{% endhint %}

* the total permanent volume of **S3 sorage** is around 20KB, which **should remain virtually free** \(maybe $0.01 if you have a lot of startups/boots in a month ?\)
* with the **"destroy when not used" strategy,** and a **minimal virtual disk** size \(8GB\), you should **extremely reduce** both **EC2 and EBS costs** \(EC2=Elastic Compute Cloud : your CPU\(s\)+RAM and EBS=Elastic Block Store : your virtual disks\)
* OpenVPN requirements are extremely low : you can run it with the **cheapest EC2 instance type**

{% hint style="info" %}
Overall, **WITHOUT the Free Tier goodies**, you should be able to run your VPN server for **50 Hours per month for less than $0.5 /month and a permanently running \(24x7\) server should cost $5 to $10 depending on the world region**
{% endhint %}



