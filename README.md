## 1 What is Remote Syslog Cloud Connector?:
Remote Syslog Cloud Connector is a modified version of OpenVPN [road warrior]. More info @ section 1.1 and 1.2. The following changes are made:
1) Default gateway ip4/ipv6 is not routed through the VPN, only the default 192.168.30.0/24 will be routed to the clients.
2) Only one host route will be created, this allows you to control the server or services on a private address. This will be done on the lo adapter.
3) Default ip adresses are changed to 192.168.30.0/24
4) DNS is disabled
5) Client-to-Client traffic is allowed
6) Static ip addresses can be assigned

With these settings you are able to create a secure network for logging only.

By enrolling this unit, clients/core layer can connect with an encrypted connection. If you place this concentrator as central gateway for logging then this network (default: 192.168.30.0 255.255.255.0) must be advertised within the infrastructure netwerk.

### 1.1 Original message openvpn-install (https://git.io/vpn):
OpenVPN [road warrior](http://en.wikipedia.org/wiki/Road_warrior_%28computing%29) installer for Ubuntu, Debian, CentOS and Fedora.

This script will let you set up your own VPN server in no more than a minute, even if you haven't used OpenVPN before. It has been designed to be as unobtrusive and universal as possible.

### 1.2 Donations for the original creaters of this openVPN installation:
If you want to show your appreciation for the original creators of the unmodified script, you can make a donation via [PayPal](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=VBAYDL34Z7J6L) or [cryptocurrency](https://pastebin.com/raw/M2JJpQpC). Thanks!

## 2 Installation VPN/Cloud connector for Remote Syslog:
Default installation steps:
```
git clone https://github.com/tslenter/Remote-Syslog-Cloud-Connector
cd Remote-Syslog-Cloud-Connector
chmod +x openvpn-install.sh
./openvpn-install.sh
```

Select the following options:
```
Welcome to this OpenVPN Remote Syslog Cloud Connector!

Which protocol should OpenVPN use?
   1) UDP (recommended)
   2) TCP
Protocol [1]: 1

What port should OpenVPN listen to?
Port [1194]:

Enter a unused private address for this host: (Select a private IP address what is not in use on all sites)
IP: 192.168.40.1

Enter a name for the first client:
Name [client]: gateway

OpenVPN installation is ready to begin.
```

Route internal traffic through the Remote Syslog Cloud connector:
```
Install:
sudo apt update && sudo apt upgrade -y 
sudo apt install openvpn

Edit:
/etc/sysctl.conf
Config:
net.ipv4.ip_forward = 1

Auto start:
crontab -e
@reboot /usr/sbin/openvpn --config /root/OpenVPN/openvpn9001.remotesyslog.com.ovpn
@reboot /usr/sbin/iptables -t nat -A POSTROUTING -o tun0 -j MASQUERADE
@reboot /usr/sbin/iptables -A FORWARD -i tun0 -o ens160 -m state --state RELATED,ESTABLISHED -j ACCEPT
@reboot /usr/sbin/iptables -A FORWARD -i ens160 -o tun0 -j ACCEPT

Info:
Don't forget to create the static route within the infrastructure to 192.168.30.0/24 with the next hop with the local ip of the server.
```

If you like to add a static IP to a client use the following guide:
```
Run the openvpn-install.sh script and add a client:
OpenVPN is already installed.

Select an option:
   1) Add a new client
   2) Revoke an existing client
   3) Remove OpenVPN
   4) Exit
Option: 1

Provide a name for the client:
Name: Remote_Syslog_Node

After the client is added run:
echo "ifconfig-push 192.168.30.x 255.255.255.0" > /etc/openvpn/ccd/Remote_Syslog_Node

x = a free address within the OpenVPN subnet
Remote_Syslog_Node = the name of the client, provided within the openvpn-install script
```

## 3. Donation

Crypto:

```
XRP/Ripple: rHdkpJr3qYqBYY3y3S9ZMr4cFGpgP1eM6B
BTC/Bitcoin: 1JVmexqGBQyGv9fVkSynHapi2U6ZCyjTUJ
LTC/Litecoin Segwit: MAH8ATCK6X7biiTQrW7jUZ6L9eg1YBo5qS
ETH/Ethereum: 0xd617391076F9bEa628f657606DEAB7a189199AF5
```
PayPal:

[![paypal](https://www.paypalobjects.com/en_US/NL/i/btn/btn_donateCC_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=KQKRPDQYHYR7W&currency_code=EUR&source=url)

## 4. Help

To improve the code and functions we like to have you help. Send your idea or code to: info@remotesyslog.com or create a pull request. We will review it and add it to this project.

## 5. License
"Remote Syslog Cloud Connector" is a free application what can be used to secure connections via OpenVPN.

Copyright (C) 2020 Tom Slenter

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.

You should have received a copy of the GNU General Public License along with this program. If not, see http://www.gnu.org/licenses/.

For more information contact the author:

Name author: Tom Slenter

E-mail: info@remotesyslog.com
