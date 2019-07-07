# Step 6 : Launch time !

## ![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) **Ready ? Steady ? Go !**

Using the EC2 Console we have this big blue "**Launch Instance**" _button+arrow_ waiting for us :

* on the "_**Dashboard**_"

![](../.gitbook/assets/image%20%28118%29.png)

* on the "_**Instances**_" console :

![](../.gitbook/assets/image%20%2810%29.png)



Let's **use our Launch Template** by using the **down-pointing arrow** on the right of the button here :

![](../.gitbook/assets/image%20%2873%29.png)

This gives you a form where you can select your **Launch Template** and its **version** :

![](../.gitbook/assets/image%20%2890%29.png)



![](../.gitbook/assets/image%20%2891%29.png)

You will then see all the choices that were recorded in the template, that you can override at launch time if you whish to :

![](../.gitbook/assets/image%20%28131%29.png)

Just check that your _**IAM instance profile**_ and _**User data**_ are populated by expanding the _**Advanced details**_ section, and click **Launch instance from template** at the bottom !

{% hint style="success" %}
**Your "instance" = virtual server has just been created and is starting !**
{% endhint %}

After closing the following "success" window, **go back to the "Instances" console** and select your "Instance" = your OpenVPN virtual server \(_by the way, don't bother trying to connect to the one below, it has long been terminated..._\) :

![First instance during startup](../.gitbook/assets/image%20%28114%29.png)

After a few seconds, you'll see the **Instance State** field at the top going from \(yellow dot\) _**pending**_ to \(green dot\) _**running**_, and its **public IPv4** and **automatically assigned public DNS name** \(highlighted in red above\); this is where, **during this session** :

* **you will connect with OpenVPN**
* **your players will connect with Fantasy Grounds** when you're ready to host a game for them

During the initialization process, when the server goes through its auto-configuration, you'll see the field \(highlighted in pink above\) "**Source/dest. check**" changing from _**True**_ to _**False**_ : this is **expected and required** !

After 1 or 2 minutes, you should see the **Status Checks** field at the top showing " ✅ _**2/2 checks passed**_".

{% hint style="success" %}
**Your server is ready, patiently waiting for you to connect !**
{% endhint %}

{% hint style="info" %}
Just take 1 second to hover your mouse over the **IPv4 Public IP** or **Public DNS \(IPv4\)** and use the small button on the right to "_**copy to clipboard**_".
{% endhint %}


