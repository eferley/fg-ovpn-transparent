# Step 2 : Create your own PKI

{% hint style="info" %}
Properly creating the PKI is the **fundamental anchor of your setup's security !**
{% endhint %}

{% hint style="danger" %}
**A number of files we'll create in this section will have to be named starting with the "configuration name" we chose in Step 0 :** _**my-fg-ovpn**_ **in this demo.**
{% endhint %}

## Duplicate from the `new-PKI`

As first step we'll duplicate the whole contents of our **`new-PKI`** folder into **our own PKI** sub-folder named **`MY-FG-OVPN-PKI`** : select **all files**, then **Copy**, go to the **target folder** then **Paste**

![Copy all from the source](../.gitbook/assets/image%20%2873%29.png)

![Paste to the target](../.gitbook/assets/image%20%28141%29.png)

{% hint style="info" %}
We'll now more or less follow the instructions you can read in the README.txt file, except that :

* **I'll bore you considerably more than the short orignal README** \(_Sorry, tech guys !_ ![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) \)
* **we'll adapt to the "autonomous" PKI location**
* **we'll add** _**some safeguards**_ **to the most dangerous command scripts**
{% endhint %}

## Create the keys sub-folder

Let's create a sub-folder into our own PKI folder, to store this particular PKI's "database" of certificates and keys, that we'll call **`MY-FG-OVPN-PKI-KEYS`** :

![](../.gitbook/assets/image%20%2844%29.png)



## Run init-config.bat, then rename it to avoid re-using it

You can double-click **init-config.bat** in **`MY-FG-OVPN-PKI`** to run this command, and that will initialize the **new vars.bat command script file** from a sample file.

{% hint style="danger" %}
The init-config.bat command **should not be re-used later on, as it would ERASE the customizations** we are about to do in the vars.bat file.

So we'll **rename init-config.bat** to **DONE-DANGER-init-config.bat and FORGET about it.**
{% endhint %}

![Renaming a &quot;once only&quot; command](../.gitbook/assets/image%20%2821%29.png)



## Edit vars.bat

We now have to **edit the vars.bat file for it to meet our requirements** :

* we **comment out \(or delete\)** the lines playing with the **PATH** to ensure the commands use the version of executable binary programs that we have in our PKI folder, and none other \(lines 7 and 10 below; only line 7 needs editing since line 10 is already commented out with "**`rem`** "\)
* we set **HOME** to the full path of the root folder for our PKI; everything the Easy-RSA commands will do later on will happen from that location : this is the **`T:\fg-ovpn\MY-FG_OVPN\MY_FG_OVPN_PKI`** folder \(line 12 below\)
* we set **KEY\_DIR** to the name of the sub-folder where we want our certificates and keys to be stored : this is the **`MY-FG-OVPN-PKI-KEYS`** we created earlier
* we give some default values for our future certificates in the list of **set KEY\_xxxx** commands at the end of the file.

{% hint style="warning" %}
To avoid accidentally keeping a \(wrong\) default value for **2 important fields** of our certificates, we'll put a **prompt for action as their default value** : **KEY\_CN** and **KEY\_NAME**
{% endhint %}

* we can **keep or delete all the commented lines** \(= lines starting with "**`rem`** "\)



So we edit vars.bat in a text editor \(even _Right click + Edit_ with the very limited Windows Notepad is sufficient for good old **`.bat`** windows-only command files\) as follows.

Supposing you start **from the original content below** \(sample as of OpenVPN v2.4.7\),

{% code-tabs %}
{% code-tabs-item title="Original vars.bat BEFORE we edit it" %}
```text
@echo off
rem Edit this variable to point to
rem the openssl.cnf file included
rem with easy-rsa.

rem Automatically set PATH to openssl.exe
FOR /F "tokens=2*" %%a IN ('REG QUERY "HKEY_LOCAL_MACHINE\SOFTWARE\OpenVPN"') DO set "PATH=%PATH%;%%b\bin"

rem Alternatively define the PATH to openssl.exe manually
rem set "PATH=%PATH%;C:\Program Files\OpenVPN\bin"

set HOME=%ProgramFiles%\OpenVPN\easy-rsa
set KEY_CONFIG=openssl-1.0.0.cnf

rem Edit this variable to point to
rem your soon-to-be-created key
rem directory.
rem
rem WARNING: clean-all will do
rem a rm -rf on this directory
rem so make sure you define
rem it correctly!
set KEY_DIR=keys

rem Increase this if you
rem are paranoid.  This will slow
rem down TLS negotiation performance
rem as well as the one-time DH parms
rem generation process.
set DH_KEY_SIZE=2048

rem Private key size
set KEY_SIZE=4096

rem These are the default values for fields
rem which will be placed in the certificate.
rem Change these to reflect your site.
rem Don't leave any of these parms blank.

set KEY_COUNTRY=US
set KEY_PROVINCE=CA
set KEY_CITY=SanFrancisco
set KEY_ORG=OpenVPN
set KEY_EMAIL=mail@host.domain
set KEY_CN=changeme
set KEY_NAME=changeme
set KEY_OU=changeme
set PKCS11_MODULE_PATH=changeme
set PKCS11_PIN=1234

```
{% endcode-tabs-item %}
{% endcode-tabs %}

...then you should **end up with this kind of minimal final result** :

{% hint style="warning" %}
**ALL the other lines** you may want to keep for reference in the **vars.bat** file should be **commented out** with **"rem " as seen above.**

**Here, i chose to delete them.**
{% endhint %}

{% hint style="info" %}
_\*\*\*\*_![](../.gitbook/assets/zeferby_dino_64%20%281%29.png)  😛 _**Yes of course ! The HOME full path to your own PKI may be different than this demo, as well as your**_ 🇫🇷 _**KEY\_COUNTRY etc... default values at the end of the file**_
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="Valid minimalist content for vars.bat" %}
```text
@echo off

set HOME=T:\fg-ovpn\MY-FG-OVPN\MY-FG-OVPN-PKI
set KEY_CONFIG=openssl-1.0.0.cnf
set KEY_DIR=MY-FG-OVPN-PKI-KEYS
set DH_KEY_SIZE=2048
set KEY_SIZE=4096

set KEY_COUNTRY=FR
set KEY_PROVINCE=FR
set KEY_CITY=MyCity
set KEY_ORG=MyOrg
set KEY_EMAIL=no-mail@no-domain.com
set KEY_CN=CHANGE-ME-MANDATORILY
set KEY_NAME=CHANGE-ME-MANDATORILY
set KEY_OU=MyOrgUnit
set PKCS11_MODULE_PATH=unused
set PKCS11_PIN=1234

```
{% endcode-tabs-item %}
{% endcode-tabs %}



## Edit and rename ⚠ clean-all.bat ⚠ to avoid killing our PKI

{% hint style="danger" %}
The **clean-all.bat command script ERASES the whole PKI "database"** including **all certificates and keys**. It **must be used ONLY ONCE to initialize the database and NEVER AGAIN !**
{% endhint %}

So we'll first edit it to **add these 2 lines between original lines 1 and 2** :

```text
choice /C YN /T 10 /D n /M "This will ERASE THE PKI AT %HOME% ...Still want to do this ??"
IF ERRORLEVEL 2 GOTO End
```

And we'll **add this last line at the end of the file** :

```text
:End
```

The **resulting clean-all.bat** file should be :

{% code-tabs %}
{% code-tabs-item title="Edited clean-all.bat" %}
```text
@echo off
choice /C YN /T 10 /D n /M "This will ERASE THE PKI AT %HOME% ...Still want to do this ??"
IF ERRORLEVEL 2 GOTO End
rem move to the HOME directory specified in VARS script
cd %HOME%
rem set a temporary KEY_DIR variable
set d=%KEY_DIR%
rem delete the KEY_DIR and any subdirs quietly
rmdir /s /q %d%
rem make a new KEY_DIR
mkdir %d%
rem copy in a fesh index file so we begin with an empty database
copy index.txt.start %d%\index.txt
rem copy in a fresh serial file so we begin generating keys at index 01
copy serial.start %d%\serial.
:End

```
{% endcode-tabs-item %}
{% endcode-tabs %}

This way, when we run this "killer" command script file, we'll have a **warning and Y/N choice**, with the "No I don't want to kill my own innocent PKI" answer selected by default after 10 seconds...

...and just to be safe, we'll **rename `clean-all.bat` to `NEVER-AGAIN-DANGER-clean-all.bat`** !

![Renaming THE &quot;killer&quot; command](../.gitbook/assets/image%20%2843%29.png)

{% hint style="info" %}
\_\_![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) _What ? Sorry ? Are you kidding me ? **Did I ever fall into that trap**, instantly killing a PKI that had been used for 3 years with 50+ certificates ? With an out of date backup ?..._

_**OF COURSE NOT !!!**_

_...Well..._~~_maybe once but i don't really remember, you see, so :_~~ _**let's move on !**_
{% endhint %}



## Going to command line mode

{% hint style="warning" %}
\*\*\*\*![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) 📢 **YOU in the background ! WAKE UP ! This is important !** 📢 
{% endhint %}

{% hint style="danger" %}
From now on, we'll use a **single command line prompt** window to type all the commands.

We'll **ABSOLUTELY NOT double-click command files !!!**
{% endhint %}

_**=&gt; Last reference to "**_[_**Check that you know/learn**_](step-0-preparation.md#check-that-you-know)_**" in Step 0 !**_

So we open a command line prompt window and we position ourselves in the right **folder for our PKI**. In my demo this is :**`T:\fg-ovpn\MY-FG_OVPN\MY_FG_OVPN_PKI`** 

{% code-tabs %}
{% code-tabs-item title="Reposition to our own PKI" %}
```text
T:
CD \fg-ovpn\MY-FG_OVPN\MY-FG-OVPN-PKI
```
{% endcode-tabs-item %}
{% endcode-tabs %}

A quick check to verify this is the right place :

{% code-tabs %}
{% code-tabs-item title="Verify the contents of the folder" %}
```text
dir
```
{% endcode-tabs-item %}
{% endcode-tabs %}

![Looks like we&apos;re in the right place !](../.gitbook/assets/image%20%28132%29.png)

Then once we know we have arrived at the right place, **the VERY FIRST COMMAND** we'll type is :

{% code-tabs %}
{% code-tabs-item title="VERY FIRST COMMAND" %}
```text
.\vars.bat
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
This will setup the "**environment variables**" that you specified earlier, and these will remain **available for other commands until you close that specific command line prompt window**.
{% endhint %}

{% hint style="danger" %}
**If you need to open a new command prompt window**, **remember to re-position to the right folder and use ".\vars.bat" command FIRST !**

**Remember that, when you come back days or months later, to add a new client certificate or revoke an existing one.**

**Try to build up your keyboard "muscle memory" with the following tip :**
{% endhint %}

{% hint style="success" %}
The **vars.bat command script can be run multiple times** without any issue.

**Actually, we'll re-run it before every other command in this example.**
{% endhint %}



## A clean starting point - ONCE AND FOR ALL

Let's use **the "killer" command script**, give **the Y answer THIS ONE TIME**, and then **let's forget about it**...

```text
.\vars.bat
.\NEVER-AGAIN-DANGER-clean-all.bat
```

**This DELETES any pre-existing data in your PKI keys folder and initializes the PKI "database" :**

![THE ONE TIME : &quot;Y&quot; answer](../.gitbook/assets/image%20%2869%29.png)

![A brand new PKI, initialized/erased by the killer command script](../.gitbook/assets/image%20%28173%29.png)





## Build the CA Certificate+Key

Easy !

```text
.\vars.bat
.\build-ca.bat
```

{% hint style="info" %}
**Answer the prompts**, just hitting "Enter/Return" to keep default values **EXCEPT FOR Common Name and Name. Use the SAME simple name without spaces for the 2 fields.**

Let's call our CA : **my-fg-ovpn-ca**, _keeping our good prefixing habit_.
{% endhint %}

![Same simple name on the 2 name fields](../.gitbook/assets/image%20%2879%29.png)

This creates the PKI Certificate Authority's own self-signed certificate \(**ca.crt**\) and private key \(**ca.key**\) in the **`MY-FG-OVPN-PKI-KEYS`** sub-folder.

![](../.gitbook/assets/image%20%28164%29.png)

Since we are here within our own PKI, we only have one CA so these simple filenames are fine. But :

{% hint style="danger" %}
**Our server will need to have the CA certificate named as "configuration name"-ca.crt**

**\("configuration name" =** _**my-fg-ovpn**_ **in this demo\)**
{% endhint %}

{% hint style="warning" %}
**=&gt;** We'll **copy+paste+rename these 2 files to have proper names :**

* **ca.crt** copied to **my-fg-ovpn-ca.crt**
* **ca.key** copied to **my-fg-ovpn-ca.key**
{% endhint %}

![Uniquely named copy of ca.crt + ca.key](../.gitbook/assets/image%20%2875%29.png)

{% hint style="danger" %}
**Do NOT rename the original files directly, but keep properly named copies !**
{% endhint %}



## Build the DH file

Easy, but may take a long time depending on your computer !

```text
.\vars.bat
.\build-dh.bat
```

![Beginning of the DH file creation](../.gitbook/assets/image%20%28110%29.png)

...time passes...finally :

![Finished !](../.gitbook/assets/image%20%2816%29.png)

This has generated the [**Diffie-Hellman-Merkel key exchange**](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange) parameter file that will be used to initiate encryption sessions.

![](../.gitbook/assets/image%20%28160%29.png)

{% hint style="danger" %}
**Our server will need to have our DH file named as "configuration name"-dh2048.pem**

**\("configuration name" =** _**my-fg-ovpn**_ **in this demo\)**
{% endhint %}

{% hint style="warning" %}
=&gt; We'll also keep a **copy of dh2048.pem** named **my-fg-ovpn-dh2048.pem**
{% endhint %}

![Uniquely named copy of dh2048.pem](../.gitbook/assets/image%20%2825%29.png)



## Build OpenVPN Server Certificate+Key

Easy again !

{% hint style="danger" %}
**Our server will need to have his certificate file named "configuration name"-srv.crt \(and similarly for the key and certificate request\)**

**\("configuration name" =** _**my-fg-ovpn**_ **in this demo\)**
{% endhint %}

{% hint style="info" %}
**Answer the prompts**, just hitting "Enter/Return" to keep default values **EXCEPT FOR Common Name and Name. Use the SAME simple name without spaces for the 2 fields.**

So let's call our **OpenVPN server** _****_: **my-fg-ovpn-srv**, to _satisfy the above requirement_.
{% endhint %}

```text
.\vars.bat
.\build-key-server.bat my-fg-ovpn-srv
```

Since our **server should boot up in unattended mode** \(without anyone to enter a password\) :

{% hint style="danger" %}
**Do NOT enter a challenge password**
{% endhint %}

{% hint style="info" %}
**\(don't mind the error about "index.txt.attr", the file will be created by the process\)**

**Answer y for Yes to the last 2 questions :**

* **sign the certificate ?** ==&gt; YES !
* **commit ?** ==&gt; YES !
{% endhint %}

![Create, sign, commit : OpenVPN server](../.gitbook/assets/image%20%2871%29.png)

This creates 3 files in your keys folder, with the name you typed on the command line :

* **my-fg-ovpn-srv.crt** : the server public certificate
* **my-fg-ovpn-srv.key** : the server private key
* **my-fg-ovpn-srv.csr** : the "certificate signing request" used during the process

![](../.gitbook/assets/image%20%28179%29.png)

{% hint style="info" %}
These files should **already be properly named**, so we'll keep them as they are:

**"configuration-name"-srv.crt, .csr and .key =&gt;** _**my-fg-ovpn-srv.crt, .csr and .key**_
{% endhint %}



## Build 2 clients \(GMs\) Certificate+Key

Being used to D&D, i already called my clients subfolders my-fg-ovpn-dm01/02, so let's stick to that.

```text
.\vars.bat
.\build-key.bat my-fg-ovpn-dm01
```

![Create, sign, commit : my-fg-ovpn-dm01 for one GM](../.gitbook/assets/image%20%28172%29.png)

**Do the same thing with my-fg-ovpn-dm02...**

![Create, sign, commit : my-fg-ovpn-dm02 for a second GM](../.gitbook/assets/image%20%28137%29.png)

You should now have :

![](../.gitbook/assets/image%20%28119%29.png)



## Revoke \(invalidate\) DM02's certificate

Our next step will be to **initialize a Certificate Revocation List \(CRL\) for our OpenVPN server** to systematically check before authorizing a connection.

{% hint style="info" %}
The **CRL** will be your way of ensuring that older **certificates**, that you may have distributed in the past and **revoke/invalidate at some point, are really blocked**.
{% endhint %}



But to create that CRL, **we need one client certificate to be revoked, so "DM02" is our victim !** 🔫 🗡 

```text
.\vars.bat
.\revoke-full.bat my-fg-ovpn-dm02
```

![Revoke poor DM02 !](../.gitbook/assets/image%20%28162%29.png)

You can see on the above screenshot :

* _**why we kept the original ca.crt and ca.key files, visibly used there...**_ 👀 
* the Easy-RAS utility checking that a session negociation initiated on behalf of **my-fg-ovpn-dm02** **actually fails because the certificate is revoked, which is what we want !** 👍 

{% hint style="info" %}
**This process** does not delete files : it **marks them as revoked \(= invalid\) in the PKI "database", and includes in the immediately \(re-\)generated CRL \(Certificate Revocation List\).**
{% endhint %}



**So : R.I.P. my-fg-ovpn-dm02...** ⚰ ****💀 ****☠ ****

_**After all, DM01 was here first, and he's not a sharer...**_

{% embed url="https://www.youtube.com/watch?v=sqcLjcSloXs&feature=youtu.be&t=5" %}



## Your initial Certification Revocation List

As soon as you have revoked the \(_probably innocent_\) my-fg-ovpn-dm02 during the previous step, your PKI \(re-\)generated the **CRL**=_**Certificate Revocation List**_ under the basic name of **crl.pem :**

![](../.gitbook/assets/image%20%2894%29.png)

{% hint style="danger" %}
**Our server will need to have the CRL named as "configuration name"-crl.pem**

**\("configuration name" =** _**my-fg-ovpn**_ **in this demo\)**
{% endhint %}

{% hint style="warning" %}
**=&gt;** We'll **copy+paste+rename** the CRL from **crl.pem** to **my-fg-ovpn-crl.pem :**
{% endhint %}

![Uniquely named CRL](../.gitbook/assets/image%20%286%29.png)



## **PKI ready !**

{% hint style="success" %}
**Congratulations !**

**Your own PKI is now complete, with clear enough filenames** so that you know who is what, in case they happen to be used on OpenVPN servers/clients managing/using multiple independent VPNs.
{% endhint %}

We'll see in the following sections how these files are put to use, first on the OpenVPN client, then on the server.



## **One more key for OpenVPN** tls-auth

While we are in our PKI root folder \(**`T:\fg-ovpn\MY-FG_OVPN\MY_FG_OVPN_PKI`**\) at the command prompt, let's have OpenVPN generate a static secret key.

That key will be used by OpenVPN "**TLS Authentication**" feature to protect initial key negociations when OpenVPN clients connect to the server.  You can read about **tls-auth** [here](https://openvpn.net/community-resources/hardening-openvpn-security/).

{% hint style="danger" %}
**Our server will need to have the TLS-Auth file named as "configuration name"-ta.key**

**\("configuration name" =** _**my-fg-ovpn**_ **in this demo\)**
{% endhint %}

We'll create this key with the right filename, in our keys sub-folder :

```text
openvpn.exe --genkey --secret MY-FG-OVPN-PKI-KEYS\my-fg-ovpn-ta.key
```

![](../.gitbook/assets/image%20%28117%29.png)

Nothing fancy, it just creates the last key file we were missing in our security arsenal. Here is the complete list of files in our keys sub-folder :

![](../.gitbook/assets/image%20%2838%29.png)



