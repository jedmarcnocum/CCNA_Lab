useful commands

* do sh ip int br - show interface brief details like ip, up/down
* do sh int status - show interface status like status, vlan trunk/routed speed, duplex
* do sh int tr - show allowed vlan trunks 
* do sh vl br - show vlan configs, names, status stc
* switchport trunk encapsulation dot1q - older cisco if isl and dot1q is option



1. Set and compute VLSM base on base ip address and needed hosts per VLAN
2. Set devices in Lab
3. add straight-through cables
4. Rename devices first (hostname switch, routers etc)



PC hosts config:

1. Set default gateway to last usable addr of the VLSM config per vlan
2. Set the ip address to 1st usable addr of the VLSM config per vlan
3. Set the subnet mask based on the VLSM config per vlan



SW1 config:

1. Create vlans, vlan 10, vlan 20, vlan 30 etc and rename if needed
2. Set interface connected to SW2 as trunk, set allowed vlans 10,20,30
3. Set interface connected on each host as access, then set vlans via sw access vlan #
4. check status and config using useful commands



SW2 config

1. run ip routing - this enables the switch to become layer 3
2. Create vlans, vlan 10, vlan 20, vlan 30 etc and rename if needed
3. Set interface connected to SW1 as trunk, set allowed vlans 10,20,30
4. Set SVI for each vlan (interface vlan #, ip address x.x.x.x)
5. enter the interface connected to router, run no switchport
6. Set the ip address of the interface connected to the router P2P (1st usable addr)
7. Set the gateway of last resort as the router's ip address connected to SW2 interface (last useable addr)



R1 config:

1. Set the ip address of the interface connected to the switch P2P (last usable addr)
2. Add static routes to VLAN networks via SW2
3. Set the gateway of last resort to internet cloud
4. 
