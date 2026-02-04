# Active Directory Fundamentals and Penetration Testing Essentials

Research shows that **90% of all Fortune 500 companies** use Microsoft Active Directory as their go-to identity and access management service. Here’s why AD is the industry standard and why every pentester must master it.

---

## What is Active Directory?
Active Directory (AD) is an Identity and Access Management (IAM) service responsible for managing authentication and authorization across a network. It allows administrators to:
* **Manage Objects:** Users, groups, and devices.
* **Define Access Rights:** Control who sees what.
* **Apply Security Policies:** Consistent rules across a domain or forest.

In modern enterprise environments, AD is the **central nervous system**. Every login, access decision, and policy enforcement flows through it. For a penetration tester, understanding AD is foundational.

---

## Active Directory as a Trust System
Administrators see a directory; attackers see a **trust system**. AD continuously answers four critical questions:
1.  **Who** you are (Identity)
2.  **How** you authenticate (Mechanism)
3.  **What** you are allowed to do (Authorization)
4.  **Where** you are allowed to do it (Scope)

---

## Understanding Active Directory Structure

### 1. Organizational Units (OUs)
OUs are containers used for Group Policy application and administrative delegation. 
* **The Risk:** Write access to an OU allows an attacker to modify Group Policy Objects (GPOs), reset passwords, or deploy startup scripts that execute code across all systems in that OU.

### 2. Domain
The primary operational unit. It defines authentication realms and hosts Domain Controllers (DCs).
* **The Risk:** Domains are where credentials concentrate. Common attacks like **Kerberoasting**, **AS-REP Roasting**, and **NTLM Relay** operate primarily within the domain context.

### 3. Tree
A collection of domains connected hierarchically sharing a common DNS namespace.
* **The Risk:** Parent-child relationships establish two-way transitive trusts. Attackers often escalate in a less-secure child domain to move laterally into high-value parent domains.

### 4. Forest
The highest-level construct and the **maximum security boundary**. 
* **The Risk:** Once an attacker gains control of forest-level groups (like **Enterprise Admins**), they effectively own the entire identity infrastructure.

---

## Security Groups vs. Organizational Units

| Feature | Organizational Unit (OU) | Security Group |
| :--- | :--- | :--- |
| **Primary Purpose** | Administrative & Policy Management | Access Control & Permissions |
| **GPO Application** | Yes | No |
| **Membership** | Objects reside in only **one** OU | Objects can belong to **multiple** groups |
| **Pentester Focus** | Delegation & GPO Injection | Privilege Escalation & Lateral Movement |

---

## Core Active Directory Services

* **AD DS (Domain Services):** The core store for objects and authentication.
* **AD CS (Certificate Services):** Manages PKI and digital certificates. *Critical for ESC1-ESC8 impersonation attacks.*
* **AD LDS (Lightweight Directory Services):** A standalone directory for apps that don't need the full domain structure.
* **AD FS (Federation Services):** Enables Single Sign-On (SSO) across different systems.
* **AD RMS (Rights Management Services):** Protects data integrity by enforcing usage restrictions (no copy/print).

---

## Authentication Protocols

### 1. LM and NTLM (Legacy)
* **LM:** Obsolete and highly vulnerable. Uses DES encryption and uppercase hashing.
* **NTLM/NTLMv2:** Challenge-response protocols. Susceptible to **Relay Attacks** where an attacker intercepts a challenge and forwards it to another server to authenticate as the victim.

### 2. Kerberos (The Standard)
A ticket-based system providing mutual authentication. 
* **The Flow:**
    1.  **AS-REQ:** User requests authentication.
    2.  **TGT:** KDC issues a Ticket Granting Ticket.
    3.  **TGS-REQ:** User requests a Service Ticket (ST) for a resource.
    4.  **Access:** User presents the ST to the target server.

### 3. LDAP (Lightweight Directory Access Protocol)
The language used to query the directory.
* **Pentester's View:** Tools like **BloodHound** use LDAP to map out attack paths to the Domain Admin.

---

## Conclusion
For a penetration tester, AD mastery transforms a complex network into a predictable system of trust relationships. By understanding how OUs, GPOs, and Kerberos tickets interact, you can map, abuse, and ultimately secure the most critical infrastructure in the corporate world.
