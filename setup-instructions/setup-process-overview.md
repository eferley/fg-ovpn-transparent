# Setup process overview

==================================================================================================== The Big Picture :

Step 0 : Have an account available on AWS

Step 1 : Download and install OpenVPN \(free community edition\) V2.2 or later for your Windows machine

Step 2 : Create a Certificate Authority and certificates + keys for the OpenVPN server and one client \(= the GM\)

Step 3 : Setup your Windows machine with OpenVPN and put the required certificates and keys into the config directory

Step 4 : Configure AWS network security settings, create an S3 bucket and an IAM role

Step 5 : Finalize all configuration elements

Step 6 : Create your FG-OpenVPN Linux server in your AWS Virtual Private Cloud

Connect your VPN and enjoy...

