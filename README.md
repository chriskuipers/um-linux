# Linux tricks for Maastricht University
This repository contains commands and configs I found useful regarding the use of
University services. Some of these may be distro-specific so your mileage may vary.

## Eduroam
Below is a config for `netctl` that works with Eduroam.

`/etc/netctl/wlp3s0-eduroam`
```
Description='eduroam'
Connection=wireless
Interface=wlp3s0
Security=wpa-configsection
ESSID=eduroam
TimeoutWPA=10
IP=dhcp
WPAConfigSection=(
	'ssid="eduroam"'
	'proto=WPA2'
	'key_mgmt=WPA-EAP'
	'eap=PEAP'
	'phase2="auth=MSCHAPV2"'
	'identity="<student id>@unimaas.nl"'
	'password="<password>"'
)
```

## VPN
[OpenConnect](https://wiki.archlinux.org/index.php/OpenConnect) may be used to
connect to the University VPN, for instance to gain access to journals. 

Installation on Arch:
```
sudo pacman -S openconnect
```
Installation on Debian/Ubuntu/Mint:
```
sudo apt-get install openconnect
```

Having installed it simply run:
```
sudo openconnect vpn.maastrichtuniversity.nl -u <student id>
```
And you're in!

Alternatively, you may use [EZproxy](https://www.oclc.org/nl/ezproxy.html) as
described [here](https://linuxunimaas.blogspot.com/2013/11/reading-literature-from-home-with-your.html).

## File service
The University file service is accessible with
[Samba](https://wiki.archlinux.org/index.php/samba). Note that in order to connect to
the file services you need to be connected to the UM network, i.e. either be
physically present and connected to University Eduroam or to the VPN as described
above.

To get a list of network drives run:
```
smbclient -L unimaas.nl -U <student id>
```

In order to mount your private drive directory in `/media/unimaas/userdata` run:
```
# Create a mountpoint
sudo mkdir -p /media/unimaas/userdata
# Mount it
sudo mount -t cifs //mfs.maastrichtuniversity.nl/users/students/<student id>/data /media/unimaas/userdata -o username=<student id>@unimaas.nl,vers=2.0,uid=$(id -u),gid=$(id -g)
```

More information may be found in the [ICTS Manual - UM drive
   mappings](https://kb-icts-maastrichtuniversity-nl.ezproxy.ub.unimaas.nl/display/ISM/Manual+File+Service+-+Mapping+UM+network+drives+in+Windows).

# Useful resources
 - [Linux @ Maastricht University blog](https://linuxunimaas.blogspot.com/)
 - [ICTS Manual by
   proxy](https://kb-icts-maastrichtuniversity-nl.ezproxy.ub.unimaas.nl/display/ISM/ICTS+Servicedesk+Manuals)
