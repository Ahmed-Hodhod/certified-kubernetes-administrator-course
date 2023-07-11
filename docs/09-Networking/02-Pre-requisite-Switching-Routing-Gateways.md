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

# Router vs Gateway 

Gateways and routers are both networking devices used to connect and manage network traffic, but they serve different functions and operate at different levels of the network stack. Here's a comparison between gateways and routers:

Function:

Gateway: A gateway is a device that connects two or more networks with different protocols, architectures, or communication formats. It acts as an entry and exit point for data between these networks, translating the protocols or formats as necessary. Gateways can also provide additional services such as security, protocol conversion, and address translation.
Router: A router is a device that connects multiple networks and directs network traffic based on IP addresses. It determines the best path for data packets to reach their destination by using routing tables and protocols, such as OSPF (Open Shortest Path First) or BGP (Border Gateway Protocol).
Network Layers:

Gateway: Gateways operate at higher network layers, typically at the application layer (Layer 7) or the presentation layer (Layer 6) of the OSI model. They deal with data in a more abstract manner and handle different protocols and data formats.
Router: Routers operate at the network layer (Layer 3) of the OSI model. They work with IP addresses and route data packets based on the destination IP address.
Network Types:

Gateway: Gateways are commonly used in scenarios where different types of networks are interconnected, such as connecting a local network (LAN) to the internet or connecting two networks that use different protocols.
Router: Routers are used in various networking environments to connect different networks, including home networks, office networks, and internet service provider (ISP) networks.
Addressing and Routing:

Gateway: Gateways often perform address translation between networks, allowing devices in one network to communicate with devices in another network using different addressing schemes.
Router: Routers use routing tables and protocols to determine the best path for data packets to reach their destination network based on IP addresses.
Complexity and Features:

Gateway: Gateways are typically more complex devices that provide additional features beyond basic routing. They may include firewall functionality, proxy servers, content filtering, and more.
Router: Routers focus primarily on routing and forwarding packets based on IP addresses. While they may have some additional features like network security or Quality of Service (QoS) capabilities, their primary function is efficient packet forwarding.
In summary, a gateway is a device that connects networks with different protocols or formats, while a router connects multiple networks based on IP addresses. Gateways operate at higher network layers and often perform protocol translation, while routers operate at the network layer and primarily handle IP routing.


# Internet gateway vs NAT gateway 

An Internet Gateway and a NAT Gateway are both networking components used in cloud computing environments to enable connectivity between private networks and the internet. However, they serve different purposes and have distinct functionalities. Here's a comparison between an Internet Gateway and a NAT Gateway:

Internet Gateway:
An Internet Gateway is a horizontally scalable, highly available, and fully managed service provided by cloud service providers. It acts as a bridge between a Virtual Private Cloud (VPC) and the internet, allowing resources within the VPC to communicate with the internet and vice versa.

Key features of an Internet Gateway include:

Ingress/Egress Traffic: An Internet Gateway facilitates inbound and outbound network traffic between the VPC and the internet. It allows resources within the VPC to access the internet for tasks like software updates, browsing, and interacting with external services.

Public IP Address Assignment: An Internet Gateway is associated with a public IP address that serves as the public-facing address for resources within the VPC. It enables communication between internet users and resources in the VPC.

Routing: An Internet Gateway handles the routing of traffic between the VPC and the internet. It typically uses routing tables to determine the appropriate path for traffic flow.

Access Control: Access control and security measures, such as network ACLs (Access Control Lists) and security groups, can be configured to regulate traffic between the VPC and the internet through the Internet Gateway.

NAT Gateway:
A NAT (Network Address Translation) Gateway is also a managed service provided by cloud service providers. It allows resources within a private subnet in a VPC to access the internet while keeping them hidden behind private IP addresses. It acts as a NAT device, translating private IP addresses to a single public IP address.

Key features of a NAT Gateway include:

Outbound Internet Access: A NAT Gateway enables resources within private subnets to establish outbound connections to the internet. It allows these resources to access external services or download software updates while keeping their internal IP addresses hidden.

IP Address Translation: The NAT Gateway performs network address translation, translating private IP addresses of the resources to a public IP address. This enables communication with external networks without revealing internal IP information.

Stateless Communication: NAT Gateway operates in a stateless manner, meaning it does not maintain a stateful connection table for incoming traffic. It allows outbound traffic initiated by resources within private subnets to traverse the NAT Gateway, but incoming connections from the internet are not allowed.

High Availability: Similar to an Internet Gateway, a NAT Gateway is highly available and can be horizontally scaled to handle higher traffic loads. Cloud providers ensure the availability and redundancy of NAT Gateway instances.

In summary, an Internet Gateway facilitates bidirectional traffic between a VPC and the internet, allowing resources within the VPC to access the internet and receive inbound connections. On the other hand, a NAT Gateway enables outbound internet access from private subnets within a VPC by translating internal IP addresses to a single public IP address. NAT Gateway primarily focuses on outbound communication while keeping internal resources hidden, whereas an Internet Gateway handles bidirectional traffic flow between the VPC and the internet.




