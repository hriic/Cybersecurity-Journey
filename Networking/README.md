# Networking

These are the networking concepts I have studied and used in security labs.

## Protocols

| Protocol | Port | Notes |
| --- | ---: | --- |
| SSH | 22 | Encrypted remote access |
| FTP | 21 | File transfer |
| Telnet | 23 | Remote access over plaintext |
| SMTP | 25 | Sending email |
| DNS | 53 | Name resolution |
| HTTP | 80 | Web traffic |
| POP3 | 110 | Retrieving email |
| IMAP | 143 | Accessing and managing email |
| HTTPS | 443 | HTTP protected with TLS |
| SMB | 445 | Windows file and printer sharing |

## TCP and UDP

TCP is connection-oriented and provides reliable, ordered delivery. UDP is connectionless and has lower overhead, but does not provide the same delivery guarantees.

This matters during scanning because TCP and UDP services need different approaches.

## DNS

DNS translates domain names into IP addresses. I have also studied DNS tunneling, where DNS queries and responses can be abused to carry data or command-and-control traffic.

## Packet analysis

I have practiced capturing and filtering traffic with tools such as tcpdump and Wireshark. For example, understanding that an old protocol such as Telnet can expose traffic in plaintext makes packet captures much easier to reason about.

## Current goals

- Get faster at subnetting
- Improve packet analysis
- Understand routing more deeply
- Get better at recognizing unusual network traffic
