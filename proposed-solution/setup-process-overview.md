# Setup process overview

We'll detail a ~~_boring_~~ step-by-step complete setup process in the following sections, and add some more technical blocks of info here and there, that non tech-savvy readers can safely ignore 😝 :

> **Tech Blurb**
>
> nerd blah blah

Blocs of "code", like the 2 sample tabs below, can be copy/pasted with the small button at the top-right corner.⚠ Use a [proper text editor](tools.md#your-text-editor-of-choice) to paste, **not Windows Notepad if you want to avoid pain !** ⚠ 

{% code-tabs %}
{% code-tabs-item title="Plain text sample.txt" %}
```text
This is a 
sample block of plain text "code"
```
{% endcode-tabs-item %}

{% code-tabs-item title="Linux shell script sample.sh" %}
```bash
#!/bin/bash
/usr/local/my-awesome-script.sh
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**\(local\)** steps are things done entirely in your own local machine. We'll begin playing with AWS at Step 4.

{% hint style="warning" %}
Note : All our screenshots of Windows Explorer have been done with **visible file extensions**, and we also use some **command prompt windows**, so :

Refer to the **"**[**Check that you know/learn**](../setup-instructions/step-0-preparation.md#check-that-you-know-learn)**"** helper links if needed...
{% endhint %}



## One-time setup process

### Local setup \(GM machine\)

**Step 0** : **Preparation** : Have an [account available on AWS](amazon-web-services.md#how-to-create-an-aws-account), prepare a "safe" place on your own machine, check you have a proper text editor

**Step 1** : \(local\) [Download and install](../setup-instructions/step-1-install-openvpn-+-easy-rsa-2.md) **OpenVPN \(+ Easy-RSA\)** on your Windows machine \(+optional Tools if you want\)

**Step 2** : \(local\) [Create a Pubic Key Infrastructure](../setup-instructions/step-2-create-your-pki.md) with its Certificate Authority and the public certificates + private keys for the OpenVPN server and at least one VPN client \(= the GM\)

**Step 3** : \(local\) [Setup your GM OpenVPN client](../setup-instructions/step-3-prepare-your-openvpn-connection.md) with the required certificates and keys into the config directory

### AWS setup

**Step 4** : \(AWS\) [Configure your AWS environment](../setup-instructions/step-4-configure-your-aws-environment.md) : VPC \(Virtual Private Cloud\), networks, security settings, create an S3 bucket and an IAM user and IAM role \(IAM=Identity and Access Management\)

### Finalization

**Step 5** : \(local+AWS\) [Check your settings, Finalize all](../setup-instructions/step-5-check-finalize-transfer-files-to-s3.md) configuration elements and transfer files to AWS S3 for server auto-configuration at launch time



## Repeatable usage process

**Step 6** : \(AWS\) [Launch your FG-OpenVPN Linux server](../setup-instructions/step-6-launch-time.md) in your AWS Virtual Private Cloud

**Step 7** : [Connect your VPN and enjoy...](../setup-instructions/step-7-connect-and-enjoy.md)

**Step 8** : \(AWS\) [Destroy \(="Terminate"\)](../setup-instructions/step-8-destroy-terminate-after-use.md) your server

