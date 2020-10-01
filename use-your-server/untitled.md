# Regular Usage Pattern

## Running 24x7 or not

Depending on your situation vs. the AWS Free Tier, you may choose to run your server :

* **either 24x7** \(especially during the first year, while your total AWS monthly cost is $0\)
* **or on-demand, only when needed** \(if you want to drastically optimize costs later on\)

The drawback of starting/stopping or launching/terminating your server on-demand is that the **public IP address will change at every startup**, and you'll have to :

* do "**Edit Config**" with the **OpenVPN GUI** to enter the new public IP
* and **communicate the new public IP to your players** so they know where to connect

{% hint style="warning" %}
Remember that FG _**Alias**_ and _**Test Connection**_ are not functionnal with our setup.
{% endhint %}



This would be the **regular usage pattern** when running the server **only when needed** :

1.  **Launch** new instance from _**Launch Template**_ and note the _**Public IP**_ of the new server
2. **OpenVPN GUI** :
   1. **Edit Config** \(to enter the new server Public IP\)
   2. **Connect**
3. **Give** the new server **Public IP to your players** 
4. **==&gt; Fantasy Grounds game session &lt;==** 
5. **Disconnect** OpenVPN
6. **Terminate** the instance



As you can guess, the management of the changing public IP can become a pain, especially if the server is shared with other GMs \(see following sections\) and has to be started multiple times a week...

This can lead you to trade some _**cost optimization**_ for _**ease of use**_ and run you server 24x7, even when you are out of the Free Tier bounds - be sure to **check the cost minimizing techniques** I mentioned earlier : [Amazon Web Services - Minimizing AWS costs when you step outside the Free Tier](../proposed-solution/amazon-web-services.md#minimizing-aws-costs-when-you-step-outside-the-free-tier)



