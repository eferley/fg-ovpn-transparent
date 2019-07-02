# Step-by-step setup -OLD



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

ADD IAM USER

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

```text
My-CA-for-FG-OVPN\my-fg-ovpn-single-dm\
  Must contain :
    my-fg-ovpn-ca.crt (from step 2.4.5)
    my-fg-ovpn-ta.key (from step 3.3)
    my-fg-ovpn-single-dm.ovpn (from zip file or listing below)
    single-dm.crt + single-dm.key (from step 2.4.8)
My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\
My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\
  Can contain (optional, not directly used by the linux box) :
    my-fg-ovpn-UserData.txt
My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\init\
  Must contain :
    my-fg-ovpn-init.sh (from zip file or listing below)
My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\conf\
My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\conf\my-fg-ovpn\
  Must contain :
    my-fg-ovpn-ca.crt (from step 2.4.5)
    my-fg-ovpn-dh2048.pem (from step 2.4.6)
    my-fg-ovpn.crt + my-fg-ovpn.csr + my-fg-ovpn.key (from step 2.4.7)
    my-fg-ovpn-ta.key (from step 3.3)
My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\refresh\
My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\refresh\my-fg-ovpn\
My-CA-for-FG-OVPN\my-very-own-fg-ovpn-bucket\deploy\refresh\my-fg-ovpn\crl\
  Must contain :
    my-fg-ovpn-crl.pem (from step 2.4.9)

```

5.2- Edit + transfer to AWS S3 bucket 

Please edit this bloc of 3 lines \(lines 4 to 6\) in my-fg-ovpn-UserData.txt to reflect your choices : export MY\_STARTUP\_CONFIG\_NAME=my-fg-ovpn export MY\_STARTUP\_VPNSubNet=10.10.10.0/24 export MY\_STARTUP\_S3\_BUCKET=my-very-own-fg-ovpn-bucket Then use "Cloudberry Explorer for Amazon S3" to transfer the whole deploy folder to your S3 bucket.

5.3- Copy the contents of My-CA-for-FG-OVPN\my-fg-ovpn-single-dm to your \config subdirectory

5.4- If OpenVPN GUI is already started, then exit it and restart it AS AN ADMINISTRATOR. Then let it sleep as a notification bar icon for the moment...

====================================================================================================

Details for Step 6 : Create your FG-OpenVPN Linux server in your AWS Virtual Private Cloud

6.0- Head over to the AWS EC2 Console6.1- Choose an AZ/subnet in your AWS VPC, with Internet access \(you will need a private IP + a public IP\) 6.2- Select a private IP for your OpenVPN server in the subnet you chose if you want it to be static \(not required if using my auto-configuration script\) 6.3- Create/Launch a new instance : 6.3.1- Select the basic Amazon Linux AMI \(64-bit, HVM, SSD volume type\) 6.3.2- Select a small \(free tier elligible\) t2.micro instance type and click "Next: Configure Instance Details" 6.3.3- Instance Details : 6.3.3.1- Select Your Network/VPC and your AZ/subnet, be sure to have Auto-assign Public IP=Enable 6.3.3.2- Select your EC2 role \(from step 4.3\) 6.3.3.3- If you chose a static Private IP at step 6.2, specify it in the "eth0" network interface "Primary IP" 6.3.3.4- Expand the "Advanced Details" section and copy "as text" the entire contents of my-fg-ovpn-UserData.txt \(properly edited at step 5.2\) in the "UserData" entry box. 6.3.3.9- Finally, click "Next: Add Storage" 6.3.4- Keep all defaults and click "Next: Add Tags" 6.3.5- If you want, you can add tags \(name/value pairs\) like Name : MyFGovpn, finish with a click on "Next: Configure Security Group" 6.3.6- Select the Security Group you created at Step 4.1, then click "Review and Launch" 6.3.7- You can review all the settings and go back to previous steps to edit your choices, before ending with the "Launch" button 6.3.8- Then if you don't already have an AWS SSH key you'll need to create one and download it \(to be used with PuTTY for SSH access\) 6.4- While you server is being created and begins it's auto-configuration, copy it's Public IP 6.5- Using your Windows OpenVPN GUI, right click/edit config to enter the Public IP on the "remote" line, then close/save the file. 6.6- Enjoy !

Important : Remember this: with these basic settings, each time you start your virtual machine it will get a new public IP, so you'll have to :

* update your OpenVPN connection parameters with the new public IP \(the "remote" line in the client .ovpn parameter file, which you can edit from OpenVPN GUI\)
* communicate the new public IP to your players

  \(you can of course manage your public IPs much better if you own a domain name and/or have a DDNS account, use AWS "Elastic IPs", etc...\)



