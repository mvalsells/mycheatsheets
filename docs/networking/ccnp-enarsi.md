# CCNP - ENARSI

## CH2 - EIGRP IPv4

Metric = (K1\*BW+K2\*BW/(256-Load)+K3\*Delay)\*K5/(K4+Realiability)

#### Routing process
##### Classic
```
! 27 is the AS number
R1(config)# router eigrp 27
R1(config-router)# eigrp router-id 2.2.2.2
R1(config-router)# network 10.0.0.0 255.255.224.0
R1(config-router)# passive-interface default
R1(config-router)# no passive-interface g0/0/0
R1# clear ip eigrp process
```
##### Named
```
R1(config)# router eigrp BASIC-EIGRP-LAB
R1(config-router)# address-family ipv4 unicast autonomous-system 27
R1(config-router-af)# eigrp router-id 1.1.1.1
R1(config-router-af)# network 10.0.12.0 255.255.255.0
R1(config-router-af)# af-interface default
R1(config-router-af-interface)# passive-interface
R1(config-router-af)# af-interface g0/0/1.2
R1(config-router-af-interface)# no passive-interface
R1# clear ip eigrp process
```
#### Authentication
##### MD5
```
! Create key-chain with password $3cre7!!
R1(config)# key chain EIGRP-AUTHEN-KEY
R1(config-keychain)# key 1
R1(config-keychain-key)# key-string $3cre7!!
R1(config-keychain-key)# end

! Use key-chain in classic EIGRP
R1(config)# interface g0/0/0
R1(config-if)# ip authentication key-chain eigrp 27 EIGRP-AUTHEN-KEY
R1(config-if)# ip authentication mode eigrp 27 md5

! Use key-chain in named EIGRP
R1(config)# router eigrp BASIC-EIGRP-LAB
R1(config-router)# address-family ipv4 unicast autonomous-system 27
R1(config-router-af)# af-interface g0/0/0
R1(config-router-af-interface)# authentication key-chain EIGRP-AUTHEN-KEY
R1(config-router-af-interface)# authentication mode md5
```
##### SHA256
```
R1(config)# router eigrp BASIC-EIGRP-LAB
R1(config-router)# address-family ipv4 unicast autonomous-system 27
R1(config-router-af)# af-interface g0/0/1.1
R1(config-router-af-interface)# authentication mode hmac-sha-256 $3cre7!!
R1(config-router-af-interface)# end
```

#### Shows
```
R1# show ip route eigrp
R1# show ip protocols | section eigrp
R1# show ip eigrp topology 192.168.3.0/24
R1# show ip eigrp topology all-links
R1# show ip eigrp interfaces
R1# show ip eigrp interfaces gi0/1 detail
R1# show ip eigrp neighbors
R1# show ip eigrp events
```
## CH3 - Advanced EIGRP IPv4

#### Timers
```
! 27 is the AS number
R1(config)# interface g0/0/0
R1(config-if)# ip hello-interval eigrp 27 10
R1(config-if)# ip hold-time eigrp 27 30

R1# show ip eigrp interfaces detail
```

#### Route manipulation
```
! 27 is the AS number
! Summary
R1(config)# interface g0/0/0
R1(config-if)# ip summary-address eigrp 27 192.168.0.0 255.255.252.0

! Stub
R1(config)# router eigrp 27
R1(config-router)# eigrp stub

! Distribute list
R1(config)# ip access-list standard EIGRP-FILTER
R1(config-std-nacl)# deny 10.12.0.0 0.0.255.255
R1(config-std-nacl)# permit any
R1(config-std-nacl)# exit
R1(config)# router eigrp 27
R1(config-router)# distribute-list EIGRP-FILTER out g0/0/1
R1(config-router)# end

! Shows
R1# show ip route eigrp
R1# show ip eigrp neighbor detail
```


## CH5 - EIGRP IPv6

Metric = (K1\*BW+K2\*BW/(256-Load)+K3\*Delay)\*K5/(K4+Realiability)

#### Routing process
##### Classic
```
! 43 is the AS number
R1(config)# ipv6 router eigrp 43
R1(config-router)# eigrp router-id 2.2.2.2
R1(config-router)# passive-interface default
R1(config-router)# no passive-interface g0/0/0
R1(config)# interface g0/0/0
R1(config-if)# ipv6 eigrp 43
```
##### Named
```
R1(config)# router eigrp EIGRP_IPV6
R1(config-router)# address-family ipv6 unicast autonomous-system 43
R1(config-router-af)# eigrp router-id 1.1.1.1
R1(config-router-af)# af-interface default
R1(config-router-af-interface)# passive-interface
R1(config-router-af)# af-interface g0/0/1.2
R1(config-router-af-interface)# no passive-interface
R1(config-router-af-interface)# summary-address 2001:db8:abcd::/56
```
!!! note
    In named-mode configuration, EIGRP for IPv6 is automatically enabled on all interfaces that are configured with an IPv6 address.
#### Authentication
##### MD5
```
! Create key-chain with password $3cre7!!
R1(config)# key chain EIGRP_IPV6
R1(config-keychain)# key 1
R1(config-keychain-key)# key-string $3cre7!!
R1(config-keychain-key)# end

! Use key-chain in classic EIGRP
R1(config)# interface g0/0/0
R1(config-if)# ip authentication key-chain eigrp 43 EIGRP-AUTHEN-KEY
R1(config-if)# ip authentication mode eigrp 43 md5

! Use key-chain in named EIGRP
R1(config)# router eigrp EIGRP_IPV6
R1(config-router)# address-family ipv6 unicast autonomous-system 43
R1(config-router-af)# af-interface g0/0/0
R1(config-router-af-interface)# authentication key-chain EIGRP-AUTHEN-KEY
R1(config-router-af-interface)# authentication mode md5
```
##### SHA256
```
R1(config)# router eigrp BASIC-EIGRP-LAB
R1(config-router)# address-family ipv6 unicast autonomous-system 43
R1(config-router-af)# af-interface g0/0/1.1
R1(config-router-af-interface)# authentication mode hmac-sha-256 $3cre7!!
R1(config-router-af-interface)# end
```

#### Shows
```
R1# show ipv6 route eigrp
R1# show ipv6 protocols | section eigrp
R1# show ipv6 eigrp topology 192.168.3.0/24
R1# show ipb6 eigrp topology all-links
R1# show ipv6 eigrp interfaces
R1# show ipv6 eigrp interfaces gi0/1 detail
R1# show ipv6 eigrp neighbors
```
## CH6 and CH7 - OSPFv2
#### Routing process
##### Router method
```
! 123 is the process id, doesn't need to match between devices
R1(config)# router ospf 123
R1(config-router)# router-id 1.1.1.1
! Advertising each network by interface IP
R1(config-router)# network 10.10.0.2 0.0.0.0 area 0
! Advertising multiple/all networks with wildcards
R1(config-router)# network 10.10.0.0 0.0.255.255 area 1
R1(config-router)# network 0.0.0.0 255.255.255.255 area 0
R1(config-router)# default-information originate
R1# clear ip ospf process
```

##### Interface method
```
R1(config)# router ospf 123
R1(config-router)# router-id 1.1.1.1
R1(config)# interface loopback 1
R1(config-if)# ip ospf network point-to-point ! Only on loopbacks
R1(config-if)# ip ospf 123 area 0
R1(config-if)# exit
R1# clear ip ospf process
```

#### OSPF tuning
```
! OSPF general parameters
D1(config)# router ospf 123
D1(config-router)# passive-interface default
D1(config-router)# no passive-interface g1/0/5
R1(config-router)# auto-cost reference-bandwidth 10000
! Timers
R1(config)# interface g0/0/1
R1(config-if)# ip ospf hello-interval 5
R1(config-if)# ip ospf dead-interval 20
! DR and BDR election
D2(config)# interface g0/0/1
D2(config-if)# ip ospf priority 0   ! Never become DR or BDR candidate
R1(config-if)# ip ospf priority 255 ! Always become DR or BDR candidate
```
!!! warning
    By default the OSPF cost reference is equivelent to a 100Mbps. This means that a 100Mbps or higher speed links will have the same cost value of 1 by default.
    
    It is recomended to set the reference bandwith to 10000 (10Gbps), but this will cause that links slower than 1.5Mbps will have the same cost of 65.535.


#### Shows
```
R1# show ip protocols
R1# show ip route ospf
R1# show ip ospf interface brief
R1# show ip ospf interface g0/0/1
R1# show ip ospf neighbor
R1# show ip ospf neighbor detail
R1# show ip ospf database
R1# show ip ospf database router        ! Type 1 LSA
R1# show ip ospf database network       ! Type 2 LSA
R1# show ip ospf database summary       ! Type 3 LSA
R1# show ip ospf database asbr-summary  ! Type 4 LSA
R1# show ip ospf database external      ! Type 5 LSA
```

## CH9 - OSPFv3
#### Routing process

##### Address family method (new devices)
```
! 123 is the process id, doesn't need to match between devices
R1(config)# ipv6 router ospf 123
R1(config-router)# router-id 1.1.1.1            ! For all AF
R1(config-router)# address-family ipv4 unicast
R1(config-router-af)# router-id 1.1.1.1         ! For a specific AF
R1(config-router-af)# default-information originate
R1(config-router)# address-family ipv6 unicast
R1(config-router-af)# router-id 1.1.1.1         ! For a specfic AF
R1(config-router-af)# default-information originate
R1(config)# interface g0/0/0
R1(config-if)# ospfv3 123 ipv4 area 0
R1(config-if)# ospfv3 123 ipv6 area 0
```

##### Interface method (old devices)
```
! 123 is the process id, doesn't need to match between devices
R1(config)# ipv6 router ospf 123
R1(config-rtr)# router-id 1.1.1.1
R1(config)# interface g0/0/0
R1(config-if)# ipv6 ospf 123 area 1
```
#### OSPFv3 tuning
```
! Passive interface - old
D1(config)# ipv6 router ospf 123
D1(config-rtr)# passive-interface g1/0/23
! Passive interface - new
D2(config)# router ospfv3 123
D2(config-router)# passive-interface g1/0/23    ! For all AF
D2(config-router)# address-family ipv4 unicast
D2(config-router-af)# passive-interface g1/0/23 ! For a specific AF

! Summarization
R1(config)# router ospfv3 123
R1(config-router)# address-family ipv6 unicast
R1(config-router-af)# area 1 range 2001:db8:acad:1000::/52

! Change network type
R1(config)# interface g0/0/1
R1(config-if)# ospfv3 network point-to-point
```

#### Shows
```
D1# show ipv6 ospfv3
D1# show ipv6 protocols
D1# show ipv6 route ospf
D1# show ipv6 ospf neighbor ! Old
R1# show ospfv3 neighbor    ! New
R1# show ospfv3 database
R2# show ospfv3 interface brief
```

## CH11 - BGP
#### Routing process
!!! note
    The default behavior in IOS is IPv4 Address Family enabled

##### IPv4 Adress Family enabled by default
```
! 1000 is the AS number
R1(config)# router bgp 1000
R1(config-router)# bgp router-id 1.1.1.1
! Declare neighbors
R1(config-router)# neighbor 10.1.2.2 remote-as 500
R1(config-router)# neighbor 10.1.3.3 remote-as 300
! Create default route to a neighbor
R1(config-router)# neighbor 10.1.3.3 default-originate
! Declare networks
R1(config-router)# network 192.168.1.0 mask 255.255.255.224
R1(config-router)# network 192.168.1.64 mask 255.255.255.192
```
##### IPv4 Adress Family not enabled by default
```
! 1000 is the AS number
R1(config)# router bgp 1000
R1(config-router)# bgp router-id 1.1.1.1
! Disable enabled by default
R1(config-router)# no bgp default ipv4-unicast
! Declare neighbors
R1(config-router)# neighbor 10.1.2.2 remote-as 500
R1(config-router)# neighbor 10.1.3.3 remote-as 300
! Create default route to a neighbor
R1(config-router)# neighbor 10.1.3.3 default-originate
R1(config-router)# address-family ipv4
! Activate neighbors
R1(config-router-af)# neighbor 10.1.2.2 activate
R1(config-router-af)# neighbor 10.1.3.3 activate
! Declare networks
R1(config-router-af)# network 192.168.3.0 mask 255.255.255.224
R1(config-router-af)# network 192.168.3.64 mask 255.255.255.192
```
#### Summarization
```
R1(config)# router bgp 100
! Advertise all sumareized networks and the summary route
R1(config-router)# aggregate-address 192.168.1.0 255.255.255.0
! Only advertise the summary route
R1(config-router)# aggregate-address 192.168.1.0 255.255.255.0 summary-only
```
#### Shows
```
R2# show ip bgp
R2# show ip bgp 192.168.1.0
R1# show ip route bgp
R1# show ip bgp neighbors
```
## CH12 - Advanced BGP / MP-BGP
#### Routing process
```
! 1000 is the AS number
R1(config)# router bgp 1000
R1(config-router)# bgp router-id 1.1.1.1
! Disable enabled by default
R1(config-router)# no bgp default ipv4-unicast

! Declare neighbors
R1(config-router)# neighbor 10.1.2.2 remote-as 500
R1(config-router)# neighbor 2001:db8:acad:1012::2 remote-as 500

! Activate IPv4 neighbors, decalre IPv4 networks and remove IPv6 neighbors on IPv4 AF
R1(config-router)# address-family ipv4
R1(config-router)# neighbor 10.1.2.2 activate
R1(config-router)# no neighbor 2001:db8:acad:1012::2 activate
R1(config-router-af)# network 10.1.2.0 mask 255.255.255.0

! Activate IPv6 neighbors, decalre IPv6 networks on IPv6 AF
R1(config-router)# address-family ipv6 unicast
R1(config-router-af)# neighbor 2001:db8:acad:1012::2 activate
R1(config-router-af)# network 2001:db8:acad:1000::/64
```

#### Route manipulation
##### ACL route filtering
```
R1(config)# ip access-list extended ALLOWED_TO_R2
R1(config-ext-nacl)# permit ip 192.168.3.0 0.0.0.0 255.255.255.224 0.0.0.0
R1(config-ext-nacl)# permit ip 192.168.3.64 0.0.0.0 255.255.255.192 0.0.0.0
R1(config-ext-nacl)# exit
R1(config)# router bgp 1000
R1(config-router)# address-family ipv4 unicast
R1(config-router-af)# neighbor 10.1.3.1 distribute-list ALLOWED_TO_R2 out
R1# clear bgp ipv4 unicast 1000 out
```

##### IPv4 Prefix-list-based route filtering
```
R1(config)# ip prefix-list ALLOWED_FROM_R2 seq 5 permit 192.168.2.0/24 le 27
R1(config)# router bgp 1000
R1(config-router)# address-family ipv4 unicast
R1(config-router-af)# neighbor 10.1.2.2 prefix-list ALLOWED_FROM_R2 in
R1# clear bgp ipv4 unicast 1000 in
```

##### IPv6 Prefix-list-based route filtering
```
R1(config)# ipv6 prefix-list IPV6_ALLOWED_FROM_R2 seq 5 permit 2001:db8:acad:2000::/64
R1(config)# ipv6 prefix-list IPV6_ALLOWED_FROM_R2 seq 10 permit 2001:db8:acad:2001::/64
R1(config)# router bgp 1000
R1(config-router)# address-family ipv6 unicast
R1(config-router-af)# neighbor 2001:db8:acad:1012::2 prefix-list IPV6_ALLOWED_FROM_R2 in
R1# clear bgp ipv6 unicast 500 in
```

##### AS-PATH ACL filtering
```
R1(config)# ip as-path access-list 13 permit ^$
R1(config)# router bgp 1000
R1(config-router)# address-family ipv4 unicast
R1(config-router-af)# neighbor 10.1.2.2 filter-list 13 out
R1# clear bgp ipv4 unicast 1000 out
```

#### Path selection with route-map
```
R1(config)# ip prefix-list PREFERRED_IPV4_PATH seq 5 permit 192.168.3.0/24 le 27
R1(config)# route-map USE_THIS_PATH_FOR_IPV4 permit 10
R1(config)# match ip address prefix-list PERFERRED_IPV4_PATH
R1(config)# set local-preference 250
R1(config)# router bgp 1000
R1(config-router)# address-family ipv4 unicast
R1(config-router-af)# neighbor 10.1.3.130 route-map USE_THIS_PATH_FOR_IPV4 in
R1# clear bgp ipv4 unicast 1000 in
```

#### Shows
```
R1# show bgp ipv4 unicast summary
R1# show bgp ipv6 unicast summary
R1# show bgp ipv4 unicast 192.168.1.0 255.255.255.0
R1# show bgp ipv6 unicast 2001:db8:acad:1000::/64
R1# show ip route bgp
R1# show ipv6 route bgp
R1# show bgp ip unicast neighbors 10.1.2.2 routes
R1# show bgp ipv6 unicast neighbors 2001:db8:acad:1012::2 routes
R1# show bgp ipv4 unicast neighbors 10.1.2.2 advertised-routes
R2# show bgp ipv6 unicast neighbors 2001:db8:acad:1012::2 advertised-routes
```