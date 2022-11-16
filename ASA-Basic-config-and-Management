# ASA

ASA:


`int e0`
`nameif Outside` 
[Default sec level 0, for the name "inside", default is 100]

`ip addr 192.1.20.10`   
[if mask not specified, it takes the default mask of that class]

`show run int e0`

_______________________________________

### Preconfiguration on the Routers [Lab]
```
interface Ethernet2
 nameif DMZ-3
 security-level 50
 ip address 192.168.3.10 255.255.255.0
ASA(config)# show run int e3
!
interface Ethernet3
 nameif DMZ-4
 security-level 50
 ip address 192.168.4.10 255.255.255.0
ASA(config)#
ASA(config)# show run int e1
!
interface Ethernet1
 nameif Inside
 security-level 100
 ip address 10.11.11.10 255.255.255.0
ASA(config)# show run int e0
!
interface Ethernet0
 nameif Outside
 security-level 0
 ip address 192.1.20.10 255.255.255.0
```
_______________________________________

### Clear configuration:
`#clear configure interface e1`


### Check telnet config on Routers
 ```
show run | s line
line con 0
	no exec-time
	logging synchronous
```

```
line vty 0 4
	password cisco
	login
	transport input all
```



---------------
### Show the connection table
ASA does TCP/UDP (Implicit Inspection) Inspection, creates connection table(entry) to allow return traffic.

```
show conn

TCP Outside  192.1.20.2:23 Inside  10.11.11.1:39275, idle 0:00:04, bytes 162, flags UIO
```

If we try to ping, it will not ping - because it only creates entries for TCP AND UDP and not for ICMP traffic

____________________________________________

## Next:-
### Allowing traffic through the firewall

- Allow the inside users to ping outside and get a response
- Allow R2 to ping R1
- Allow R2 Loopback 2.2.2.2 to Telnet to 10.11.11.1


---------------------
- ACL on the firewall is a Named Extended ACL.

`# access-list OUTSIDE permit icmp any 10.11.11.0 255.255.255.0 echo-reply` <OUTSIDE is the name of the ACL>
`# access-list OUTSIDE permit icmp host 192.1.20.2 host 10.11.11.1 echo` <2nd requirement>
`# access-list OUTSIDE permit tcp host 2.2.2.2 host 10.11.11.1 eq 23`

### Applying the ACL
`access-group OUTSIDE in Interface outside`

`Show run access-list`
`show access-list` <Shows the hit counts as well>
`show run access-group`
---------------------

### Static Route on the ASA:

```
route <EXIT-interface> <Network> <Mask> <Next-HOP>
route Outside 2.0.0.0 255.255.255.0 192.1.20.2
```
__________________________________

### Telnet from specific Source interface

On the router:
`telnet 2.2.2.2 /source-interface lo0`
___________________________________

By default, no traffic is allowed (even with allow ALL ACL) in between the interfaces with same security leve.
Command to allow Traffic between interfaces with same security level: <No ACL required, throughout>
`same-security-traffic permit inter-interface`
________________________________________

### ANY-ANY ACL:

`access-list ALL extended permit ip any any`



--------------------------------------------------------
### ENABLING HTTPS SERVER FOR ASDM ON ASA:

`ASA1(config)# http server enable`

Instead of giving everyone access to the HTTP server we will specify which network and interface are permitted to use the HTTP server:

`ASA1(config)# http 192.168.1.0 255.255.255.0 INSIDE`

This will only allow network 192.168.1.0 /24 on the inside interface to reach the HTTP server. It might be even a better idea to only allow one or two IP addresses that you use for management instead of an entire network.

`ASA1(config)# username ADMIN password PASSWORD privilege 15`

++++++++++++++++++++++++++++++++++++++++

- ACL only controls the through traffic
- it DOESN'T affect the TO traffic <destined to the firewall>
- Traffic to the firewall is controlled by enabling or disabling services
- the only service that is enabled by default [Implicitly] is ICMP <THAT IS WHY WE CAN PING IT>

----:>>>> other services: telnet, http

- The services are enabled or disabled on per interface basis

command:

`(config)# icmp deny <source> <interface>`
`# icmp deny any outside`    ---------------:>>>>>>>> Even the through icmp gets blocked 

Alternative solution:

`# icmp permit any echo-reply outside`   ---:>>>>>>>> there is an implicit deny

--------------
### ICMP:

- By default, all ICMP traffic to the firewall is allowed implicitly.
- if you create a filter by using the ICMP command, the rest of the traffic will be blocked

icmp permit any echo-reply outside
icmp deny any outside


e.g.:

Allow ASA to ping Outside and get a response
Allow R2 - 2.2.2.2 to ping outside interface of the ASA
the rest of the ICMP traffic to the outside interface should be blocked

icmp permit any echo-reply outside
icmp permit host 2.2.2.2 echo outside
<< implicit deny will take care of the 3rd requirement - other icmp traffic than specified will be blocked>>

--------------

### ENABLE telnet on the ASA

1. Enable the Service and specify the IPs that are allowed and the interface 

Allow telnet from 10.11.11.11 host:
`telnet 10.11.11.1 255.255.255.255 inside`
2. Configure the password:
`password Cisco123`


Note: Telnet is not allowed from an interface that has a security level of 0.

### Enable SSH
1. Generate a RSA crypto key
`domain-name example.com` -> Requires a FQDN
`crypto key generate rsa modulus 1024` -> 1024 is the size of the key

2. Enable the Service specify the IPs that are allowed and the interface 
`ssh 10.11.11.1 255.255.255.255 inside`
`ssh 192.1.20.0 255.255.255.0 outside`

3. SSH will need a username and password [REQUIRED]
Creating LOCAL user account:
`username admin1 password Cisco123`

4. Specify that SSH should use the local DB for authentication [DOESN'T DO IT BY DEFAULT]

`aaa authentication ssh console LOCAL` -> LOCAL is case-sensitive and upper case

**To SSH**:
`ssh -l admin1 192.1.20.10`

- Allow SSH to the Firewall from R1 and the Outside Network
- Set the username and password for ssh as admin1/cisco123

`domain-name example.com`
`crypto key generate rsa modulus 1024`
!
`ssh 10.11.11.1 255.255.255.255 inside`
`ssh 192.1.20.0 255.255.255.0 outside`
!
`username admin1 password Cisco123`
`aaa authentication ssh console LOCAL`

