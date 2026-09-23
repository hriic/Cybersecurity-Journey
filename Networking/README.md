# Networking for Cybersecurity

Networking is the foundation I rely on when I enumerate a target or analyze traffic. I do not want to treat a port number as an answer; I want to understand how hosts communicate, what the protocol is supposed to do, and what abnormal behavior would look like.

## Core model

When two systems communicate I think about the path from application data down to packets and frames: application protocols such as HTTP, DNS and SSH use transport protocols such as TCP or UDP; IP provides addressing and routing; the local network handles delivery to the next hop.

### TCP
TCP is connection-oriented. A connection normally starts with the three-way handshake: SYN, SYN/ACK, ACK. Sequence numbers, acknowledgements and retransmissions provide reliable ordered delivery. This is why TCP scanning can reason about states such as open, closed and filtered from the responses a host returns.

### UDP
UDP has no handshake and does not guarantee delivery or ordering. That lower overhead is useful for protocols such as DNS, but it also makes UDP enumeration different: silence does not necessarily mean a port is closed.

## Addressing and subnetting

IPv4 addresses identify interfaces on a network. A subnet mask/CIDR prefix separates network and host portions. I am practicing subnetting because it affects discovery scope, routing and the way I interpret a target range.

Important ideas: private vs public addresses, loopback, default gateway, network/broadcast addresses, CIDR notation, NAT and routing.

## ARP and local networks

ARP maps an IPv4 address to a MAC address on the local Layer-2 network. Before sending an Ethernet frame to a local peer, a host may need to discover its MAC address. This is useful context when reading packet captures and understanding local-host discovery.

## Protocols I have studied

| Protocol | Default port | Purpose | Security perspective |
| --- | ---: | --- | --- |
| FTP | 21/TCP | File transfer/control | Traditional FTP can expose credentials/data in plaintext |
| SSH | 22/TCP | Secure remote administration | Key authentication, server configuration and exposed versions matter |
| Telnet | 23/TCP | Remote terminal | Plaintext traffic makes it unsuitable for sensitive administration |
| SMTP | 25/TCP | Mail transfer | Banner/configuration can reveal useful service information |
| DNS | 53/UDP,TCP | Name resolution | Records, unusual queries and tunneling behavior can be relevant |
| HTTP | 80/TCP | Web applications | Leads into headers, methods, directories, auth and application testing |
| POP3 | 110/TCP | Mail retrieval | Plaintext variants can expose credentials/content |
| IMAP | 143/TCP | Mailbox access | Useful for understanding mail protocols and packet analysis |
| HTTPS | 443/TCP | HTTP over TLS | Encryption protects transport, not application logic flaws |
| SMB | 445/TCP | Windows file/resource sharing | Important in Windows and Active Directory environments |

## Packet analysis

I use **Wireshark** for interactive packet analysis and **tcpdump** for command-line capture. I distinguish a capture filter, which limits what is collected, from a display filter, which narrows already captured traffic. I have practiced inspecting plaintext Telnet/mail traffic and protocol-focused filters such as `imap`.

## How networking changes my pentesting workflow

An open port is only the beginning. My process is: **host → port → protocol/service → version/configuration → protocol-specific enumeration → evidence-driven next step**. This prevents me from jumping straight to an exploit before understanding what is actually exposed.

See also: [DNS](DNS.md), [Protocols and Ports](Protocols-and-Ports.md), [Traffic Analysis](Traffic-Analysis.md), and [Nmap](../Nmap/).