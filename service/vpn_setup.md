# BASIC INFO 
* LOCATION: NORTH YORK, ON, CA
* PROTOCAL: L2TP/IPSEC
* NETWORK: 1500M/1000M Fibre
* SERVER: redportal.sideai.com
* USERNAME: *your-user
* PASSWORD: *your-passwd
* SECRET/KEY: *your-key

# GUILD FOR ANDROID USER 

[Settings] >> [More Connections] >> [VPN] >> [Add VPN Network]

## Edit VPN network 
* Name: REDPORTAL
* Type: L2TP/IPSec PSK
* Server: redportal.sideai.com
* L2TP secret: (not used)
* IPSec identifier: (not used)
* IPSec pre-shared key: *your-key

## Connect to REDPORTAL
* Username: *your-user
* Password: *your-passwd
* [ON] Save account infomation
* [OFF] Always-on VPN

# GUILD FOR IOS USER 

[Settings] >> [General] >>[VPN & Device Management] >> [VPN] >> [Add VPN Configration]

## Add Configuration 
* Type: L2TP
* Description: REDPORTAL
* Server: redportal.sideai.com
* Account: *your-user
* RSA SecurlID: OFF
* Password: *your-passwd
* Secret: *your-key
* Send All Traffic: ON

# GUILD FOR WIN10 USER

[PRESS "WIN KEY"] >> [SEARCH "VPN Settings"] >> [Add a VPN Connection]

## Add a VPN Connection
* VPN Provider: Windows (built-in)
* Connection name: REDPORTAL
* Server name or address: redportal.sideai.com
* VPN type: L2TP/IPsec with pre-shared key
* Pre-shared key: *your-key
* Type of sign-in info: User name and password
* User name: *your-user
* Password: *your-passwd
* [ON] Remember my sign-in info

# TROUBLE SHOOTING 

## 1. VPN server not respond
Check is that if the network is blocked by the firewall. 
```
ping redportal.sideai.com 
```
OR OPEN
```
redportal.sideai.com:12080
```

## 2. WIN10 can not connect
Fix the Registry setting by [open notepad] >> [copy & paste the content below] >> [save as "win_vpn_fix.reg"] >> [click to run "win_vpn_fix.reg"] >> [reboot your pc]
```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\PolicyAgent]
"AssumeUDPEncapsulationContextOnSendRule"=dword:00000002
```