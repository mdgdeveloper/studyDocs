
# Index of Contents
1. [Business Intent Overlays (BIO)](#business-intent-overlays)
2. [Silver Peak Network Elements](#silver-peak-network-elements)
3. [Silver Peak Deployment](#deployment)


# Business Intent Overlays
Two types of tunnels to route the data:
1. **Underlays**: Using the physical connection. One tunnel per connection.
2. **Overlays**: Using one or more underlay tunnels. Logical tunnel. Treated as unique link. Logical

## Traffic Handling Features (Silver Peak)
### Path conditioning
1. **Forward Error Correction**
2. **Packet Order Correction**

### Dynamic Path Control (DPC)
Dyanically select the appropriate underlay tunnel with a BIO's Link Bonding policy

### BOOST 
WAN optimization technologies. 
* **TCP Acceleration**: Optimizes the TCP protocol to mitigate the effects of latency. 
* **Network Memory**: Deduplicates transmitted data to reduce congestion. 


## Dynamic Path Control and Link Bonding 
Controlled on a per Overlay basis by configuration options called:
**Link Bonding Policies**:
* FEC
* Quality
  * Loss
  * Latency
  * Jitter
  * Mean Opinion Score (MOS)
* Load-Balance

Group of technologies that Silver Peak uses to select best path. 

## Path Conditioning
Packet Loss and Packet Out of Order are issues that must be handled by the endpoint.

Affects Video and Audio connections. 
Commonly seen on shared infraestructure links such as MPLS or Internet Based IP VPNs. 

## Boost 
Boost components:
* Overcome Latency ➡ **TCP Acceleration**
  * Window scaling
  * Selective akcnowledgement
  * Round Trip Time Measurement 
  * High Speed TCP 

    With TCP Acceleration, the transmitting device experiences only LAN latency, using a TCP Proxy in Silver Peak appliance.

    TCP Accelearation REQUIRES **symmetric flows.**

* Reduce Congestion ➡ **Network Memory**
  * Deduplication
    * Does not work if the data is encrypted.
  * Compression (LZ compression)

# Silver Peak Network Elements
## Cloud Portal
All licenses are managed by the Portal. No customer action required.
## Orchestrator
Management software for Silver Peak devices. 
Must register with Cloud Portal. Required for EdgeConnect appliance registration approval.
## Edge Connect
Appliances.

Must register with Cloud Portal to operate. Crate network connections and move data as directed by Orchestrator. Devices must reregister with Portal periodically. 

The licensing can be done through Orchestrator if EdgeConnect devices have no direct Internet connection. 

Orchestrator can act as a proxy server. 


# Path Selection
## Subnet Sharing 
Used between appliances to advertise to each other through tunnels (like routing protocol):
* Directly attached.
* Manually added static.
* Subnets learned via a routing protocol. 


## Routing Protocols
* Silver Peak 
* BGP
* OSPF

## Routes table
Redistribution
* Configurable globally per protocol
* Configurable per route per static routes. 

## Management Routes
Separate from data path routes. 

# Deployment 

## Deployment Types
Two types of deployment of Silver Peak appliances:
### ILRM: Inline Router Mode
✅ Recommended

All traffic must physically flow through appliance.

### OPRM: Out of Path Router Mode
L3 router or switch must redirect packets to appliance

## Deployment Modes
### Router Mode
### Bridge Mode
### Server Mode 
