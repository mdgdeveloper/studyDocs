
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
✅ Recommended (Best Practice)

All traffic must physically flow through appliance.

### OPRM: Out of Path Router Mode
L3 router or switch must redirect packets to appliance

## Deployment Modes
### Router Mode
✅ Recommended

Easiest mode to deploy:
* At least **two interfaces**: IP address & next-hop
* Data path between two or more different subnets.
* Flexible interfasce deployment options: Stateful firewall, ZBF
* Appliances can be managed via data path interfaces.
* Incoming LAN traffic: placed in any tunnel or sent direct-to-net. 

#### Types of deployment
1. Traditional HA
2. Edge High Availability (HA)
   1. Appliance Clustering
   2. Reduces Capex/Opex

#### Router Mode - Out of Path 
When ILRM is NOT possible. 

Traffic redirection is REQUIRED. 

### Bridge Mode
- LAN & WAN Interface Pairs: form a BRIDGE.
- Each pair is a BVI: Bridge Virtual Interface: One IP Address per BVI. 
- Stateful FW supported.
- ZBZ not supported 
- In bypass, appliance looks like crossover cable. 
- No EdgeHA mode

### Server Mode 
- Out of path ONLY
- Only one interface: mgmt0
- One IP addres
- No Firewall
- Traffic has to be redirected to the appliance. 

# Data Security 
## WAN Interface Firewall Modes
- FW Mode: Allow All
- FW Mode: Harden (Default) ⭕
- FW Mode: Stateful. Simple L3/L4 functionality. 
  - Not an IDS/IPS & not L7 content inspection. 
- FW Mode: Stateful + SNAT:
  - SNAT Applied outbound to Passthrough traffic only
  - Source address will be NAT

## Inbound Port Forwarding
Allow WAN-side devices to connect inbound to LAN-side. 

## Zone Based Firewall
- Assing interfaces and BIO's to zones
- Create Labels for each Zone
- Apply labels to interfaces/BIO
- Matrix to create ACL based on rules permit/deny 
- Intra-zone traffic **always allowed**.

## Encryption
- Data encryption:
  - Disk Encryption: 128 bit AES
  - IPSec_UDP: 256 bit AES

- SSL/TLS
  - Use the Key 
  - Re-Encrypts 
  - ❗ **REQUIRED FOR DEDUPLICATION**

# Configuration Overview
## Labels
- Identifiers to be applied to interfacs.
- Orchestrator will treat interfaces with the same labels in the same way.

## Deployment Profiles
- Configuration templates.
- Does NOT contain IP addresses.
- Can be applied to each appliance.

## Business Intent Overlays
- Configuration templates Orchestrator uses to dynamically create the overlay network. 
- Define which interfaces at each site should be connected based on the labels.
- LAN traffic - WAN Traffic associations based on the labels. 

---
## Order of operations
1. Design and Prepare
   - Obtain/create topology diagrams.
   - Know link speeds.
   - Understand firewalls and NAT/PAT in use.
   - Decide which traffic should be routed to which sites over shich underlay link types.
   - Indetify any hub sites.
2. Install and Licenses Orchestrator
3. Configure Orchestrator.
  - Create Labels
  - Create BIO
  - Create ACLs and other required configuration templates
  - Create Deployment Profiles for type of sites.
4. Install Appliances 
  - Register appliances with the Cloud Portal
  - Approve the appliances in Orchestrator.


