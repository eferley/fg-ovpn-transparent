# Setup process overview

The overall process follows here.

We'll detail a step-by-step setup in the following pages, starting at Step 2.

**\(local\)** steps are things done entirely in your own local machine.



**Step 0** : Have an [account available on AWS](../proposed-solution/amazon-web-services.md)

**Step 1** : Download and install [**OpenVPN \(+ Easy-RSA\)**](../proposed-solution/tools.md#open-vpn-community-free) on your Windows machine \(+[optional Tools](../proposed-solution/tools.md#optional-tools) if you want\)

**Step 2** : \(local\) Create a PKI with its Certificate Authority and public certificates + private keys for the OpenVPN server and at least one VPN client \(= the GM\)

**Step 3** : Setup your Windows machine with OpenVPN and put the required certificates and keys into the config directory

**Step 4** : Configure AWS network security settings, create an S3 bucket and an IAM role

**Step 5** : Finalize all configuration elements

**Step 6** : Launch your FG-OpenVPN Linux server in your AWS Virtual Private Cloud

Connect your VPN and enjoy...

