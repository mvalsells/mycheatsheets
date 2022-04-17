# CCNA 2

## Basic

```
R1(config)# no ip domain-lookup
R1(config)# enable secret class
R1(config)# service password-encryption
R1(config)# banner motd $Access restricted$
R1(config)# clock set 12:58:20 JAN 2022
---
R1(config)# line vty 0 9999
R1(config-line)# password cisco
R1(config-line)# login
---
R1(config)# line console 0
R1(config-line)# password cisco
R1(config-line)# login
```

## M1 - Basic device configuration
##### SSH
```
R1(config)# ip domain-name cisco.com
R1(config)# crypto key generate rsa
R1(config)# user admin secret ccna
R1(config)# line vty 0 15
R1(config-line)# transport input ssh
R1(config-line)# login local
```

## M2 - InterVLAN routing

##### Subinterfaces
```
R1(config)# int g0/0.10
R1(config-subif)# encapsulation dot1Q 10
R1(config-subif)# encapsulation dot1Q 11 native
R1(config-subif)# ip address 172.17.10.1 255.255.255.0
```

## M3 - VLANs
##### VLANs and switchport basics
```
S1(config)# vlan 100
S1(config-vlan)# name Operations
---
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk native vlan 1000
S1(config-if)# switchport trunk allowed vlan 100,200,1000
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 10
---
S1# delete vlan.dat
```
##### DTP

|                   | Dynamic auto  | Dynamic desirable | Trunk   | Access
|-------------------|---------------|-------------------|---------|--------
| Dynamic Auto      | Access        | Trunk             | Trunk   | Access
| Dynamic desirable | Trunk         | Trunk             | Trunk   | Access
| Trunk             | Trunk         | Trunk             | Trunk   | Limited
| Access            | Access        | Access            | Limited | Access

```
S1(config-if)# switchport mode dynamic auto
S1(config-if)# switchport mode dynamic desirable
S1(config-if)# switchport mode trunk
S1(config-if)# switchport mode access
! DTP can be disable with nonegotiate
S1(config-if)# switchport nonegotiate
```
##### Shows
```
S1# show interfaces trunk
S1# show vlan
S1# show dtp interface fa0/1
```

## M6 - EtherChannel

##### Modes
- **active** -> Enable LACP unconditionally
- **auto** -> Enable PAgP only if a PAgP device is detected
- **desirable** -> Enable PAgP unconditionally
- **on** -> Enable Etherchannel without any negotiation protocol
- **passive** -> Enable LACP only if a LACP device is detected

|           | active  | passive | desirable | auto
|-----------|---------|---------|-----------|------
| active    | LACP    | LACP    | -         | -
| passive   | LACP    | -       | -         | -
| desirable | -       | -       | PAgP      | PAgP
| auto      | -       | -       | PAgP      | -

##### PAgP
```
S1(config)# interface range f0/21 – 22
S1(config-if-range)# shutdown
S1(config-if-range)# channel-group 1 mode desirable
S1(config-if-range)# no shutdown
```

##### LACP
```
S1(config)# interface range g0/1 - 2
S1(config-if-range)# shutdown
S1(config-if-range)# channel-group 2 mode active
S1(config-if-range)# no shutdown
```


## M7 - DHCPv4
##### DHCP server
```
R2(config)# ip dhcp excluded-address 192.168.10.1 192.168.10.10
R2(config)# ip dhcp pool R1-LAN
R2(dhcp-config)# network 192.168.10.0 255.255.255.0
R2(dhcp-config)# default-router 192.168.10.1
R2(dhcp-config)# dns-server 192.168.20.254
```
##### DHCP relay
```
R1(config)# interface g0/0
R1(config-if)# ip helper-address 10.1.1.2
```
##### DHCP Client
```
R2(config)# interface g0/1
R2(config-if)# ip address dhcp
```
##### Shows
```
R2# show ip dhcp binding
R2# show ip dhcp pool
R2# show ip dhcp server statistics
```
## M9 - FHRP (First Hop Reduncy Protocol)
##### HSRP (Hot Standby Router Protocol)

```
R1(config-if)# standby version 2
R1(config-if)# standby 1 ip 192.168.1.254
! Default priority is 100, higher -> active Router
R1(config-if)# standby 1 priority 150
! Preempt makes this router active after recovery
R1(config-if)# standby 1 preempt
---
R1# show standby
```

## M11 - Switch security
##### Port-security
```
S1(config-if)# switchport port-security
S1(config-if)# switchport port-security maximum 1
S1(config-if)# switchport port-security mac-address sticky
S1(config-if)# switchport port-security violation restrict
```
##### Shows
```
S1# show port-security
S1# show port-security address
S1# show port-security interface f0/2
```
