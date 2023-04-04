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