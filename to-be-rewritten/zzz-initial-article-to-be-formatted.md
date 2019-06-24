# ZZZ-Initial raw article to be re-written

A small OpenVPN virtual server on AWS to publish Fantasy Grounds through a 1-DM-only VPN By ZeFerby

Objective :

A solution for your local FG machine to be reachable from players on the Internet \(connections to your TCP 1802 port\). Your players DO NOT NEED any kind of specific setup \(unlike solutions like Hamachi\) ONLY THE DM uses the VPN, and ONLY for the TCP 1802 port used by FG : your usual Internet access route stays "as is"

Requirements :

Setup a minimalist Linux-based virtual server \(that is needed only during game sessions\) Install OpenVPN 2.2+ on your Windows machine Prepare the configs and security aspects !! Have the Windows firewall accept inbound TCP 1802 connections for Fantasy Grounds \(and tag the VPN network as a private on W10\)

This example will be detailed for a virtual server on AWS = Amazon Web Services, using the basic Amazon Linux AMI. New AWS accounts can run a small virtual server 24x7 for one year. The full documentation of Amazon Web Services is available at [https://docs.aws.amazon.com/](https://docs.aws.amazon.com/)

!! STRONGLY RECOMMENDED TOOLS !! Cloudberry Explorer for Amazon S3 \(free version is ok\) to easily transfer folders/files between your Windows machine and your AWS storage. If you need SSH sessions from Windows to a Linux box, the "goto" software is the free and excellent PuTTY.

We will build an auto-configured virtual server thanks to some paramater and script files that will be kept in a privte AWS storage space \(an "S3 bucket"\). One advantage of this technique is that you can destroy \("terminate"\) and re-create your server at will.

==================================================================================================== The Big Picture :

Step 0 : Have an account available on AWS Step 1 : Download and install OpenVPN \(free community edition\) V2.2 or later for your Windows machine Step 2 : Create a Certificate Authority and certificates + keys for the OpenVPN server and one client \(= the GM\) Step 3 : Setup your Windows machine with OpenVPN and put the required certificates and keys into the config directory Step 4 : Configure AWS network security settings, create an S3 bucket and an IAM role Step 5 : Finalize all configuration elements Step 6 : Create your FG-OpenVPN Linux server in your AWS Virtual Private Cloud

Connect your VPN and enjoy...

====================================================================================================

Details for Step 2 : Create a Certificate Authority \(CA\) and certificates + keys

2.0- This is where the security of the VPN PKI infrastructure lies, so you want to build it yourself !

2.1- Prepare a "safe" CA storage : create a dedicated folder somewhere on your hard drive, that we call  \(example: My-CA-for-FG-OVPN\) 2.2- Unzip or copy the contents of Easy-RSA \(I use v2.2.2-1\) into  2.3- Open a command line \(cmd.exe\) and go into your  2.4- In the command line window into  : 2.4.1- To initialize everything, run the command : init-config.bat 2.4.2- !! VITAL !! - Edit vars.bat with the proper value for the HOME environment variable : set it to your  2.4.3- From now on, you will have to run the vars.bat command before any other, so that your environment variables are properly set, so run it now : vars.bat !! Remember to do this again if you close your command line window and reopen a new one !! 2.4.4- Create new empty index and serial files \(once only\) with the command : clean-all.bat 2.4.5- Build a CA key+certificate \(once only\) with the command : build-ca.bat =&gt; files for the OpenVPN Server and Client : ca.crt KEEP the file ca.key in your CA safe place, neither the OpenVPN server nor the client need it ! 2.4.6- Build a Diffie-Hellman file \(once only\) with the command : build-dh.bat =&gt; files for the OpenVPN Server ONLY : dh2048.pem 2.4.7- Build a private key/certficate for the OpenVPN server \(once only\) with the command : build-key-server.bat  !! To avoid errors, use a simple  and specify the same when prompted for the "common name" !! =&gt; files for the OpenVPN Server ONLY : .crt, .csr and .key 2.4.8- Build key files in PEM format \(for each client machine\) : only once, for the single DM session that will be supported, with the command : build-key.bat  !! Once again, to avoid errors, use a simple  like single-dm and specify the same when prompted for the "common name" !! =&gt; files for the OpenVPN Client ONLY : .crt and .key

Optional, recommended : Certificate Revocation List \(CRL\)

2.4.9- Build a dummy certificate+key to test its revocation : 2.4.9.1- Use the command : build-key.bat a-dummy-name \(same process as 2.4.8\) 2.4.9.2- Use the command : revoke-full a-dummy-name That will create a crl.pem file for you, and you should see the "certificate revoked" at the end of the command output =&gt; files for the OpenVPN Server ONLY : crl.pem that you can copy for example as my-fg-ovpn-crl.pem

Optional, recommended: This is especially useful if you maintain connections to multiple OpenVPN servers, linked to multiple CAs, as you will need multiple CA certificates, etc in the OpenVPN config directory Copy your CA certificate+key \(ca.crt, ca.key\) for example as my-fg-ovpn-ca.crt and .key Also copy your DH file \(dh2048.pem\) for example as my-fg-ovpn-dh2048.pem

Also copy your crl.pem for example as my-fg-ovpn-crl.pem

Details for Step 3 : Setup your Windows machine with OpenVPN and put the required certificates and keys into the config directory

3.1- If not yet done, run the Windows installation for OpenVPN, then make sure you ALWAYS start the OpenVPN GUI as administrator 3.2- Copy these files from step 2 results into your \config subdirectory \(Windows will require you to do this as administrator\) :

* my-fg-ovpn-ca.crt
* .crt
* .key

  3.3- Generate a TLS authentication key for the OpenVPN Server and Client to protect session startup negociation :

  3.3.1- Start a command line \(cmd.exe\) as administrator and go into your 

  3.3.2- Generate a TLS authentication key by running this command : openvpn --genkey --secret my-fg-ovpn-ta.key

  3.3.3- Move the resulting key file to your \config subdirectory and keep a copy of it to send to your server \(for example in a sub-folder of \)

====================================================================================================

Details for Step 4 : Configure AWS network security settings, create an S3 bucket and an IAM role

4.1- Security Groups Without trying to replace the AWS documentation, let's just say AWS Security Groups are like firewall rules. Within the AWS EC2 console, you can dynamically assign Security Groups to machine instances, change the rules of existing Security Groups, etc... Note: If you follow the method given here with the auto-configuring scripts, SSH access will not be needed... You will need to authorize inbound network traffic to your OpenVPN virtual server :

* for your SSH connection into Linux \(TCP 22\) : at least from your own home public IP \(but this IP may change over time\)
* for your OpenVPN client \(UDP 1194\) : at least from your own home public IP \(but this IP may change over time\)
* for your players : from all the Internet, but only for the TCP 1802 port \(the Fantasy Grounds connection port\)

  So head over to the AWS EC2 console and create a Security Group with the following inbound rules :

  Inbound SSH = TCP 22 from "My IP"

  Inbound OpenVPN = UDP 1194 from "My IP"

  Inbound FG = TCP 1802 from "Anywhere"

4.2- Create an S3 bucket This is a storage space where you will transfer the files for your virtual server to use during auto-configuration. Keep all the defaults, choosing a name that you will have to apply in parameters later on \(example: my-very-own-fg-ovpn-bucket\)

4.3- Create an IAM role for your EC2 virtual server In order for your virtual server to auto-configure, it will need authorizations to change one of its own configuration properties, and to download files from the S3 bucket you just created : this is what this role is for \(IAM is the Identity and Access Management\). To make this easy we'll create a role that is "too powerful", but if you want to tighten security here you'll have to read some \(a lot of ?\) AWS docs.... So head over to the AWS IAM console and create a new Group, give it a name \(example: my-fg-ovpn-role\) and add 2 AWS-managed security policies to it : AmazonEC2FullAccess and AmazonS3FullAccess

====================================================================================================

Details for Step 5 : Finalize all configuration elements

5.1- Keep local source folders on your Windows machine

I suggest your create a hierarchy of subfolders in your "CA safe storage" \(\) to store parameter/security files. All the names I use are example names, bu sure to use your own and edit appropriately ! I provide a zip file to initialize this hierarchy, that you'll have to complete with your own files :

My-CA-for-FG-OVPN\my-fg-ovpn-single-dm Must contain : my-fg-ovpn-ca.crt \(from step 2.4.5\) my-fg-ovpn-ta.key \(from step 3.3\) my-fg-ovpn-single-dm.ovpn \(from zip file or listing below\) single-dm.crt + single-dm.key \(from step 2.4.8\) My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy Can contain \(optional, not directly used by the linux box\) : my-fg-ovpn-UserData.txt My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\init Must contain : my-fg-ovpn-init.sh \(from zip file or listing below\) My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\conf My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\conf\my-fg-ovpn Must contain : my-fg-ovpn-ca.crt \(from step 2.4.5\) my-fg-ovpn-dh2048.pem \(from step 2.4.6\) my-fg-ovpn.crt + my-fg-ovpn.csr + my-fg-ovpn.key \(from step 2.4.7\) my-fg-ovpn-ta.key \(from step 3.3\) My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\refresh My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\refresh\my-fg-ovpn My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\refresh\my-fg-ovpn\crl Must contain : my-fg-ovpn-crl.pem \(from step 2.4.9\)

5.2- Edit + transfer to AWS S3 bucket Please edit this bloc of 3 lines \(lines 4 to 6\) in my-fg-ovpn-UserData.txt to reflect your choices : export MY\_STARTUP\_CONFIG\_NAME=my-fg-ovpn export MY\_STARTUP\_VPNSubNet=10.10.10.0/24 export MY\_STARTUP\_S3\_BUCKET=my-very-own-fg-ovpn-bucket Then use "Cloudberry Explorer for Amazon S3" to transfer the whole deploy folder to your S3 bucket.

5.3- Copy the contents of My-CA-for-FG-OVPN\my-fg-ovpn-single-dm to your \config subdirectory

5.4- If OpenVPN GUI is already started, then exit it and restart it AS AN ADMINISTRATOR. Then let it sleep as a notification icon for the moment...

====================================================================================================

Details for Step 6 : Create your FG-OpenVPN Linux server in your AWS Virtual Private Cloud

6.0- Head over to the AWS EC2 Console6.1- Choose an AZ/subnet in your AWS VPC, with Internet access \(you will need a private IP + a public IP\) 6.2- Select a private IP for your OpenVPN server in the subnet you chose if you want it to be static \(not required if using my auto-configuration script\) 6.3- Create/Launch a new instance : 6.3.1- Select the basic Amazon Linux AMI \(64-bit, HVM, SSD volume type\) 6.3.2- Select a small \(free tier elligible\) t2.micro instance type and click "Next: Configure Instance Details" 6.3.3- Instance Details : 6.3.3.1- Select Your Network/VPC and your AZ/subnet, be sure to have Auto-assign Public IP=Enable 6.3.3.2- Select your EC2 role \(from step 4.3\) 6.3.3.3- If you chose a static Private IP at step 6.2, specify it in the "eth0" network interface "Primary IP" 6.3.3.4- Expand the "Advanced Details" section and copy "as text" the entire contents of my-fg-ovpn-UserData.txt \(properly edited at step 5.2\) in the "UserData" entry box. 6.3.3.9- Finally, click "Next: Add Storage" 6.3.4- Keep all defaults and click "Next: Add Tags" 6.3.5- If you want, you can add tags \(name/value pairs\) like Name : MyFGovpn, finish with a click on "Next: Configure Security Group" 6.3.6- Select the Security Group you created at Step 4.1, then click "Review and Launch" 6.3.7- You can review all the settings and go back to previous steps to edit your choices, before ending with the "Launch" button 6.3.8- Then if you don't already have an AWS SSH key you'll need to create one and download it \(to be used with PuTTY for SSH access\) 6.4- While you server is being created and begins it's auto-configuration, copy it's Public IP 6.5- Using your Windows OpenVPN GUI, right click/edit config to enter the Public IP on the "remote" line, then close/save the file. 6.6- Enjoy !

Important : Remember this: with these basic settings, each time you start your virtual machine it will get a new public IP, so you'll have to :

* update your OpenVPN connection parameters with the new public IP \(the "remote" line in the client .ovpn parameter file, which you can edit from OpenVPN GUI\)
* communicate the new public IP to your players

  \(you can of course manage your public IPs much better if you own a domain name and/or have a DDNS account, use AWS "Elastic IPs", etc...\)

====================================================================================================

### AWS UserData script : my-fg-ovpn-UserData.txt

{% hint style="danger" %}
**Please edit this bloc of 3 lines \(lines 4 to 6\) in my-fg-ovpn-UserData.txt to reflect your choices :**

```bash
#!/bin/bash
echo `date` "UserData script starting - Args : $@"

export MY_STARTUP_CONFIG_NAME=my-fg-ovpn
export MY_STARTUP_VPNSubNet=10.10.10.0/24
export MY_STARTUP_S3_BUCKET=my-very-own-fg-ovpn-bucket
```
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="my-fg-ovpn-UserData.txt" %}
```bash
#!/bin/bash
echo `date` "UserData script starting - Args : $@"

export MY_STARTUP_CONFIG_NAME=my-fg-ovpn
export MY_STARTUP_VPNSubNet=10.10.10.0/24
export MY_STARTUP_S3_BUCKET=my-very-own-fg-ovpn-bucket

export MY_STARTUP_S3_DEPLOY_URL=s3://${MY_STARTUP_S3_BUCKET}/deploy

echo `date` "MY_FGVPN --- STARTUP CONFIG NAME   : ${MY_STARTUP_CONFIG_NAME}"
echo `date` "MY_FGVPN --- STARTUP VPN Subnet    : ${MY_STARTUP_VPNSubNet}"
echo `date` "MY_FGVPN --- STARTUP S3 Deploy URL : ${MY_STARTUP_S3_DEPLOY_URL}"

rm -rf /usr/local/${MY_STARTUP_CONFIG_NAME}
mkdir /usr/local/${MY_STARTUP_CONFIG_NAME}
mkdir /usr/local/${MY_STARTUP_CONFIG_NAME}/boot
mkdir /usr/local/${MY_STARTUP_CONFIG_NAME}/init

echo `date` "UserData script getting ${MY_STARTUP_CONFIG_NAME} init hierarchy from ${MY_FGVPN_S3_BUCKET} S3 bucket (from /deploy/init)"
echo `date` "THIS REQUIRES AN ASSIGNED EC2 ROLE"
aws s3 cp ${MY_STARTUP_S3_DEPLOY_URL}/init /usr/local/${MY_STARTUP_CONFIG_NAME}/init --recursive
chmod -R u+x /usr/local/${MY_STARTUP_CONFIG_NAME}/init/*.sh

echo `date` "UserData script building config vars sourcing script - /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs"
echo "export MY_FGVPN_CONFIG_NAME=${MY_STARTUP_CONFIG_NAME}" > /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs
echo "export MY_FGVPN_VPNSubNet=${MY_STARTUP_VPNSubNet}" >> /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs
echo "export MY_FGVPN_S3_BUCKET=${MY_STARTUP_S3_BUCKET}" >> /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs
echo "export MY_FGVPN_S3_DEPLOY_URL=${MY_STARTUP_S3_DEPLOY_URL}" >> /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs
echo 'echo `date` "MY_FGVPN --- CONFIG NAME   : ${MY_FGVPN_CONFIG_NAME}"' >> /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs
echo 'echo `date` "MY_FGVPN --- VPN Subnet    : ${MY_FGVPN_VPNSubNet}"' >> /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs
echo 'echo `date` "MY_FGVPN --- S3 Deploy URL : ${MY_FGVPN_S3_DEPLOY_URL}"' >> /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs

echo `date` "UserData script running ${MY_STARTUP_CONFIG_NAME}-init.sh"
/usr/local/${MY_STARTUP_CONFIG_NAME}/init/${MY_STARTUP_CONFIG_NAME}-init.sh
echo `date` "UserData script finished"

```
{% endcode-tabs-item %}
{% endcode-tabs %}

### OpenVPN Client parameter file : my-fg-ovpn-single-dm.ovpn

{% code-tabs %}
{% code-tabs-item title="my-fg-ovpn-single-dm.ovpn" %}
```text
client
float
dev tun
; dev-node <name of your Tap virtual network adapter in Windows>
proto udp
remote <OpenVPN Server Public IP or FQDN> 1194
resolv-retry infinite
nobind
persist-key
persist-tun
;http-proxy-retry # retry on connection failures
;http-proxy [proxy server] [proxy port #]
;mute-replay-warnings
ca my-fg-ovpn-ca.crt
cert single-dm.crt
key single-dm.key
ns-cert-type server
tls-auth my-fg-ovpn-ta.key 1
comp-lzo
verb 4

```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Server auto-configuration shell script : my-fg-ovpn-init.sh

{% code-tabs %}
{% code-tabs-item title="my-fg-ovpn-init.sh" %}
```bash
#!/bin/bash

#set -x

echo `date` "+++ $0 starting with $# args : $@"
echo `date` "+++ $0 sourcing static config vars from /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs"
. /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs

# From now on, the MY_FGVPN_xxxx variables will be used - we won't need the MY_STARTUP_xxxx anymore

echo `date` "+++ $0 UPDATING packages"
yum -y update

echo `date` "+++ $0 INSTALLING Midnight Commander"
yum -y install mc

echo `date` "+++ $0 Creating AWS metadata vars sourcing script"

echo "#!/bin/bash" > /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo "" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo "# set -x" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo "" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo 'export MY_FGVPN_INSTANCE_ID=`/usr/bin/curl --silent http://169.254.169.254/latest/meta-data/instance-id`' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo 'export MY_FGVPN_AZ=`/usr/bin/curl --silent http://169.254.169.254/latest/meta-data/placement/availability-zone`' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo 'export MY_FGVPN_REGION=${MY_FGVPN_AZ:0:${#MY_FGVPN_AZ}-1}' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo "" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo 'export MY_FGVPN_PUBLIC_IP=`/usr/bin/curl --silent http://169.254.169.254/latest/meta-data/public-ipv4`' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo 'export MY_FGVPN_PRIVATE_IP=`/usr/bin/curl --silent http://169.254.169.254/latest/meta-data/local-ipv4`' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo "" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo 'export MY_FGVPN_MAC_ETH0=`/usr/bin/curl --silent http://169.254.169.254/latest/meta-data/mac`' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo 'export MY_FGVPN_VPC_CIDR_URL="http://169.254.169.254/latest/meta-data/network/interfaces/macs/${MY_FGVPN_MAC_ETH0}/vpc-ipv4-cidr-block"' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo 'export MY_FGVPN_VPC_CIDR_RANGE=`/usr/bin/curl --silent ${MY_FGVPN_VPC_CIDR_URL}`' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo "" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo 'echo `date` "MY_FGVPN --- InstanceId : ${MY_FGVPN_INSTANCE_ID} - Region : ${MY_FGVPN_REGION} - AZ : ${MY_FGVPN_AZ}"' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo 'echo `date` "MY_FGVPN --- Public IP  : ${MY_FGVPN_PUBLIC_IP} - Private IP : ${MY_FGVPN_PRIVATE_IP}"' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo 'echo `date` "MY_FGVPN --- ETH0 MAC   : ${MY_FGVPN_MAC_ETH0}"' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars
echo 'echo `date` "MY_FGVPN --- VPC CIDR   : ${MY_FGVPN_VPC_CIDR_RANGE}"' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars


# no need to chmod for sourcing
# chmod u+x /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars

echo `date` "+++ $0 sourcing AWS metadata vars"
. /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars

# From now on, we have the full set of MY_FGVPN_xxxx variables

# ----------------------------- EC2 SOURCE/DEST CHECK -------------------------

echo `date` "+++ $0 REMOVING EC2 SOURCE/DEST CHECK FOR NAT/PAT"
echo `date` "+++ $0 THIS REQUIRES AN ASSIGNED EC2 ROLE"
aws ec2 modify-instance-attribute --region ${MY_FGVPN_REGION} --instance-id ${MY_FGVPN_INSTANCE_ID} --no-source-dest-check

# ----------------------------- IP FORWARDING + PAT, CRL REFRESH, FG PUBLISHING => boot script -------------------------

echo `date` "+++ $0 Creating IP Forwarding and PAT script - boot/ipfw_pat.sh"

echo "#!/bin/bash" > /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo "" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo ". /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo ". /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo 'echo 1 > /proc/sys/net/ipv4/ip_forward && echo 0 > /proc/sys/net/ipv4/conf/eth0/send_redirects' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo 'echo "Activating MASQUERADE"' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo '/sbin/iptables -t nat -A POSTROUTING -o eth0 -s ${MY_FGVPN_PRIVATE_IP}/32 -j MASQUERADE' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo '# /sbin/iptables -t nat -A POSTROUTING -o eth0 -s ${MY_FGVPN_VPC_CIDR_RANGE} -j MASQUERADE' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo '# /sbin/iptables -t nat -A POSTROUTING -o eth0 -s ${MY_FGVPN_VPNSubNet} -j MASQUERADE' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo 'echo "Finished ipfw_pat.sh script"' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh


echo `date` "+++ $0 Creating ipfw_publish boot script - boot/ipfw_publish.sh"

# This is the magic of iptables

echo "#!/bin/bash" > /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_publish.sh
echo "" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_publish.sh
echo "# set -x" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_publish.sh
echo "" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_publish.sh
echo ". /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_publish.sh
echo ". /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_publish.sh
echo "" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_publish.sh
echo '/sbin/iptables -t nat -A PREROUTING -i eth0 -p tcp --dst ${MY_FGVPN_PRIVATE_IP} --dport 1802 -j DNAT --to-destination ${MY_FGVPN_VPNSubNet:0:${#MY_FGVPN_VPNSubNet}-5}.6:1802' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_publish.sh
echo '/sbin/iptables -t nat -A POSTROUTING -p tcp --dst ${MY_FGVPN_VPNSubNet:0:${#MY_FGVPN_VPNSubNet}-5}.6 --dport 1802 -j SNAT --to-source ${MY_FGVPN_PRIVATE_IP}' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_publish.sh
echo 'echo "Finished ipfw_publish.sh script"' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_publish.sh



echo `date` "+++ $0 Creating boot/${MY_FGVPN_CONFIG_NAME}-boot.sh script with calls to ipfw_pat.sh, crl_refresh.sh and ipfw_publish.sh"

cat << OVPNBOOTEOF  > /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/${MY_FGVPN_CONFIG_NAME}-boot.sh
#!/bin/bash

. /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs
. /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars

/usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
/usr/local/${MY_FGVPN_CONFIG_NAME}/boot/crl_refresh.sh
/usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_publish.sh

OVPNBOOTEOF

chmod u+x /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/*.sh


echo `date` "+++ $0 Adding call to the /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/${MY_FGVPN_CONFIG_NAME}-boot.sh script at the end of /etc/rc.d/rc.local"

echo "" >> /etc/rc.d/rc.local
echo "# added by $0 :" >> /etc/rc.d/rc.local
echo "/usr/local/${MY_FGVPN_CONFIG_NAME}/boot/${MY_FGVPN_CONFIG_NAME}-boot.sh $@" >> /etc/rc.d/rc.local
echo "" >> /etc/rc.d/rc.local


# ---------------------------- INSTALL OPENVPN + OVPN SERVER CONFIGS ---------------------------------

echo `date` "+++ $0 INSTALLING conntrack-tools"
yum -y install conntrack-tools

echo `date` "+++ $0 INSTALLING openvpn"
yum -y install openvpn

OVPNROOT="/etc/openvpn"

OVPN_CA="${MY_FGVPN_CONFIG_NAME}-ca"
OVPNCAPATH="${OVPNROOT}/${OVPN_CA}"

OVPNSRVNAME="${MY_FGVPN_CONFIG_NAME}"
OVPNSRVPATH="${OVPNROOT}/server-${OVPNSRVNAME}"

echo `date` "+++ $0 Building server dir : ${OVPNSRVPATH}"
mkdir ${OVPNCAPATH}/
mkdir ${OVPNSRVPATH}/
mkdir ${OVPNSRVPATH}/${OVPNSRVNAME}-ccd/
aws s3 cp ${MY_FGVPN_S3_DEPLOY_URL}/conf/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-ca.crt ${OVPNCAPATH}/
aws s3 cp ${MY_FGVPN_S3_DEPLOY_URL}/conf/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-ta.key ${OVPNCAPATH}/
aws s3 cp ${MY_FGVPN_S3_DEPLOY_URL}/conf/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-dh2048.pem ${OVPNCAPATH}/
aws s3 cp ${MY_FGVPN_S3_DEPLOY_URL}/conf/${MY_FGVPN_CONFIG_NAME}/${OVPNSRVNAME}.crt ${OVPNSRVPATH}/
aws s3 cp ${MY_FGVPN_S3_DEPLOY_URL}/conf/${MY_FGVPN_CONFIG_NAME}/${OVPNSRVNAME}.csr ${OVPNSRVPATH}/
aws s3 cp ${MY_FGVPN_S3_DEPLOY_URL}/conf/${MY_FGVPN_CONFIG_NAME}/${OVPNSRVNAME}.key ${OVPNSRVPATH}/

# The CRL must be regularly refreshed !
aws s3 cp ${MY_FGVPN_S3_DEPLOY_URL}/refresh/${MY_FGVPN_CONFIG_NAME}/crl/${MY_FGVPN_CONFIG_NAME}-crl.pem ${OVPNCAPATH}/

chown root:root ${OVPNSRVPATH}/* ${OVPNCAPATH}/*
chmod 600 ${OVPNSRVPATH}/${OVPNSRVNAME}.key
chmod 600 ${OVPNCAPATH}/${MY_FGVPN_CONFIG_NAME}-dh2048.pem


echo `date` "+++ $0 Creating CRL Refresh boot script - boot/crl_refresh.sh"
# This must be done only when we have the OVPNxxx local variables

echo "#!/bin/bash" > /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/crl_refresh.sh
echo "" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/crl_refresh.sh
echo ". /usr/local/${MY_STARTUP_CONFIG_NAME}/conf-specs" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/crl_refresh.sh
echo ". /usr/local/${MY_FGVPN_CONFIG_NAME}/${MY_FGVPN_CONFIG_NAME}-source-aws-vars" >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/crl_refresh.sh
echo 'aws s3 cp ${MY_FGVPN_S3_DEPLOY_URL}/refresh/${MY_FGVPN_CONFIG_NAME}/crl/${MY_FGVPN_CONFIG_NAME}-crl.pem ${OVPNCAPATH}/' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/crl_refresh.sh
echo 'service openvpn restart' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/crl_refresh.sh
echo 'echo "Finished crl_refresh.sh script"' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/crl_refresh.sh

chmod u+x /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/crl_refresh.sh


echo `date` "+++ $0 Creating server-${OVPNSRVNAME}.conf"

cat << SRVEOF  > ${OVPNSRVPATH}.conf
; local ${MY_FGVPN_PRIVATE_IP}
port 1194
proto udp
dev tun
ca ${OVPNCAPATH}/${MY_FGVPN_CONFIG_NAME}-ca.crt
dh ${OVPNCAPATH}/${MY_FGVPN_CONFIG_NAME}-dh2048.pem
crl-verify ${OVPNCAPATH}/${MY_FGVPN_CONFIG_NAME}-crl.pem
cert ${OVPNSRVPATH}/${OVPNSRVNAME}.crt
key ${OVPNSRVPATH}/${OVPNSRVNAME}.key
; topology subnet
server ${MY_FGVPN_VPNSubNet:0:${#MY_FGVPN_VPNSubNet}-3} 255.255.255.0
ifconfig-pool-persist ${OVPNSRVPATH}/${OVPNSRVNAME}-ipp.txt
client-config-dir ${OVPNSRVPATH}/${OVPNSRVNAME}-ccd
# CIDR for this VPN
push "route ${MY_FGVPN_VPNSubNet:0:${#MY_FGVPN_VPNSubNet}-3} 255.255.255.0"
# CIDR for this server private IP in the VPC (not the full VPC since is not needed for FG)
push "route ${MY_FGVPN_PRIVATE_IP} 255.255.255.255"
route ${MY_FGVPN_PRIVATE_IP} 255.255.255.255
# CIDRs for other networks available through this server
; route X.Y.0.0 255.255.0.0
; push "redirect-gateway def1 bypass-dhcp"
; push "dhcp-option DNS a.b.c.d"
; client-to-client
keepalive 10 60
; inactive 120
tls-server
tls-auth ${OVPNCAPATH}/${MY_FGVPN_CONFIG_NAME}-ta.key 0
; cipher BF-CBC
comp-lzo
; max-clients 50
user nobody
group nobody
persist-key
persist-tun
status ${OVPNSRVPATH}-status.log 5
log ${OVPNSRVPATH}-log.log
; log-append ${OVPNSRVPATH}-log.log
verb 5
; mute 20
SRVEOF


# Initialize ccd ??? Nah, not for a single session...


echo `date` "+++ $0 Setting autostart for Openvpn and server-${OVPNSRVNAME} startup"
service openvpn start
chkconfig openvpn on

echo `date` "+++ $0 OPENVPN INSTALLED AND STARTED"

# --------------------------------------------------------------------------------------------------

echo `date` "+++ $0 finished - Args : $@"

exit 0

```
{% endcode-tabs-item %}
{% endcode-tabs %}



