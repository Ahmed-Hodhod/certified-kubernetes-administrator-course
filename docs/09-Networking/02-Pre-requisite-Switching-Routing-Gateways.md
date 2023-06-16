# Pre-requisite Switching Routing Gateways

  - Take me to [Lecture](https://kodekloud.com/topic/pre-requisite-switching-routing-gateways-cni-in-kubernetes/)

In this section, we will take a look at **Switching, Routing and Gateways**

## Switching
- we connect the two machines to a switch to form a netowrk.
- To see the interface on the host system
  
```
$ ip link
```
- To see the IP Address interfaces.

```
$ ip addr
```

![net-14](../../images/net14.PNG)

## Routing
- we use routers to connect multiple networks together.
- router is assigned an ip on each side of the connected networks.

- To see the existing routing table on the host system.

```
$ route
```
```
$ ip route show
or
$ ip route list
```

- To add entries into the routing table.

```
$ ip route add 192.168.1.0/24 via 192.168.2.1
```

![net-15](../../images/net15.PNG)

## Route table
Each entry in a route table specifies the destination network and the corresponding next-hop or outgoing interface to reach that network.

## Gateways

- To add a default route.
- you can add an entry in the route table to allow traffic to any external unknow network (0.0.0.0) and this router in this case is known as a gateway.
- if you want to have a dedicated router to communicate to specific networks, you need to add the routes to them all in the route table.
- you can have a router for internal networks and another for internet.
```
$ ip route add default via 192.168.2.1
```
- IP forwarding means that packets can be forwarded from one ethernet eth0 to another eth1
- To check the IP forwarding is enabled on the host.
```
$ cat /proc/sys/net/ipv4/ip_forward
0

$ echo 1 > /proc/sys/net/ipv4/ip_forward
```

- Enable packet forwarding for IPv4.
```
$ cat /etc/sysctl.conf

# Uncomment the line
net.ipv4.ip_forward=1
```

- To view the sysctl variables.
```
$ sysctl -a 
```

- To reload the sysctl configuration.
```
$ sysctl --system
```





