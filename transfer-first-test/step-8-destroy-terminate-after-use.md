# Step 8 : Destroy/"Terminate" after use

_**So, we're happy !**_ \(hopefully ?\)

And we have **finished our tests or game session**...

And we have **disconnected our VPN client connection**...

So, what do we do now ?...

## We break our new toy !

Ok, **but why ???** 

Well, because :

* **when we are not hosting** a game, **we don't need it**
* if we are **out of the Free Tier** bounds, we want to **keep our costs as low as possible**
* we'll be sure **we can re-create our environment in a matter of minutes** in the unlikely event of catastrophic failure
* it is a good starting point to explore, later on, the [AWS Spot offer](https://aws.amazon.com/ec2/spot) for the long term.

So we go to the **EC2 Console** -&gt; **Instances** and we **"Terminate"** our instance :

![Terminating = erradication !](../.gitbook/assets/image%20%2867%29.png)

A quick confirmation dialog, and then our instance goes...

![](../.gitbook/assets/image%20%2823%29.png)

And after about 1 minute...

![](../.gitbook/assets/image%20%2824%29.png)

From that point, it is as if it had never existed...and after some time \(1H or so ?\) it will have _completely disappeared from your EC2 console_.



