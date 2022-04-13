## CCNA 3

### M2 - Single-area OSPF configuration
```
! Where 10 is the ospf process-id
R2(config)# router ospf 10
R2(config-router)# router-id 1.1.1.1
R2(config-router)# network 192.168.10.0 0.0.0.255 area 0
R2(config-router)# default-information originate
---
R2(config)# interface s0/0/0
R2(config-if)# ip ospf 10 area 0
R2(config-if)# ip ospf priority 200
R2(config-if)# ip ospf hello-interval 15
R2(config-if)# ip ospf dead-interval 60
R2(config-if)# bandwidth 64 !in kbps
---
R2# clear ip ospf process
R2# show ip ospf
R2# show ip ospf neighbor
R2# show ip ospf interface
show ip route ospf
show ip protocols
```

### M5 - ACLs for IPv4 config
##### Standard
```
R3(config)# access-list 1 remark Allow R1 LANs Access
R3(config)# access-list 1 permit 192.168.10.0 0.0.0.255
R3(config)# access-list 1 permit 192.168.20.0 0.0.0.255
R3(config)# access-list 1 deny any
! Modifying ACLs
R1#(config)# ip access-list standard BRANCH-OFFICE-POLICY
R1(config-std-nacl)# 30 permit 209.165.200.224 0.0.0.31
R1(config-std-nacl)# 40 deny any
---
R3(config)# interface g0/0/0
R3(config-if)# ip access-group 1 out
---
R3# show access-lists
R3# show access-lists 34
R3# show access-lists LAN-ACL
!To see which ACL is applied to the interface
R3# show ip interface g0/0/0
```
##### Extended
```
! Option 1
R1(config)# access-list 100 permit tcp 172.22.34.64 0.0.0.31 host 172.22.34.62 eq ftp
! Option 2
R1(config)#ip access-list extended HTTP_ONLY
R1(config-ext-nacl)#permit tcp 172.22.34.96 0.0.0.15 host  172.22.34.62 eq www
R1(config-ext-nacl)#permit icmp 172.22.34.96 0.0.0.15 host  172.22.34.62
----
R1(config)#interface gigabitEthernet 0/0
R1(config-if)#ip access-group 100 in
R1(config-if)# ip access-group HTTP_ONLY in
---
RT1# show running-config | begin access-list
RT1# show ip access-lists
```
