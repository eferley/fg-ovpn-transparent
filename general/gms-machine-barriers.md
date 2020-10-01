# GM's machine barriers

{% hint style="info" %}
**Applies to** : FGC \(+ FGU in LAN mode\)

Default ports : FG "Classic" = TCP 1802, FG "Unity" = UDP 1802
{% endhint %}

These first 3 points relate to the configuration of the GM's machine/OS.

{% hint style="success" %}
_**Breaking these barriers** should be enough to **successfully host a game for players** located **in your own home network**, even if you are in a no-go situation for remote player access._
{% endhint %}

{% hint style="warning" %}
If you have a third-party protection software installed, it _**may**_ "take over" some settings from Windows 10 itself : you may then find them in the third-party software settings.
{% endhint %}

## Barrier 1 😡 : Windows 10 "public/private" network setting

Windows 10 has a setting to classify each network it is connected to as :

* either "pubic" \(read : "not safe"\)
* or "private" \(read : "safe"\)

Windows 10 will prevent incoming connections from "public" networks, including Fantasy Grounds, so be sure to 👍 **tag your current W10 network connection as "private"**, **or else...** 👎 

[How to change your Windows network connections between "Public" and "Private"](https://www.digitalcitizen.life/how-set-your-networks-location-private-or-public-windows-10)

_**TODO : W10 screenshots needed here**_

## Barrier 2 👿 : OS-managed firewall

Be sure to 👍 **allow incoming communications to your TCP 1802, or better yet** 👍\*\*\*\*👍 **to your Fantasy Grounds executable program in the operating system firewall**, **or else...** 👎 

_Here is an example of Windows 7 firewall rules authorizing **any kind of incoming connection to the Fantasy Grounds program**, with TCP+UDP protocols and the "ALL" value for networks, adresses and ports parameters \(W7 Firewall "advanced" view\) :_

![W7 &quot;All Incoming&quot; rules example for F.G. ](../.gitbook/assets/image%20%28153%29.png)

_**TODO : W10 screenshots needed here**_

{% hint style="info" %}
I would advise **authorizing the Fantasy Grounds program \("Classic" and/or "Unity"\) for ALL incoming connections**, instead of specifically TCP 1802 \(FGC\) or UDP 1802 \(FGU\).

That will enable using alternate ports for use cases like "multiple FGC+FGU games with 1 VPN server" - see [Lifting the "1 GM at-a-time" restriction](../simultaneous-games-fgc-1-vpn-server/untitled.md) and following sections.
{% endhint %}

## Barrier 3 👹 : Third-party protection software

If you are using **any kind of  third-party anti- malware/virus/threat protection software**, chances are that you also need to 👍 **enable incoming communications for FG in these**, **or else...** 👎 

_**TODO : Other screenshots needed here ??**_



