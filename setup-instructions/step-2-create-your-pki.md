# Step 2 : Create your PKI

Properly creating the PKI is the **fundamental anchor of your setup's security !**

## Duplicate from the `new-PKI`

As first step we'll duplicate the whole contents of our **`new-PKI`** folder into **our own PKI** sub-folder named **`MY-FG-OVPN-PKI`** : select **all files**, then **Copy**, go to the **target folder** then **Paste**

![Copy all from the source](../.gitbook/assets/image%20%287%29.png)

![Paste to the target](../.gitbook/assets/image%20%2816%29.png)

{% hint style="info" %}
We'll now more or less follow the instructions you can read in the README.txt file, except that :

* **I'll bore you considerably more than the short orignal README \(**_**Hi tech guys !**_**\)**
* **we'll adapt to the "autonomous" PKI location**
* **we'll add some safeguard to the most dangerous command script**
{% endhint %}

## MY-FG-OVPN-PKI-KEYS sub-folder

Let's create a sub-folder into our own PKI folder, to store this particular PKI's "database" of certificates and keys, that we'll call **`MY-FG-OVPN-PKI-KEYS`** :

![](../.gitbook/assets/image%20%284%29.png)



## Use init-config and edit vars.bat





























{% hint style="warning" %}
📢 **YOU in the background ! WAKE UP ! This is important !** 📢 
{% endhint %}

{% hint style="danger" %}
From now on, we'll use a **single command line prompt** window to type all the commands.

We'll certainly **NOT double-click command files !!!**
{% endhint %}

_**Last reference to "**_[_**Check that you know**_](step-0-preparation.md#check-that-you-know)_**" in Step 0 !**_



We'll position ourselves in the right folder : In my demo this is T:\fg-ovpn\MY-FG\_OVPN :

{% code-tabs %}
{% code-tabs-item title="Reposition to our own PKI" %}
```text
T:
CD \fg-ovpn\MY-FG_OVPN
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Then once we have arrived **the very first command** we'll type is :

{% code-tabs %}
{% code-tabs-item title="VERY FIRST COMMAND" %}
```text
.\vars.bat
```
{% endcode-tabs-item %}
{% endcode-tabs %}

{% hint style="info" %}
This will setup the "**environment variables**" that you specified earlier, and they will remain **available for other commands until you close that specific command line prompt window**.
{% endhint %}

{% hint style="danger" %}
**If you** happen to close the command line prompt window and have to **open a new one**, **be sure to re-position to the right folder and re-type this command FIRST as above !**
{% endhint %}



