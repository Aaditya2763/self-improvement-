# How Internet Works

## Learning Objectives
- Understand internet architecture
- Learn client-server model
- Master DNS and routing
- Know IP addressing

## Topics

### 1. Internet Basics
- What is the internet?
- Network types (LAN, WAN)
- ISP and backbone
- Peer-to-peer vs client-server

### 2. IP Addressing
- IPv4 addresses (xxx.xxx.xxx.xxx)
- IPv6 addresses
- Subnets and CIDR notation
- Public vs private addresses

### 3. DNS (Domain Name System)
- DNS hierarchy
- DNS lookup process
- Resolvers and name servers
- DNS records (A, AAAA, CNAME, MX)

### 4. Data Transmission
- Packets and packet switching
- Routing protocols
- Traceroute
- Network topology

### 5. Network Layers
- Physical layer
- Data link layer (MAC)
- Network layer (IP)
- Transport layer (TCP/UDP)
- Application layer (HTTP, SMTP)

## DNS Process

```
1. User enters URL
   ↓
2. Local DNS resolver (ISP)
   ↓
3. Root nameserver (.com, .org, etc)
   ↓
4. TLD nameserver (Top Level Domain)
   ↓
5. Authoritative nameserver
   ↓
6. IP address returned
   ↓
7. Browser connects to IP
```

## Network Layers Diagram
```
Application Layer (HTTP, HTTPS, SMTP, FTP)
     ↓
Transport Layer (TCP, UDP)
     ↓
Network Layer (IP - Routing)
     ↓
Data Link Layer (MAC - Ethernet, WiFi)
     ↓
Physical Layer (Cables, Signals)
```

## Key Concepts
- **Packet**: Unit of data transmission
- **Router**: Directs packets between networks
- **Firewall**: Controls network traffic
- **Gateway**: Entry point to network
- **ISP**: Internet Service Provider

## Practice Tasks
- [ ] Understand IP addressing
- [ ] Learn DNS lookup
- [ ] Run traceroute command
- [ ] Study network protocols
- [ ] Understand routing

## Commands to Know
```bash
ipconfig / ifconfig          # Check IP
nslookup example.com         # DNS lookup
ping example.com             # Check connectivity
tracert example.com          # Trace route
netstat                      # Network statistics
```

## Interview Tips
- Explain DNS resolution
- Know OSI vs TCP/IP models
- Understand routing basics
- Explain packet transmission
- Know public vs private IPs

## Resources
- Mozilla: How the Internet Works
- Khan Academy: Internet Basics
- Cisco Networking Basics
