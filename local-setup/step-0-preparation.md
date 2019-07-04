# Step 0 : Preparation

Let's begin with : 

### **Create/have an** [**account available on AWS**](../proposed-solution/amazon-web-services.md#how-to-create-an-aws-account)\*\*\*\*

...as explained there... 👆 

### Check that you know/learn :

* [how to open a command line](https://www.digitalcitizen.life/7-ways-launch-command-prompt-windows-7-windows-8) with your version of Windows
* [how to show file extensions](https://www.howtohaven.com/system/show-file-extensions-in-windows-explorer.shtml) in Windows Explorer

**It may also be a good idea to know/learn** [how to show hidden/system files](https://helpx.adobe.com/x-productkb/global/show-hidden-files-folders-extensions.html) in Windows Explorer as well...

_**...and remember you'll end up needing a**_ [_**proper text editor**_](../proposed-solution/tools.md#your-text-editor-of-choice) _**at some point !**_

### **Prepare a "safe" place on your own machine.**

{% hint style="warning" %}
Create that on **another drive than your system C: drive if you can, mandatorily** in a place where you have **Read+Write permissions...and of course NOT in a "recovery" drive/partition.**
{% endhint %}

We'll create an empty folder to store your whole thing, then empty sub-folders in it to store different types of files by category.

_It is always easier to have multiple sub-folders that you can independently rename/move/copy/zip/etc..., rather than a messy bunch of "more or less" related files in a single folder !_

**I suggest an organization like this one** \(in this example i prepared sub-folders for 2 GMs = VPN "clients"\) :

![](../.gitbook/assets/image%20%2845%29.png)

In the **T:** drive, I created the **`fg-ovpn`** root folder, with :

* **`docs`** for docs & infos you may download in case you need to look them up years later...
* ⚠ **`MY-FG-OVPN`** for **our own FG OpenVPN setup** ⚠ 
* **`new-PKI`** for a **blank PKI** installation, ready to be duplicated **to initialize a PKI from scratch**
* **`source-install`** for downloaded installation packages

We also have a **separate `Tests` folder for tests and messing around with temporary things**.

{% hint style="info" %}
**Choose a simple "configuration name" for your setup**, and try to **use it to prefix/idenfity** all related things.
{% endhint %}

{% hint style="danger" %}
**This "configuration name" is VERY important : it will be used by configuration scripts; many of the files you will create later on MUST start with it as a prefix, so : better get used to it !**
{% endhint %}

![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) _**Read this again !**_ 👆 

{% hint style="warning" %}
In this sample setup we use "**my-fg-ovpn**" for our configuration name...
{% endhint %}

![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) _**Got it ?**_ 👆 

...and we create the **`MY-FG-OVPN`** folder \(in capitals to remember it must be kept safe\) to store **our own FG OpenVPN setup**, with these sub-dirs:

* **`my-fg-ovpn-clients`** will store the **complete set** of parameter/security files needed **for each VPN client** \(with **a sub-folder for each**\), ready to be zipped/copied/emailed
* **`my-fg-ovpn-s3`** will store the **source files we'll transfer to the AWS S3 bucket** for server auto-initialization
* **`my-fg-ovpn-server`** could store **files you may want to copy from your OpenVPN server** \(logs,...\)
* **`MY-FG-OVPN-PKI`** will be the **"safe" storage for our entire PKI**, ready to use at any time if we need to create additional certificates or revoke an existing certificate.

{% hint style="warning" %}
I suggest you **ONLY** use **CAPITAL** letters on your **local Windows** machine, to be reminded of the **folders you NEED TO KEEP SAFE**, and use **lowercase for all folder and file names you will transfer to AWS and the server**.

**Your OpenVPN server** will be Linux-based, which implies **case-sensitive** !
{% endhint %}

**CAPITAL LETTERS = YOUR MOST PRECIOUS LOCAL FOLDERS**.

{% embed url="https://youtu.be/Iz-8CSa9xj8" %}



