# Active Directory Fundamentals

Active Directory (AD) is a major focus for my long-term penetration-testing and Red Team path. Before learning advanced AD attacks, I am building a clear model of how a Windows domain is supposed to work.

## What Active Directory does

AD Domain Services provides centralized identity and resource management. Instead of managing every computer independently, an organization can manage users, computers, groups and policies within a domain.

## Domain and Domain Controller

A **domain** is an administrative/security boundary containing directory objects. A **Domain Controller (DC)** runs AD DS and participates in authentication and directory services. In a domain environment, the DC is therefore much more than "another Windows server."

## Users, computers and groups

Users represent identities; computer accounts represent domain-joined machines; groups make permissions manageable at scale. I am learning to think in terms of group membership and effective permissions rather than assuming that a username alone describes a user's access.

## Authentication

I have studied AD authentication at a foundational level and am building toward deeper understanding of Kerberos and NTLM. The key idea is that authentication proves identity while authorization determines what that identity can access.

## Group Policy

Group Policy allows centralized configuration of users and computers. Policies can affect security settings, system behavior and enterprise configuration, which makes understanding GPOs important both for administration and security assessment.

## Permissions

AD security depends heavily on permissions and relationships between objects. My goal is to become comfortable reading these relationships before moving into attack-path tooling. Misconfiguration is often more important than a flashy exploit.

## Windows Server practice

My lab work has included Windows Server, AD fundamentals, DHCP and RRAS/NAT. Building the environment gives me context for what I later enumerate during an internal assessment.

## Direction from here

My next layers are deeper Kerberos/NTLM knowledge, LDAP, SMB in domain environments, AD enumeration, common misconfigurations, privilege relationships and eventually authorized attack-path practice.

The principle I want to keep is: **understand the domain before trying to attack the domain.**