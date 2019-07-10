# AS112-IXP-ansible
An Ansible playbook to spin up and configure an AS112 DNS server for IXP operation

This playbook assumes that it's being run on a server with two interfaces:
* The Out of Band (oob) interface on a globally routed subnet with default pointed at a gateway and no other routing.
* The Internet Exchange (IXP) interface where it will establish BGP sessions with the two route servers of the exchange and presumably receive most of the DNS queries.

The relevant AS112 anycast addresses are all added to the loopback interface, and BIND is configured to respond to AS112 authoritative requests on those eight addresses plus both sets of IXP and OOB addresses.

The OOB addresses need to be unfiltered public addresses, since the server's default is pointed at the OOB's gateway. This means you can't use NAT, and you can't filter AS112 source addresses on that subnet (i.e. BCP38) since it's possible for queries to be routed to the AS112 node on the IXP while the route servers lack the return path.

To run this playbook, install an Ubuntu 18.04 server, add your SSH key to its authorized keys, and edit the sudo config file to allow running sudo without a password.

ansible-playbook -i ./inventory.yaml ./playbook.yaml


The example inventory file should be modified to meet the local configuration. Some notes:

* All IPv6 subnets are assumed to be /64s
* The soa_rname is the email address for this DNS node
* The oob_dns_servers field probably doesn't need to be changed, but everything else probably should.

