# Nmap Host Discovery

Host discovery answers an important first question: which systems appear to be alive?

## ICMP options I studied

```bash
nmap -PE TARGET
nmap -PP TARGET
nmap -PM TARGET
```

- `-PE`: ICMP Echo
- `-PP`: ICMP Timestamp
- `-PM`: ICMP Address Mask

I have also studied TCP-based discovery such as SYN ping. One important lesson is that a host that does not answer one discovery method is not automatically offline; filtering can change what responses I see.