# AS112-IXP-ansible
An Ansible playbook to spin up and configure an AS112 DNS server for IXP operation

This playbook isn't complete, so don't expect it to entirely work yet.

This playbook assumes that it's being run on a server with two interfaces:
* The Out of Band (oob) interface on a globally routed subnet with default pointed at a gateway and no other routing.
* The Internet Exchange (IXP) interface where it will establish BGP sessions with the two route servers of the exchange and presumably receive most of the DNS queries.

The relevant AS112 anycast addresses are all added to the loopback interface, and BIND is configured to respond to AS112 authoritative requests on those eight addresses plus both sets of IXP and OOB addresses.


