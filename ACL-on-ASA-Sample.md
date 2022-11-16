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

---------------
## Also, there are two categories of access-list:  

### Numbered access-list
– These are the access list that cannot be deleted specifically once created i.e if we want to remove any rule from an Access-list then this is not permitted in the case of the numbered access list. If we try to delete a rule from the access list then the whole access list will be deleted. The numbered access-list can be used with both standard and extended access lists. 
 
### Named access list 
– In this type of access list, a name is assigned to identify an access list. It is allowed to delete a named access list, unlike numbered access list. Like numbered access lists, these can be used with both standards and extended access lists. 
 
## Rules for ACL – 

- The standard Access-list is generally applied close to the destination (but not always).
- The extended Access-list is generally applied close to the source (but not always).
- We can assign only one ACL per interface per protocol per direction, i.e., only one inbound and outbound ACL is permitted per interface.
- We can’t remove a rule from an Access-list if we are using numbered Access-list. If we try to remove a rule then the whole ACL will be removed. If we are using named access lists then we can delete a specific rule.
- Every new rule which is added to the access list will be placed at the bottom of the access list therefore before implementing the access lists, analyses the whole scenario carefully.
- As there is an implicit deny at the end of every access list, we should have at least a permit statement in our Access-list otherwise all traffic will be denied.
- Standard access lists and extended access lists cannot have the same name. 
 
## Advantages of ACL – 

- Improve network performance.
- Provides security as the administrator can configure the access list according to the needs and deny the unwanted packets from entering the network.
- Provides control over the traffic as it can permit or deny according to the need of the network.- 


### More Info. here:
https://study-ccna.com/types-of-acls/
