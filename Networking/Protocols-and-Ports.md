# Protocols and Ports

| Protocol | Port | What I learned |
| --- | ---: | --- |
| FTP | 21 | File transfer; traditional FTP does not encrypt traffic |
| SSH | 22 | Encrypted remote administration |
| Telnet | 23 | Remote terminal protocol that sends traffic in plaintext |
| SMTP | 25 | Mail transfer |
| DNS | 53 | Name resolution |
| HTTP | 80 | Web traffic |
| POP3 | 110 | Retrieving email |
| IMAP | 143 | Accessing and managing email |
| HTTPS | 443 | HTTP protected with TLS |
| SMB | 445 | Windows file and printer sharing |

Older protocols helped me understand why encryption matters. When I find an open port, I still need to identify the real service, version, configuration and what information it exposes.