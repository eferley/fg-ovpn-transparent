# Setup process overview

We'll detail a ~~_very boring_~~ step-by-step complete setup process in the following sections, and add some more technical blocks of info here and there, that non tech-savvy readers can safely ignore 😝 :

> **Tech Blurb**
>
> nerd blah blah

Blocs of "code", like the 2 sample tabs below, can be copy/pasted with the small button at the top-right corner.⚠ Use a [proper text editor](tools.md#your-text-editor-of-choice) to paste, **not Windows Notepad if you want to avoid pain !** ⚠ 

{% tabs %}
{% tab title="Plain text sample.txt" %}
```text
This is a 
sample block of plain text "code"
```
{% endtab %}

{% tab title="Linux shell script sample.sh" %}
```bash
#!/bin/bash
/usr/local/my-awesome-script.sh
```
{% endtab %}
{% endtabs %}

**\(local\)** steps are things done entirely in your own local machine. We'll begin playing with AWS at Step 4.

{% hint style="warning" %}
Note : All our screenshots of Windows Explorer have been done with **visible file extensions**, and we also use some **command prompt windows**, so :

Refer to the **"**[**Check that you know/learn**](../local-setup/step-0-preparation.md#check-that-you-know-learn)**"** helper links if needed...
{% endhint %}



## One-time setup process

### Local setup \(GM machine\)

**Step 0** : **Preparation** : Have an [account available on AWS](amazon-web-services.md#how-to-create-an-aws-account), prepare a "safe" place on your own machine, check you have a proper text editor and know/learn some basics about Windows and its Explorer

**Step 1** : \(local\) [Download and install](../local-setup/step-1-install-openvpn-+-easy-rsa-2.md) **OpenVPN \(+ Easy-RSA\)** on your Windows machine \(+optional Tools if you want\)

**Step 2** : \(local\) [Create a Pubic Key Infrastructure](../local-setup/step-2-create-your-pki.md) with its Certificate Authority and the public certificates + private keys for the OpenVPN server and at least one VPN client \(= the GM\)

**Step 3** : \(local\) [Setup your GM OpenVPN client](../local-setup/step-3-prepare-your-openvpn-connection.md) with the required parameters, certificates and keys

### AWS setup

**Step 4** : \(AWS\) [Configure your AWS environment](../aws-setup-step-by-step/step-4-configure-your-aws-environment.md) : VPC \(Virtual Private Cloud\), networks, security settings, create an S3 bucket and an IAM user and IAM role \(IAM=Identity and Access Management\), prepare parameters and scripts

### Transfer & First Test

**Step 5** : \(local+AWS\) [Transfer files to AWS S3 ](../transfer-first-test/step-5-check-finalize-transfer-files-to-s3.md)for server auto-configuration at launch time

**Step 6** : \(AWS\) [Launch your FG-OpenVPN Linux server](../transfer-first-test/step-6-launch-time.md) in your AWS Virtual Private Cloud

**Step 7** : [Connect your VPN and test...](../transfer-first-test/step-7-connect-and-enjoy.md)

**Step 8** : \(AWS\) [Destroy \(="Terminate"\)](../transfer-first-test/step-8-destroy-terminate-after-use.md) your server

