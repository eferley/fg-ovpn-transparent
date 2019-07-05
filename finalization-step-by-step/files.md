# Utility files and scripts -OLD



#### AWS "UserData" script :

#### my-fg-ovpn-UserData.txt

{% hint style="danger" %}
**Please edit this bloc of 3 lines \(lines 4 to 6\) in my-fg-ovpn-UserData.txt to reflect your choices :**

```bash
#!/bin/bash
echo `date` "UserData script starting - Args : $@"

export MY_STARTUP_CONFIG_NAME=your-configuration-name
export MY_STARTUP_S3_BUCKET=your-s3-bucket-name
export MY_STARTUP_VPNSubNet=10.10.10.0/24

```
{% endhint %}

{% code-tabs %}
{% code-tabs-item title="my-fg-ovpn-UserData.txt" %}
```bash
#!/bin/bash
echo `date` "UserData script starting - Args : $@"

export MY_STARTUP_CONFIG_NAME=your-configuration-name
export MY_STARTUP_S3_BUCKET=your-s3-bucket-name
export MY_STARTUP_VPNSubNet=10.10.10.0/24

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

#### OpenVPN Client parameter file : 

#### my-fg-ovpn-single-dm.ovpn

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

#### Server auto-configuration shell script : 

#### my-fg-ovpn-init.sh

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
echo '# That enables proxying just for our own private IP' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo '/sbin/iptables -t nat -A POSTROUTING -o eth0 -s ${MY_FGVPN_PRIVATE_IP}/32 -j MASQUERADE' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo '# That would enable proxying to the Internet for the whole VPC CIDR' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo '# /sbin/iptables -t nat -A POSTROUTING -o eth0 -s ${MY_FGVPN_VPC_CIDR_RANGE} -j MASQUERADE' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
echo '# That would enalbe proxying to the Internet for VPN clients' >> /usr/local/${MY_FGVPN_CONFIG_NAME}/boot/ipfw_pat.sh
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


# ---------------------------- INSTALL CONNTRACK-TOOLS + OPENVPN + OVPN SERVER CONFIGS ---------------------------------

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
aws s3 cp ${MY_FGVPN_S3_DEPLOY_URL}/conf/${MY_FGVPN_CONFIG_NAME}/${OVPNSRVNAME}-srv.crt ${OVPNSRVPATH}/
aws s3 cp ${MY_FGVPN_S3_DEPLOY_URL}/conf/${MY_FGVPN_CONFIG_NAME}/${OVPNSRVNAME}-srv.csr ${OVPNSRVPATH}/
aws s3 cp ${MY_FGVPN_S3_DEPLOY_URL}/conf/${MY_FGVPN_CONFIG_NAME}/${OVPNSRVNAME}-srv.key ${OVPNSRVPATH}/

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
cert ${OVPNSRVPATH}/${OVPNSRVNAME}-srv.crt
key ${OVPNSRVPATH}/${OVPNSRVNAME}-srv.key
; topology subnet
server ${MY_FGVPN_VPNSubNet:0:${#MY_FGVPN_VPNSubNet}-3} 255.255.255.0
ifconfig-pool-persist ${OVPNSRVPATH}/${OVPNSRVNAME}-ipp.txt
client-config-dir ${OVPNSRVPATH}/${OVPNSRVNAME}-ccd
# CIDR for this VPN
push "route ${MY_FGVPN_VPNSubNet:0:${#MY_FGVPN_VPNSubNet}-3} 255.255.255.0"
# CIDR for this server private IP in the VPC (not the full VPC since it is not needed for FG)
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

