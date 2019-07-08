# Step 5 : Transfer to S3

So, we want to transfer the whole content of our my-fg-ovpn-s3 folder to our S3 bucket : we can do it "manually" in AWS **S3 Console**, and we can also do it in one go with **CloudBerry Explorer for Amazon S3** \(I'll call it CBES3\).

While **manual upload** with the S3 Console is straightforward \(with the "_**Upload**_" and "_**Create Folder**_" very obvious buttons\), it **is a bit tedious**, so I **prefer using CBES3**, especially as it enables me to overwrite files easily, manage my bandwidth \(Tools/Options menu\) and even "sync" changes if I want - _which I generally don't as I prefer to keep initiative on what is transfered when and where..._

## Define account in CBES3

To use CBES3 we need an [**AWS user authorized on our S3 bucket with** AWS "**Access Key ID + Secret Key**"](../appendixes/aws-user-with-api-keys.md), to **define a "**_**Storage Account**_**" in CBES3** :

![](../.gitbook/assets/image%20%287%29.png)

We'll give a _**Display name**_ to our S3 "storage account" and our AWS _**Access Key**_ and _**Secret Key**_ :

![](../.gitbook/assets/image%20%28139%29.png)

A quick click on "_**Test Connection**_" to check the access is granted, then the _**OK**_ button takes us to our list of _**Registered Accounts**_, that we can close.

![](../.gitbook/assets/image%20%2866%29.png)

## The "Double Explorer" GUI

On the **left side** of the main GUI window, after a 🔄 **refresh of "my computer"**, we navigate to **our source folder** :

![](../.gitbook/assets/image%20%28163%29.png)

On the **right side** we select our **S3 "storage account"**, which will show us our single, lonely, empty bucket :

![](../.gitbook/assets/image%20%284%29.png)

Let's **double-click it** and we'll be **ready to drag-and-drop** :

![](../.gitbook/assets/image%20%2845%29.png)

{% hint style="success" %}
**==&gt; S3 bucket ready !**
{% endhint %}

![](../.gitbook/assets/image%20%28120%29.png)

The result as seen in the AWS S3 Console :

![](../.gitbook/assets/image%20%28151%29.png)







