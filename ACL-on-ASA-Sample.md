# ASA ACL:

Types of ACL:

1. Extended: Used in 95% of cases:
- An access-list that is widely used as it can differentiate IP traffic.
- It uses both source and destination IP addresses and port numbers to make sense of IP traffic.
- You can also specify which IP traffic should be allowed or denied.
- They use the numbers 100-199 and 2000-2699.
2. Standard ACL:
- An access-list that is developed solely using the source IP address.
- These access control lists allow or block the entire protocol suite.
- They don’t differentiate between IP traffic such as UDP, TCP, and HTTPS.
- They use numbers 1-99 or 1300-1999 so the router can recognize the address as the source IP address.

--------------------------
Sample ACL:

- ACL to allow tcp traffic from outside network to inside network using extended ACL:
- 
```
int Gig 0
nameif Outside
ip addr 102.1.1.1 255.255.255.0
no shut
!
int Gig 0
nameif Inside
ip addr 192.168.1.1 255.255.255.0
no shut
!
object network InsideNet
	subnet 192.168.1.0 255.255.255.0
	exit
!
object network OutsideNet
	subnet 102.1.1.0 255.255.255.0
	exit
!
! Creating Access List
access-list OUT-In-Allow extended permit tcp object OutsideNet object InsideNet 
!
! Applying the ACL
access-group OUT-In-Allow in interface outside
```



