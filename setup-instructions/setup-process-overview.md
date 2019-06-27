# Setup process overview

The overall process follows here.

We'll detail a step-by-step setup in the following pages, and add some more technical blocks of info that non tech-savvy readers can ignore 😝 :

> **Tech Blurb**
>
> blah blah



**\(local\)** steps are things done entirely in your own local machine.



**Step 0** : **Preparation** : Have an [account available on AWS](../proposed-solution/amazon-web-services.md#how-to-create-an-aws-account) and prepare a "safe" place on your own machine

**Step 1** : \(local\) [Download and install](step-1-install-openvpn-+-easy-rsa-2.md) **OpenVPN \(+ Easy-RSA\)** on your Windows machine \(+optional Tools if you want\)

**Step 2** : \(local\) [Create a Pubic Key Infrastructure](step-2-create-your-pki.md) with its Certificate Authority and the public certificates + private keys for the OpenVPN server and at least one VPN client \(= the GM\)

**Step 3** : \(local\) [Setup your OpenVPN client](step-3-prepare-your-openvpn-connection.md) with the required certificates and keys into the config directory

**Step 4** : \(AWS\) [Configure your AWS environment](step-4-configure-your-aws-environment.md) : VPC \(Virtual Private Cloud\), networks, security settings, create an S3 bucket and an IAM role

**Step 5** : \(local+AWS\) [Check your settings, Finalize all](step-5-check-finalize-transfer-files-to-s3.md) configuration elements and transfer files to AWS S3

**Step 6** : \(AWS\) [Launch your FG-OpenVPN Linux server](step-6-launch-time.md) in your AWS Virtual Private Cloud

Connect your VPN and enjoy...

