# Step 6 : Launch time !

## ![](../.gitbook/assets/zeferby_dino_64%20%281%29.png) **Ready ? Steady ? Go !**

Using the EC2 Console we have this big blue "**Launch Instance**" _button+arrow_ waiting for us :

* on the "_**Dashboard**_"

![](../.gitbook/assets/image%20%28161%29.png)

* on the "_**Instances**_" console :

![](../.gitbook/assets/image%20%2812%29.png)



Let's **use our Launch Template** by using the **down-pointing arrow** on the right of the button here :

![](../.gitbook/assets/image%20%28104%29.png)

This gives you a form where you can select your **Launch Template** and its **version** :

![](../.gitbook/assets/image%20%28125%29.png)



![](../.gitbook/assets/image%20%28126%29.png)

You will then see all the choices that were recorded in the template, that you can override at launch time if you whish to :

![](../.gitbook/assets/image%20%28175%29.png)

Just check that your _**IAM instance profile**_ and _**User data**_ are populated by expanding the _**Advanced details**_ section, and click **Launch instance from template** at the bottom !

{% hint style="success" %}
**Your "instance" = virtual server has just been created and is starting !**
{% endhint %}

## Your first "Instance"

After closing the following "success" window, **go back to the "Instances" console** and select your "Instance" = your OpenVPN virtual server \(_by the way, don't bother trying to connect to the one below, it has long been terminated..._\) :

![](../.gitbook/assets/image%20%28139%29.png)

There's a lot of infos on this screen :

* **Instance ID** \(blue highlight\) is the unique identifier of this virtual machine \(= "Instance"\)
* **Instance State** \(orange highlight\) should change if you refresh after a few seconds, going from  _**pending**_ to _**running**_
* **Status Checks** \(green highlight\) should stay at _**Initializing**_ until the very last test after several minutes, when it will become✅ _**2/2 checks passed**_, \(the server will actually be ready sooner than that\)
* **Private IPs** \(cyan highlight\) should only show _1 private IP, dynamically assigned_ from your VPC
* **IPv4 Public IP** and its automatically assigned **Public DNS \(IPv4\)** \(red highlight\) is also _dynamically assigned by AWS_ for your server \(because we want one !\)
* **Source/dest. check** \(pink highlight\) should change during auto-configuration from _**True**_ to _**False**_

### The IPv4 Public IP

This is actually **the graal you were seeking** 🙌 : a perfectly routable public IP where, **during the lifetime of this "Instance" running your OpenVPN server** :

* **you will connect with OpenVPN \(as a client\)**
* **your players will connect with Fantasy Grounds** when you're ready to host a game for them

{% hint style="info" %}
So : Just take 1 second to hover your mouse over the **IPv4 Public IP** and use the small button appearing to the right, to "_**copy to clipboard**_" !
{% endhint %}

![](../.gitbook/assets/image%20%2864%29.png)

### Your own firewall rules = Security Groups

In the left column of the _**Description**_ tab, you find the _**Security groups**_ assigned to this Instance \(you can have multiple SGs assigned\), and you can easily check the **Incoming and Outgoing authorizations**, and from what Security Group they come from :

![View inbound rules](../.gitbook/assets/image%20%28146%29.png)

![View Outbound rules](../.gitbook/assets/image%20%28159%29.png)



## Ready GM 1 !

When you see the **Status Checks** field at the top showing✅ _**2/2 checks passed**_, it will mean that...

{% hint style="success" %}
**Your server is ready and patiently waiting for you to connect !**
{% endhint %}



