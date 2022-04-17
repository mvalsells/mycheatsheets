# CCNA 3

## M2 - Single-area OSPF
##### Global
```
! Where 10 is the ospf process-id
R1(config)# router ospf 10
R1(config-router)# router-id 1.1.1.1
R1(config-router)# network 192.168.10.0 0.0.0.255 area 0
R1(config-router)# default-information originate
R1# clear ip ospf process
```
##### Interface
```
R1(config)# interface s0/0/0
R1(config-if)# ip ospf 10 area 0
R1(config-if)# ip ospf priority 200
R1(config-if)# ip ospf hello-interval 15
R1(config-if)# ip ospf dead-interval 60
R1(config-if)# bandwidth 64 !in kbps
```
##### Shows
```
R1# show ip ospf
R1# show ip ospf neighbor
R1# show ip ospf interface
R1# show ip route ospf
R1# show ip protocols
```

## M5 - ACLs for IPv4
#### Standard
##### Create ACLs rules
```
R1(config)# access-list 1 remark Allow R1 LANs Access
R1(config)# access-list 1 permit 192.168.10.0 0.0.0.255
R1(config)# access-list 1 permit 192.168.20.0 0.0.0.255
R1(config)# access-list 1 deny any
```
##### Modify ACLs rules
```
R1#(config)# ip access-list standard BRANCH-OFFICE-POLICY
R1(config-std-nacl)# 30 permit 209.165.200.224 0.0.0.31
R1(config-std-nacl)# 40 deny any
```

#### Extended
##### Create ACLs rules
```
! Option 1
R1(config)# access-list 89 permit tcp 172.2.34.4 0.0.0.31 host 10.22.4.62 eq ftp
! Option 2
R1(config)# ip access-list extended HTTP_ONLY
R1(config-ext-nacl)# permit tcp 172.22.34.96 0.0.0.15 host 172.22.34.62 eq www
R1(config-ext-nacl)# permit icmp 172.22.34.96 0.0.0.15 host 172.22.34.62
```
##### Modify ACLs rules
```
R1(config)# ip access-list extended HTTP_ONLY
R1(config-ext-nacl)# 25 permit tcp 172.22.34.96 0.0.0.15 host 172.22.34.62 eq 53
R1(config-ext-nacl)# 4 permit icmp 172.22.34.96 0.0.0.15 host 172.22.34.62
```
#### Apply ACL to an interface
```
R1(config)# interface g0/0/0
R1(config-if)# ip access-group 1 out
R1(config-if)# ip access-group BRANCH-OFFICE-POLICY in
```
#### Shows
```
R1# show access-lists
R1# show access-lists 34
R1# show access-lists LAN-ACL
R1# show running-config | begin access-list
R1# show ip access-lists
!To see which ACL is applied to the interface
R1# show ip interface g0/0/0

```

## M6 - NAT
##### Static
```
R1(config)# ip nat inside source static 172.16.16.1 64.100.50.1
R1(config)# interface g0/0
R1(config-if)# ip nat inside
R1(config)#interface s0/0/0
R1(config-if)#ip nat outside
```

##### Port Address Translation (PAT) / NAT overload
```
! Create ACL for insde IPs
R1(config)# access-list 1 permit 172.16.0.0 0.0.255.255

! Option 1 - Create IP pool for outside IPs
R1(config)# ip nat pool NAME 209.15.20.233 209.15.20.234 netmask 255.255.255.252
R1(config)# ip nat inside source list 1 pool NAME overload

! Option 2 - Set out interface instead of a pool
R1(config)# ip nat inside source list 2 interface s0/1/0 overload

```
##### NAT interfaces
```
R1(config)# interface s0/1/0
R1(config-if)# ip nat outside
R1(config-if)# interface g0/0/0
R1(config-if)# ip nat inside
```
##### Shows
```
R1# show run | include nat
R1# show ip nat translations
R1# show ip nat statistics
```
## M10 - Network Management
#### Cisco Discovery Protocol (CDP)
##### Enable/disable
```
R1(config)# cdp run
R1(config)# interface s0/0/1
R1(config-if)# no cdp enable
R1(config-if)# exit
```
##### Shows
```
R1# show cdp
R1# show cdp neighbors
R1# show cdp neighbors detail
```
#### Link Layer Discovery Protocol (LLDP)
##### Enable/disable
```
R1(config)# lldp run
R1(config)# int g0/0
R1(config-if)# no lldp transmit
R1(config-if)# exit
```
##### Shows
```
R1# show lldp
R1# show lldp neighbors
R1# show cdp neighbors detail
```
