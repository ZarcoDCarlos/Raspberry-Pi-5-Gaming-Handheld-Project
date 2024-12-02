#  Instructions for setting a static IP address in RecalBox

## Table of Contents
A. Why set a Static IP Address?

B. Prerequisites

C. Steps

D. Conclusion

## A. Why set a Static IP Address?
  Setting a static IP address makes it easier to SSH into your Raspberry Pi running RecalBox OS.
  This allows quick access to the command line interface for editing configuration files or changing
  other key system settings. 

## B. Prerequisites
- Raspberry Pi with a working image of RecalBox OS installed
- RecalBox should be connected to your network
- Program like Putty installed on your computer to SSH into your Raspberry Pi

## C. Steps

### SSH into your Pi
To enable a static IP Address on Recalbox first SSH into the Raspberry Pi 
using the IP address from Recalbox's Network settings page.

The default username is: 'root'
The default password is: 'recalboxroot'

Recalbox differs from other Linux distributions because it offers default root access. This means that the 'sudo' command 
cannot be used as the user is already a superuser or admin. 

### Network Configuration Details
Now that you have SSH'd into the Pi, find the following Network configuration details:
- **IP Address**: A unique IP address within the range of your network.
- [**Netmask**](https://wiki.teltonika-networks.com/view/What_is_a_Netmask%3F#:~:text=A%20Netmask%20is%20a%2032,is%20the%20assigned%20network%20address.): A number that separates your IP Address into two parts: the network and the hosts on that network.
- **Gateway**: IP address of your router. This address serves as an access point or gateway to other networks.
- [**Nameservers**](https://kinsta.com/knowledgebase/what-is-a-nameserver/): These are the IP addresses of the DNS servers that you want to use.
                Nameserver contains domain names and their corresponding IP address.

To locate these Config Details do the following:

For the **Netmask** type:
```
ifconfig
```
Short for 'interface configuration', this command is for configuring network interfaces. It outputs network information.
Particularly the Netmask under the wlan0 section.


For the **Gateway**, type:
```
ip route | grep default
```
This command shows the default gateway which is the router's IP address.


To locate the **Nameservers**, type:
```
cat /etc/resolv.conf
```
This command displays the contents of the DNS configuration file. These are the servers your Pi uses to translate domains
to IP addresses. 


With all of this information, we can now edit the Recalbox Configuration file and adjust the network parameters with the
information that is found above.
To access the file type:
```
nano /recalbox/share/system/recalbox.conf
```
Find these lines in the config file:
```
## Wifi - static IP
## if you want a static IP address, you must set all 4 values (ip, netmask, gateway and nameservers)
## if any value is missing or all lines are commented out, it will fall back to the
## default of DHCP. For nameservers, you must set at least 1 nameserver.
;wifi.ip=manual ip address (ex: 192.168.1.2)
;wifi.netmask=network mask (ex: 255.255.255.0)
;wifi.gateway=ip address of gateway (ex: 192.168.1.1)
;wifi.nameservers=ip address of domain name servers, space separated (ex: 192.168.1.1 8.8.8.8)
```

For example:
```
wifi.ip=192.168.1.1
wifi.netmask=255.255.255.0
wifi.gateway=192.168.1.254
wifi.nameservers=8.8.8.8 
```
Note the Gateway and IP address are different. The Gateway is the router's IP address, the IP address section is almost 
identical. This is the IP Address that the PI will use. I used the IP address from Recalbox's Network settings page here.
This will be the **Static IP Address** used for SSH in the future if done successfully. **Make note of it.**

Note that for the Nameservers section, you can add multiple. I added the one found using the steps above as well as '8.8.8.8' since that is a public nameserver from Google's DNS service. 

Type:
```
reboot
```
Hit enter, and check to see either in Recalbox Network settings if the newly assigned Static IP Address stays the same,
or by successfully SSH'ing into the PI by using the IP Address. 

## D. Conclusion
Now, you should be able to log in to your Raspberry Pi with Recalbox OS easily via SSH and the Static IP Address that has been configured. Happy Gaming!
