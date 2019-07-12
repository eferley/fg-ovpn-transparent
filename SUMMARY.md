# Table of contents

* [Fantasy Grounds - Port Forwarding and a "transparent" OpenVPN setup](README.md)

## General

* [Introduction - F.G.Comms](general/introduction.md)
* [GM's machine barriers](general/gms-machine-barriers.md)
* [Internet connection barriers](general/internet-connection-barriers.md)
* [Some NO-GO Situations](general/some-no-go-situations.md)
* [VPN solutions](general/vpn-solutions.md)

## Proposed Solution

* ["Transparent" OpenVPN-based Port Forwarding](proposed-solution/transparent-openvpn-solution-presentation.md)
* [Tools](proposed-solution/tools.md)
* [Amazon Web Services](proposed-solution/amazon-web-services.md)
* [Setup process overview](proposed-solution/setup-process-overview.md)

## Local setup - Step-by-step <a id="local-setup"></a>

* [Step 0 : Preparation](local-setup/step-0-preparation.md)
* [Step 1 : OpenVPN + Easy-RSA & "new-PKI"](local-setup/step-1-install-openvpn-+-easy-rsa-2.md)
* [Step 2 : Create your own PKI](local-setup/step-2-create-your-pki.md)
* [Step 3 : Setup OpenVPN connections](local-setup/step-3-prepare-your-openvpn-connection.md)

## AWS Setup - Click-by-Click <a id="aws-setup-step-by-step"></a>

* [Step 4 : Your AWS environment](aws-setup-step-by-step/step-4-configure-your-aws-environment.md)
* [4.1-Creating the IAM Role](aws-setup-step-by-step/4.1-creating-the-iam-role.md)
* [4.2-Choosing the AWS Region](aws-setup-step-by-step/4.2-aws-region.md)
* [4.3-Creating the S3 bucket](aws-setup-step-by-step/4.3-creating-the-s3-bucket.md)
* [4.4-Virtual Private Cloud - Default VPC](aws-setup-step-by-step/4.4-virtual-private-cloud-default-vpc.md)
* [4.5-Creating an AWS Key Pair](aws-setup-step-by-step/4.5-creating-an-aws-key-pair.md)
* [4.6-Creating an AWS Security Group](aws-setup-step-by-step/4.6-creating-an-aws-security-group.md)
* [4.7-Filling the bucket](aws-setup-step-by-step/4.7-filling-the-bucket.md)
* [4.8-Building the Launch Template](aws-setup-step-by-step/4.8-building-the-launch-template.md)

## Transfer & First Test <a id="transfer-first-test"></a>

* [Step 5 : Transfer to S3](transfer-first-test/step-5-check-finalize-transfer-files-to-s3.md)
* [Step 6 : Launch time !](transfer-first-test/step-6-launch-time.md)
* [Step 7 : Connect, test, fix glitches](transfer-first-test/step-7-connect-and-enjoy.md)
* [Step 8 : Destroy/"Terminate" after use](transfer-first-test/step-8-destroy-terminate-after-use.md)

## Use your server

* [Regular Usage Pattern](use-your-server/untitled.md)

## Appendixes

* [Acronyms and definitions](appendixes/acronyms-and-definitions.md)
* [AWS acronyms](appendixes/aws-terminology.md)
* [AWS admin user + API access key](appendixes/aws-user-with-api-keys.md)
* [AWS, DNS, DDNS, CRL...](appendixes/aws-and-other-considerations.md)
* [Possible \(?\) developments](appendixes/future-possible-developments.md)

## Links

* [Fantasy Grounds Web](https://www.fantasygrounds.com/)
* [Fantasy Grounds Discord](https://discord.gg/dbuQvgG)
* [FG College Web](https://fantasygroundscollege.net/)
* [FG College Discord](https://discord.gg/Ew6nYyw)
* [Our "EU" Discord](https://discord.gg/6s6WCuZ)
* [OpenVPN](https://openvpn.net/)
* [AWS](https://aws.amazon.com/)

