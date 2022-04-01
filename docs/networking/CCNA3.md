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
```
R3(config)# access-list 1 remark Allow R1 LANs Access
R3(config)# access-list 1 permit 192.168.10.0 0.0.0.255
R3(config)# access-list 1 permit 192.168.20.0 0.0.0.255
R3(config)# access-list 1 deny any
---
R3(config)# interface g0/0/0
R3(config-if)# ip access-group 1 out
```
