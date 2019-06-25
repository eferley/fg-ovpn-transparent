# Some NO-GO Situations

Unfortunately there are **situations where you can't break some of barriers 4 to 6 "as is".** Some common ones are listed below :

{% hint style="info" %}
**Most of them come down to the inability to have the proper port forwarding rule applied on ALL NAT/PAT routers between you and the Internet.**
{% endhint %}

### No unique IPv4 address per subscriber \("IPv6 Internet connection"\)

Some ISPs and countries rely on a **purely IPv6 infrastructure, down to the subscriber endpoint**. In that case **they already use a form of NAT/PAT in the ISP network** to **share a public IPv4 address between multiple subscribers**.

{% hint style="danger" %}
In this situation, there is **no way to have a port forwarding rule applied**, for one specific subscriber, within the ISP network infrastructure itself.

**That is a NO-GO for FG "Classic" game hosting**.
{% endhint %}

### No administrative acces to an "Internet router\(s\)"

In case you **cannot acces the administration/setup** facility of one or more of the NAT/PAT routers between you and the Internet :

{% hint style="danger" %}
You will then be **unable to apply a port forwarding rule in that router**, so...bad luck !
{% endhint %}

### Public/shared Internet access

Sometimes if you are a "**GM on the road**", you'll need to use a kind of **public/shared access** to the Internet \(hotel room, internet cafe, etc...\), which means that here also you will **not have admin access to the NAT/PAT router**, and once again :

{% hint style="danger" %}
You will be **unable to apply a port forwarding rule in that router**, so...bad luck !
{% endhint %}

### Upstream "closed" firewall

You may not always be able to "open ports" or add "exceptions" on the firewalls between your GM FG program and the Internet, even if proper port forwarding rules have been setup. This is generally true when you are in a **protected/corporate network environment**.

{% hint style="danger" %}
If **a firewall you don't control blocks incoming connections to TCP 1802**, it's also a **NO-GO for FG game hosting**.
{% endhint %}

