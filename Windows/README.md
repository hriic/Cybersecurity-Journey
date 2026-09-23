# Windows & Windows Server Fundamentals

Windows knowledge matters to my penetration-testing direction because enterprise environments frequently combine Windows endpoints, Windows Server and Active Directory. I am learning normal administration first so security behavior has context.

## Windows fundamentals

I have studied the filesystem, users/groups, permissions, services, processes, system configuration and common administrative concepts. The important security lesson is that identity, privileges and service configuration determine what a user or process can actually do.

## PowerShell

PowerShell is both a shell and an automation/scripting environment built around objects. I have practiced basic navigation and administration concepts and want to become comfortable reading PowerShell instead of treating it as a collection of copied commands.

For security work, PowerShell knowledge helps me understand legitimate administration as well as activity defenders may see in enterprise logs.

## Windows services

Services are background components managed by the operating system. During system analysis I care about what a service does, which account it runs as, whether it starts automatically, what it listens on and how it is configured.

## Windows Server lab

I have used Windows Server in a virtual lab to practice roles and infrastructure rather than learning Active Directory only from attack tools.

### DHCP
DHCP automatically supplies network configuration such as IP address, subnet mask, gateway and DNS settings. Understanding the normal lease process helps me understand how Windows clients join and operate on a network.

### RRAS / NAT
I have practiced Routing and Remote Access/NAT concepts. NAT translates addresses between networks, while routing decides where packets should be forwarded. This gave me practical context for multi-network lab setups.

### Active Directory
I have studied domains, Domain Controllers, users, groups, authentication, permissions and Group Policy. I keep the deeper AD notes in [Active Directory](../Active-Directory/).

## What I look for when learning Windows

I try to connect every concept to four questions: **what is the normal administrative purpose, where is it configured, what permissions protect it, and what evidence would appear if it were misused?**