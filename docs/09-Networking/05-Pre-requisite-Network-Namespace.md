# Pre-requisite Network Namespaces

  - Take me to [Lecture](https://kodekloud.com/topic/prerequsite-network-namespaces/)

In this section, we will take a look at **Network Namespaces**
a local host has its own interfaces and its own environment. 
- To enable a container running on a host to have its own interfaces and route tables as if it is a host, we create naespaces.
- Namespaces are also used to isolate the containers from their hosts.
- In this lesson, we assume that the container is running in a namespace.
- for the namespace, you can configure different arp and routes as well.
- routes and arps are not inherited from the host environment.
- 

## Process Namespace

> On the container
```
$ ps aux      
```

> On the host
```
$ ps aux 

```

## Network Namespace

```
$ route
```

```
$ arp
```

## Create Network Namespace

```
$ ip netns add red

$ ip netns add blue
```
- List the network namespace

```
$ ip netns
```

## Exec in Network Namespace

- List the interfaces on the host

```
$ ip link
```

- Exec inside the network namespace

```
$ ip netns exec red ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00

$ ip netns exec blue ip link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
```
- You can try with other options as well. Both works the same.
```
$ ip -n red link
1: lo: <LOOPBACK> mtu 65536 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
```

## ARP and Routing Table

> On the host
```
$ arp
Address                  HWtype  HWaddress           Flags Mask            Iface
172.17.0.21              ether   02:42:ac:11:00:15   C                     ens3
172.17.0.55              ether   02:42:ac:11:00:37   C                     ens3
```

> On the Network Namespace
```
$ ip netns exec red arp
Address                  HWtype  HWaddress           Flags Mask            Iface

$ ip netns exec blue arp
Address                  HWtype  HWaddress           Flags Mask            Iface
```

> On the host 
```
$ route
```

> On the Network Namespace
```
$ ip netns exec red route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface

$ ip netns exec blue route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
```

## Virtual Cable

- To create a virtual cable between two interfaces (connect them together)
```
$ ip link add veth-red type veth peer name veth-blue
```

- To attach with the network namespaces
```
$ ip link set veth-red netns red

$ ip link set veth-blue netns blue
```

- To add an IP address to each namespace
```
$ ip -n red addr add 192.168.15.1/24 dev veth-red

$ ip -n blue addr add 192.168.15.2/24 dev veth-blue
```

- To turn it up `ns` interfaces
```
$ ip -n red link set veth-red up

$ ip -n blue link set veth-blue up
```

- Check the reachability 
```
$ ip netns exec red ping 192.168.15.2
PING 192.168.15.2 (192.168.15.2) 56(84) bytes of data.
64 bytes from 192.168.15.2: icmp_seq=1 ttl=64 time=0.035 ms
64 bytes from 192.168.15.2: icmp_seq=2 ttl=64 time=0.046 ms

$ ip netns exec red arp
Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.15.2             ether   da:a7:29:c4:5a:45   C                     veth-red

$ ip netns exec blue arp
Address                  HWtype  HWaddress           Flags Mask            Iface
192.168.15.1             ether   92:d1:52:38:c8:bc   C                     veth-blue

```

- Delete the link.
```
$ ip -n red link del veth-red
```

> On the host ( no idea what is going between the interfaces)
```
# Not available
$ arp
Address                  HWtype  HWaddress           Flags Mask            Iface
172.16.0.72              ether   06:fe:61:1a:75:47   C                     ens3
172.17.0.68              ether   02:42:ac:11:00:44   C                     ens3
172.17.0.74              ether   02:42:ac:11:00:4a   C                     ens3
172.17.0.75              ether   02:42:ac:11:00:4b   C                     ens3
```

## Linux Bridge

- Create a network namespace
- To connect many namespaces together, we need a central bridge to connect them all together.

```
$ ip netns add red

$ ip netns add blue
``` 
- To create a internal virtual bridge network, we add a new interface to the host named `v-net-0`
```
$ ip link add v-net-0 type bridge
```
- Display in the host ( considered a new interface ) 
```
$ ip link
8: v-net-0: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
    link/ether fa:fd:d4:9b:33:66 brd ff:ff:ff:ff:ff:ff
```
- Currently it's down, so turn it up
```
$ ip link set dev v-net-0 up
```
- To connect network namespace to the bridge. Creating a virtual cabel
```
$ ip link add veth-red type veth peer name veth-red-br

$ ip link add veth-blue type veth peer name veth-blue-br
```
- Set with the network namespaces
```
$ ip link set veth-red netns red

$ ip link set veth-blue netns blue

$ ip link set veth-red-br master v-net-0

$ ip link set veth-blue-br master v-net-0
```
- To add an IP address to the namespaces
```
$ ip -n red addr add 192.168.15.1/24 dev veth-red

$ ip -n blue addr add 192.168.15.2/24 dev veth-blue
```
- To turn it up `ns` interfaces
```
$ ip -n red link set veth-red up

$ ip -n blue link set veth-blue up
```
- To add an IP address
```
$ ip addr add 192.168.15.5/24 dev v-net-0
```
- Turn it up added interfaces on the host
```
$ ip link set dev veth-red-br up
$ ip link set dev veth-blue-br up
```

> On the host (you need to add an ip to the new interface to access the new network on the host)
```
$ ping 192.168.15.1   (not reachable)  
```

> On the ns ( 192.168.1.00 is a new network on a different host ) 
```
$ ip netns exec blue ping 192.168.1.1
Connect: Network is unreachable

$ ip netns exec blue route ( the route table of the blue interface has no awareness about other networks) 

$ ip netns exec blue ip route add 192.168.1.0/24 via 192.168.15.5 (forward any external traffic to the network interface ) 

# Check the IP Address of the host
$ ip a

$ ip netns exec blue ping 192.168.1.1  ( it is reachable now but you still did not get a response back, we need to enable the NAT on the host. so now the other network thinks that the traffic is coming from the host and not from inside the interface network ) 
PING 192.168.1.1 (192.168.1.1) 56(84) bytes of data.

$ iptables -t nat -A POSTROUTING -s 192.168.15.0/24 -j MASQUERADE

$ ip netns exec blue ping 192.168.1. ( reachble and we get a response ) 

$ ip netns exec blue ping 8.8.8.8 ( not reachable from the blue interface becauze there is no route for that in the route table ) 

$ ip netns exec blue route

$ ip netns exec blue ip route add default via 192.168.15.5

$ ip netns exec blue ping 8.8.8.8
```

- Adding port forwarding rule to the iptables ( to let other hosts and networks talk to our inteface network ) 

```
$ iptables -t nat -A PREROUTING --dport 80 --to-destination 192.168.15.2:80 -j DNAT
```
```
$ iptables -nvL -t nat
```


