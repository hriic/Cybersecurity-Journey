# Linux for Cybersecurity

Linux is the environment I use most often for hands-on security labs, mainly through Kali Linux, with additional exploration of Parrot OS. My goal is terminal fluency and understanding the operating system underneath the tools.

## Filesystem navigation

```bash
pwd
ls -la
cd /path
cat file
less file
find / -name "name" 2>/dev/null
grep -i "text" file
```

`pwd` shows where I am, `ls -la` includes hidden files and permissions, `cat/less` inspect content, `find` searches the filesystem and `grep` searches text. These become enumeration primitives when I need to understand an unfamiliar system.

## Users, groups and permissions

Linux permissions are represented for owner, group and others with read (`r`), write (`w`) and execute (`x`) bits. I use commands such as `id`, `whoami`, `ls -l` and `chmod` to understand identity and access.

Permissions matter in security because the same file or service can be harmless for one user and sensitive for another. I want to understand *why* access is allowed before thinking about privilege escalation.

## Processes and services

```bash
ps aux
ss -tulpn
```

`ps aux` gives me a broad process view. `ss -tulpn` helps identify listening TCP/UDP sockets and, when permissions allow, associated processes. This connects local enumeration back to networking.

## Networking from Linux

I regularly use `ip`, `ss`, `curl`, `wget` and `ssh`. These let me inspect interfaces/routes, identify listeners, interact with web services, retrieve resources and administer authorized systems remotely.

## Shells, pipes and redirection

The shell becomes much more useful when commands are combined. Pipes (`|`) send one command's output into another; `>` replaces a file and `>>` appends. This is important for filtering large outputs and building repeatable workflows.

## Kernel

The kernel is the core layer between software and hardware/resources. `uname -r` shows the running kernel release. Kernel information is one part of system enumeration, but a version number alone is not enough to conclude that a system is vulnerable.

## Package management

On Debian-derived systems such as Kali and Parrot, APT is used for package management. I use the distribution repositories rather than treating random scripts from the internet as trusted by default.

## Security mindset

Linux skill for me is not memorizing 100 commands. It is being able to arrive on a system, answer **who am I, where am I, what is running, what is listening, what can I access, and what evidence should I investigate next?**