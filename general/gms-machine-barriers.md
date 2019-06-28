# GM's machine barriers

These first 3 points relate to the configuration of the GM's machine/OS.

{% hint style="success" %}
_**Breaking these barriers** should be enough to **successfully host a game for players** located **in your own home network**, even if you are in a no-go situation for remote player access._
{% endhint %}

## Barrier 1 😡 : Windows 10 "public/private" network setting

Windows 10 has a setting to classify each network it is connected to as :

* either "pubic" \(read : "not safe"\)
* or "private" \(read : "safe"\)

Windows 10 will prevent incoming connections from "public" networks, including Fantasy Grounds, so be sure to 👍 **tag your current W10 network connection as "private"**, **or else...** 👎 

_**TODO : W10 screenshots needed here**_

## Barrier 2 👿 : OS-managed firewall

Be sure to 👍 **allow incoming communications to your TCP 1802, or better yet** 👍👍 **to your Fantasy Grounds executable program in the operating system firewall**, **or else...** 👎 

_Here is an example of Windows 7 firewall rules authorizing **any kind of incoming connection to the Fantasy Grounds program**, with TCP+UDP protocols and the "ALL" value for networks, adresses and ports parameters \(W7 Firewall "advanced" view\) :_

![W7 &quot;All Incoming&quot; rules example for F.G. ](../.gitbook/assets/image%20%287%29.png)

_**TODO : Other screenshots needed here**_

## Barrier 3 👹 : Third-party protection software

If you are using **any kind of  third-party anti- malware/virus/threat protection software**, chances are that you also need to 👍 **enable incoming communications for FG in these**, **or else...** 👎 

_**TODO : Other screenshots needed here ?**_

