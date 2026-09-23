# NSE and Nmap Output

NSE scripts in my training environment are stored under:

```text
/usr/share/nmap/scripts/
```

I have practiced finding scripts based on the service or check I am investigating, including HTTP and SSH-related scripts.

## Saving results

```bash
nmap -oG scan.txt TARGET
nmap -oX scan.xml TARGET
```

Saving scan output lets me review findings later instead of repeatedly running the same discovery work.