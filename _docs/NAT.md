---
title: "Network Address Translation"
permalink: /docs/NAT/
excerpt: "Instructions for installing the theme for new and existing Jekyll based sites."
last_modified_at: 2019-08-20T21:36:18-04:00
toc: true
---

## **Network Address Translation**


### **What it is**

Network Address Translation (NAT) is the process of mapping one or more private IPv4 addresses to one or more public IPv4 addresses. Its primary function is to provide devices in a local network with the capability to communicate with an external network, most commonly the Internet, whilst preserving the public IPv4 address space.


### **History**

NAT provides a solution to _IPv4 address exhaustion_; a term used to describe the depletion of unique IPv4 addresses caused by widespread Internet adoption, which was not accounted for during the design phase of the web’s initial architecture.

A total of 4.3 billion unique IPv4 addresses were made available when engineers opted for a 32-bit address space in 1977. Whilst this number matched the global population at the time, the explosive growth in global connectivity in the years that followed made it clear assigning a unique public IPv4 address to every device was impossible.

Although IPv6’s 128-bit address system was introduced to also contend with the same issue, adoption has been slow, meaning NAT is still widely used and an important concept to understand.


### **How NAT works**

NAT can be implemented in three different ways:



1. **Static NAT** - one-to-one mapping of a single private IPv4 address to a single public IPv4 address.
2. **Dynamic NAT** - mapping of a single private IPv4 address to a public IPv4 address allocated from a pool of public addresses.
3. **Port Address Translation** - mapping of multiple private IPv4 addresses to a single public IP address using port numbers.

Port Address Translation is the most common application of NAT. The process is usually handled by physical devices, with routers usually performing the function in both home and corporate networks. 

The diagram below gives a visual representation of how NAT works, showing the flow of traffic in a common home network, consisting of two laptops and one smartphone behind a router. 

The configuration is as follows:



* All three devices have been assigned a private IPv4 address by the router
* The router has a public IP address of 85.241.20.11

**Note:** Each device is attempting to access [https://www.google.com](https://www.google.com) from the local network. 
{: .notice--warning}
<figure>
  <img src="{{ '/assets/images/NAT_Diagram.png' | relative_url }}" alt="NAT diagram">
</figure>

The successful transmission of data from source to destination and back again follows this order:



1. All three end devices send data to Google, with a destination IP address of **8.8.8.8** and a destination port of **443**.
2. The router receives the data and adds the private source IP address and private source port of each device to its NAT table.
3. The router translates each private source IP address into its own public IP address and each private source port into a unique public port and adds this information to the NAT table.
4. The router forwards the data to **8.8.8.8:443.**
5. The return traffic for each end device arrives back at the router, with a destination IP address of **85.241.20.11** (the public IP address of the NAT device) and a destination port of the device's public port. 
6. The router translates the public IP address and public port of each data packet back to the private IP address and public port numbers stored in the NAT table before delivering the data to its final destination. 


### **Security Benefit of NAT**

NAT adds an additional layer of security to home and corporate networks by virtue of its obfuscation of private IP addresses. The use of a single IP public address in front of multiple devices allows incoming traffic to be filtered by the NAT device before it can reach the end destination, allowing policies and rules to be put in place in order to filter or block unwanted traffic.
