# Step 0 : Preparation

Let's begin with 2 things : 

## **Create/have an** [**account available on AWS**](../proposed-solution/amazon-web-services.md#how-to-create-an-aws-account)\*\*\*\*

...as previously explained.

## Check that you know :

* [how to open a command line](https://www.digitalcitizen.life/7-ways-launch-command-prompt-windows-7-windows-8) with your version of Windows
* [how to show file extensions](https://www.howtohaven.com/system/show-file-extensions-in-windows-explorer.shtml) in Windows Explorer

**It may also be a good idea to know** [how to show hidden/system files](https://helpx.adobe.com/x-productkb/global/show-hidden-files-folders-extensions.html) in Windows Explorer as well...

## **Prepare a "safe" place on your own machine.**

{% hint style="warning" %}
**If you can, create that on another drive than your system C: drive, and of course NOT in a kind of "recovery" drive/partition.**
{% endhint %}

We'll create an empty folder to store your whole "reference materials", then empty sub-folders in it to store different types of files by category.

_It is always easier to have multiple sub-folders that you can independently rename/move/copy/zip/etc..., rather than a bunch of "more or less" related files in a single folder !_

**I suggest an organization like this one** \(in this example i prepared sub-folders for 2 GMs, just in case\) :

![](../.gitbook/assets/image%20%284%29.png)

In the **T:** drive, I created **`fg-ovpn`**, with :

* **`docs`** for docs, articles, infos you may also download in case you need to look them up years later...
* ⚠ **`MY-FG-OVPN`** for **our own FG OpenVPN setup** ⚠ 
* **`new-PKI`** for a **blank PKI** installation, ready to be duplicated **to initialize a PKI from scratch**
* **`source-install`** for downloaded installation packages

We also have a **separate `Tests` folder for tests and messing around with temporary things**.

{% hint style="info" %}
**Choose a simple name for your setup**, and try to **use it to prefix/idenfity** all related things.

We'll see later on how it helps managing multiple setups.
{% endhint %}

In this sample setup we use **`MY-FG-OVPN`** to store **our own FG OpenVPN setup**, with these sub-dirs:

* **`my-fg-ovpn-clients`** will store the **complete set** of parameter/security files needed **for each VPN client** \(with **a sub-folder for each**\)
* **`my-fg-ovpn-s3`** will store the **source files we'll transfer to the AWS S3 bucket** for server auto-initialization
* **`my-fg-ovpn-server`** will store the **files you may want to copy from your OpenVPN server** when it's running
* **`MY-FG-OVPN-PKI`** will be the **"safe" storage for our entire PKI**, ready to use if we need to create additional certificates or revoke a certificate.

{% hint style="warning" %}
I suggest you **ONLY** use **capital** letters on your **local Windows** machine, to be reminded of the **folders you need to keep safe**, and use **lowercase for all folder and file names you will transfer to AWS**.

**Your server** will be Linux-based, which implies **case-sensitive** !
{% endhint %}

**CAPITAL LETTERS = YOUR MOST PRECIOUS LOCAL FOLDERS**.

{% embed url="https://youtu.be/Iz-8CSa9xj8" %}



