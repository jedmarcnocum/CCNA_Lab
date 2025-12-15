useful commands

* do sh ip int br - show interface brief details like ip, up/down
* do sh int status - show interface status like status, vlan trunk/routed speed, duplex
* do sh int tr - show allowed vlan trunks 
* do sh vl br - show vlan configs, names, status stc
* switchport trunk encapsulation dot1q - older cisco if isl and dot1q is option



1. Set VLSM
2. Set devices in Lab
3. add straight-through cables
4. Rename devices first (hostname switch, routers etc)



SW1 config:

1. Set interface connected to SW2 as trunk, set allowed vlans 10,20,30
2. Set interface connected on each host as access, then set vlans via sw access vlan #
3. check status and config using useful commands



SW2 config

1. Set interface connected to SW1 as trunk, set allowed vlans 10,20,30
2. Set SVI for each vlan (interface vlan #, ip address x.x.x.x)
3. Set the ip address of the interface connected to the router P2P (1st usable addr)
4. Set the gateway of last resort as the router's ip address connected to SW2 interface (last useable addr)



R1 config:

1. Set the ip address of the interface connected to the switch P2P (last usable addr)
2. Set the gateway of last resort as the SW2 ip address connected to this router interface (1st useable addr)



1. 
2. Set SW1 (layer 2) interface connected to SW set sw mode access, sw access vlan #
3. Once trunk is uplinked to SW2, set the allowed vlans on that trunk via sw trunk allowed vlan #
4. Move to SW2 CLI, set SVI's for each vlan via interface vlan #, then set the ip address (default gateway per vlan) and subnet mask
5. Set the interface connected from SW1 to SW2 (layer 3) as sw tr enc dot1q, sw mode trunk, then set the allowed vlans sw trunk allowed vlan #
6. &nbsp;set the P2P ip address of SW2 going to R1
7. once set, exit and set gateway of last resort via ip route 0.0.0.0 0.0.0.0 ip\_address\_of\_r1
8. move to R1, set the ip address interface connected to SW2, remember to do no shut as routers are down by default
9. set gateway of last resort via ip route 0.0.0.0 0.0.0.0 ip\_address\_of\_sw2 so R1 can reach other vlans
